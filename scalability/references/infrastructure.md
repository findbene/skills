# Infrastructure Scaling

## Kubernetes Horizontal Pod Autoscaler (HPA)

### Basic CPU-Based HPA
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: api-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: api-deployment
  minReplicas: 2
  maxReplicas: 20
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70    # scale up when CPU > 70%
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 80
  behavior:
    scaleUp:
      stabilizationWindowSeconds: 60     # wait 60s before scaling up
      policies:
      - type: Percent
        value: 100                        # double pods at most
        periodSeconds: 60
    scaleDown:
      stabilizationWindowSeconds: 300    # wait 5min before scaling down
      policies:
      - type: Percent
        value: 25                         # remove 25% of pods at a time
        periodSeconds: 60
```

### Custom Metrics HPA (Queue Length)
```yaml
# Scale based on Celery queue depth
metrics:
- type: External
  external:
    metric:
      name: celery_queue_length
      selector:
        matchLabels:
          queue: video-generation
    target:
      type: AverageValue
      averageValue: "10"   # scale to keep queue at 10 items per pod
```

### Deployment Configuration for HPA
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-deployment
spec:
  replicas: 2    # initial; HPA overrides this
  selector:
    matchLabels:
      app: api
  template:
    spec:
      containers:
      - name: api
        image: ai-agency-api:latest
        resources:
          requests:
            cpu: "250m"       # REQUIRED for HPA to work
            memory: "256Mi"
          limits:
            cpu: "1000m"
            memory: "512Mi"
        readinessProbe:
          httpGet:
            path: /health
            port: 8000
          initialDelaySeconds: 10
          periodSeconds: 5
        livenessProbe:
          httpGet:
            path: /health
            port: 8000
          initialDelaySeconds: 30
          periodSeconds: 10
```

---

## AWS Auto Scaling Groups

### Launch Template
```python
import boto3
import base64

ec2 = boto3.client('ec2')

# Create launch template
ec2.create_launch_template(
    LaunchTemplateName='api-server',
    LaunchTemplateData={
        'ImageId': 'ami-0abcdef',
        'InstanceType': 't3.medium',
        'SecurityGroupIds': ['sg-xxx'],
        'UserData': base64.b64encode("""#!/bin/bash
cd /app
source .env
uvicorn main:app --host 0.0.0.0 --port 8000
""".encode()).decode(),
        'TagSpecifications': [{
            'ResourceType': 'instance',
            'Tags': [
                {'Key': 'Environment', 'Value': 'prod'},
                {'Key': 'AutoShutdown', 'Value': 'false'}
            ]
        }]
    }
)

# Create Auto Scaling Group
autoscaling = boto3.client('autoscaling')
autoscaling.create_auto_scaling_group(
    AutoScalingGroupName='api-asg',
    LaunchTemplate={
        'LaunchTemplateName': 'api-server',
        'Version': '$Latest'
    },
    MinSize=2,
    MaxSize=20,
    DesiredCapacity=2,
    TargetGroupARNs=['arn:aws:elasticloadbalancing:...'],
    HealthCheckType='ELB',
    HealthCheckGracePeriod=300,
)

# Add scaling policy
autoscaling.put_scaling_policy(
    AutoScalingGroupName='api-asg',
    PolicyName='cpu-scale-out',
    PolicyType='TargetTrackingScaling',
    TargetTrackingConfiguration={
        'PredefinedMetricSpecification': {
            'PredefinedMetricType': 'ASGAverageCPUUtilization'
        },
        'TargetValue': 70.0  # keep CPU at 70%
    }
)
```

---

## Serverless Scaling

### AWS Lambda
```python
# Lambda scales to 1000 concurrent executions by default
# Zero cost when idle -- perfect for sporadic workloads

import boto3
import json

lambda_client = boto3.client('lambda')

# Invoke async (fire and forget)
lambda_client.invoke(
    FunctionName='process-video',
    InvocationType='Event',    # async
    Payload=json.dumps({'niche': 'ai_tools', 'script_id': 'abc'})
)

# Reserved concurrency (prevent one function from consuming all capacity)
lambda_client.put_function_concurrency(
    FunctionName='process-video',
    ReservedConcurrentExecutions=50
)

# Provisioned concurrency (eliminate cold starts for critical functions)
lambda_client.put_provisioned_concurrency_config(
    FunctionName='critical-api',
    Qualifier='prod',
    ProvisionedConcurrentExecutions=10
)
```

### Google Cloud Run
```yaml
# cloud-run-service.yaml
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
  name: api-service
spec:
  template:
    metadata:
      annotations:
        autoscaling.knative.dev/minScale: "1"    # avoid cold starts
        autoscaling.knative.dev/maxScale: "100"
        autoscaling.knative.dev/target: "80"     # target requests per instance
    spec:
      containerConcurrency: 80
      timeoutSeconds: 300
      containers:
      - image: gcr.io/project/api:latest
        resources:
          limits:
            cpu: "2"
            memory: "1Gi"
```

---

## Stateless Design for Scaling

### Externalizing State
```python
# WRONG: State stored in process memory (doesn't scale)
sessions = {}  # dies when process restarts

# RIGHT: State in Redis (survives restarts, shared across instances)
import redis
import json
import os

r = redis.Redis.from_url(os.environ["REDIS_URL"])

def get_session(session_id: str) -> dict:
    data = r.get(f"session:{session_id}")
    return json.loads(data) if data else {}

def set_session(session_id: str, data: dict, ttl: int = 3600):
    r.setex(f"session:{session_id}", ttl, json.dumps(data))
```

### File Storage Externalization
```python
# WRONG: Files on local disk (not shared across instances)
with open(f"/tmp/video_{id}.mp4", "wb") as f:
    f.write(video_data)

# RIGHT: S3 (accessible from any instance)
import boto3
import os

s3 = boto3.client('s3')

s3.put_object(
    Bucket=os.environ["S3_BUCKET"],
    Key=f"videos/{id}.mp4",
    Body=video_data,
    ContentType="video/mp4"
)

# Generate pre-signed URL for temporary access
url = s3.generate_presigned_url(
    'get_object',
    Params={'Bucket': os.environ["S3_BUCKET"], 'Key': f"videos/{id}.mp4"},
    ExpiresIn=3600
)
```
