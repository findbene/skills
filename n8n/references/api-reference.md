# n8n REST API Reference

## Base URL
```
http://localhost:5678/api/v1    (self-hosted default)
```

## Authentication
```bash
# Basic Auth (default)
curl -u admin:password http://localhost:5678/api/v1/workflows

# API Key (recommended for automation)
# Generate: Settings -> API -> Create API Key
curl -H "X-N8N-API-KEY: your-api-key" http://localhost:5678/api/v1/workflows
```

---

## Workflow Endpoints

### List All Workflows
```bash
GET /workflows
GET /workflows?active=true          # filter active only
GET /workflows?tags=production      # filter by tag
```

### Get Workflow
```bash
GET /workflows/{id}
```

### Create Workflow
```bash
POST /workflows
Content-Type: application/json

{
  "name": "My Workflow",
  "nodes": [...],
  "connections": {...},
  "active": false,
  "settings": {
    "errorWorkflow": "workflow-id-of-error-handler",
    "saveDataSuccessExecution": "all",
    "saveDataErrorExecution": "all",
    "executionTimeout": 300
  }
}
```

### Update Workflow
```bash
PUT /workflows/{id}
Content-Type: application/json
{ ...full workflow object... }
```

### Activate / Deactivate Workflow
```bash
PATCH /workflows/{id}
{ "active": true }

PATCH /workflows/{id}
{ "active": false }
```

### Delete Workflow
```bash
DELETE /workflows/{id}
```

---

## Execution Endpoints

### List Executions
```bash
GET /executions
GET /executions?workflowId={id}
GET /executions?status=error        # error | success | waiting
GET /executions?limit=50&cursor=xxx
```

### Get Execution Details
```bash
GET /executions/{id}
GET /executions/{id}?includeData=true    # include full execution data
```

### Delete Execution
```bash
DELETE /executions/{id}
```

### Run Workflow Manually (via API)
```bash
POST /workflows/{id}/run
Content-Type: application/json

{
  "runData": {
    "Webhook": [
      {
        "json": { "input": "test data" }
      }
    ]
  }
}
```

---

## Credentials Endpoints

### List Credentials
```bash
GET /credentials
GET /credentials?type=httpBasicAuth
```

### Create Credentials
```bash
POST /credentials
Content-Type: application/json

{
  "name": "My API Key",
  "type": "httpHeaderAuth",
  "data": {
    "name": "Authorization",
    "value": "Bearer sk-xxx"
  }
}
```

---

## Webhook Trigger Patterns

### Webhook Node Configuration
```json
{
  "type": "n8n-nodes-base.webhook",
  "parameters": {
    "httpMethod": "POST",
    "path": "pipeline-complete",
    "responseMode": "responseNode",
    "options": {
      "allowedOrigins": "*"
    }
  }
}
```

### Webhook URL format
```
Production:  http://localhost:5678/webhook/pipeline-complete
Test:        http://localhost:5678/webhook-test/pipeline-complete
```

### Respond to Webhook Node
```json
{
  "type": "n8n-nodes-base.respondToWebhook",
  "parameters": {
    "respondWith": "json",
    "responseBody": "={{ JSON.stringify({ status: 'ok', received: $json }) }}",
    "options": {
      "responseCode": 200
    }
  }
}
```

---

## n8n Python Client (for programmatic control)
```python
import requests
import os

class N8nClient:
    def __init__(self, base_url: str, api_key: str):
        self.base_url = base_url.rstrip('/')
        self.headers = {"X-N8N-API-KEY": api_key, "Content-Type": "application/json"}

    def trigger_webhook(self, path: str, data: dict) -> dict:
        url = f"{self.base_url}/webhook/{path}"
        response = requests.post(url, json=data, headers=self.headers)
        response.raise_for_status()
        return response.json()

    def get_workflow(self, workflow_id: str) -> dict:
        url = f"{self.base_url}/api/v1/workflows/{workflow_id}"
        response = requests.get(url, headers=self.headers)
        response.raise_for_status()
        return response.json()

    def activate_workflow(self, workflow_id: str) -> dict:
        url = f"{self.base_url}/api/v1/workflows/{workflow_id}"
        response = requests.patch(url, json={"active": True}, headers=self.headers)
        response.raise_for_status()
        return response.json()

# Usage (AI Agency)
n8n = N8nClient("http://localhost:5678", os.environ["N8N_API_KEY"])
n8n.trigger_webhook("pipeline-complete", {
    "run_id": "abc123",
    "status": "success",
    "video_count": 9,
    "avg_quality_score": 7.8
})
```
