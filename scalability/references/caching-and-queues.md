# Caching & Message Queues

## Caching Strategies

### Cache-Aside (Lazy Loading)
```python
import redis
import json
from typing import Optional

r = redis.Redis()

def get_user(user_id: str) -> dict:
    # 1. Check cache
    cached = r.get(f"user:{user_id}")
    if cached:
        return json.loads(cached)

    # 2. Cache miss — fetch from DB
    user = db.query(User).filter(User.id == user_id).first()

    # 3. Write to cache
    r.setex(f"user:{user_id}", 3600, json.dumps(user.to_dict()))

    return user.to_dict()
```
Best for: read-heavy data, tolerate slightly stale data.

### Write-Through Cache
```python
def update_user(user_id: str, data: dict) -> dict:
    # 1. Write to DB
    db.query(User).filter(User.id == user_id).update(data)
    db.commit()

    # 2. Immediately update cache
    r.setex(f"user:{user_id}", 3600, json.dumps(data))

    return data
```
Best for: frequently read data that's also frequently written.

### Cache Invalidation Patterns
```python
# Pattern: invalidate on write
def delete_user(user_id: str):
    db.query(User).filter(User.id == user_id).delete()
    db.commit()
    r.delete(f"user:{user_id}")  # remove from cache

# Pattern: versioned cache keys
def get_user_v2(user_id: str, version: int) -> dict:
    key = f"user:{user_id}:v{version}"
    ...

# Pattern: tag-based invalidation
def invalidate_user_data(user_id: str):
    keys = r.keys(f"user:{user_id}:*")
    if keys:
        r.delete(*keys)
```

### CDN Caching
```python
# FastAPI: set Cache-Control headers
from fastapi import Response

@app.get("/api/public/content/{id}")
async def get_public_content(id: str, response: Response):
    response.headers["Cache-Control"] = "public, max-age=3600, s-maxage=86400"
    response.headers["Vary"] = "Accept-Encoding"
    return get_content(id)

# For dynamic content that varies by user:
response.headers["Cache-Control"] = "private, no-cache"
```

CDN header strategy:
- Static assets (JS, CSS, images): `max-age=31536000, immutable` (1 year)
- Public API responses: `s-maxage=300` (CDN cache 5min, browser no cache)
- Private/user-specific: `private, no-store`

---

## Message Queues

### Redis Queue (Simple, No Dependencies)
```python
import redis
import json
import threading
from typing import Callable, Optional

class SimpleQueue:
    def __init__(self, name: str, redis_url: str = "redis://localhost:6379"):
        self.name = name
        self.r = redis.from_url(redis_url)

    def push(self, task: dict) -> None:
        self.r.lpush(self.name, json.dumps(task))

    def pop(self, timeout: int = 30) -> Optional[dict]:
        result = self.r.brpop(self.name, timeout=timeout)
        if result:
            _, data = result
            return json.loads(data)
        return None

    def worker(self, handler: Callable) -> None:
        while True:
            task = self.pop()
            if task:
                try:
                    handler(task)
                except Exception as e:
                    self.r.lpush(f"{self.name}:failed", json.dumps({
                        "task": task, "error": str(e)
                    }))

# Usage (AI Agency pipeline)
queue = SimpleQueue("video-generation")
queue.push({"niche": "ai_tools", "script_id": "abc123"})

# Worker process
def generate_video(task):
    run_video_pipeline(task["niche"], task["script_id"])

queue.worker(generate_video)
```

### Celery (Production Queue System)
```python
# tasks.py
from celery import Celery
import os

app = Celery(
    "ai_agency",
    broker=os.environ["REDIS_URL"],
    backend=os.environ["REDIS_URL"],
    include=["tasks"]
)

app.conf.update(
    task_serializer="json",
    result_serializer="json",
    accept_content=["json"],
    task_acks_late=True,           # Only ack after successful completion
    task_reject_on_worker_lost=True,
    worker_prefetch_multiplier=1,  # One task at a time per worker
    task_routes={
        "tasks.generate_video": {"queue": "video"},    # GPU queue
        "tasks.publish_video": {"queue": "publish"},   # I/O queue
        "tasks.send_alert": {"queue": "default"},      # Fast queue
    }
)

@app.task(
    bind=True,
    max_retries=3,
    default_retry_delay=60,
    soft_time_limit=300,    # 5min soft limit (raises exception)
    time_limit=360          # 6min hard limit (kills worker)
)
def generate_video(self, niche: str, script_id: str) -> dict:
    try:
        result = run_video_pipeline(niche, script_id)
        return result
    except TemporaryError as e:
        raise self.retry(exc=e, countdown=30 * (2 ** self.request.retries))
```

```bash
# Start workers for each queue type
celery -A tasks worker -Q video --concurrency 2 --loglevel info &
celery -A tasks worker -Q publish --concurrency 4 --loglevel info &
celery -A tasks worker -Q default --concurrency 8 --loglevel info &

# Monitor
celery -A tasks flower --port=5555
```

### AWS SQS (Cloud Queue)
```python
import boto3
import json
import os

sqs = boto3.client('sqs', region_name='us-east-1')
QUEUE_URL = os.environ["SQS_QUEUE_URL"]

def enqueue_video(niche: str, script_id: str) -> str:
    response = sqs.send_message(
        QueueUrl=QUEUE_URL,
        MessageBody=json.dumps({"niche": niche, "script_id": script_id}),
        MessageGroupId="video-gen",                     # FIFO queue
        MessageDeduplicationId=f"{niche}-{script_id}",  # Dedup key
        DelaySeconds=0
    )
    return response["MessageId"]

def process_messages():
    while True:
        response = sqs.receive_message(
            QueueUrl=QUEUE_URL,
            MaxNumberOfMessages=10,
            WaitTimeSeconds=20,    # Long polling (reduces empty responses)
            VisibilityTimeout=300  # 5min to process before re-queue
        )

        for msg in response.get("Messages", []):
            task = json.loads(msg["Body"])
            try:
                process_video_task(task)
                sqs.delete_message(
                    QueueUrl=QUEUE_URL,
                    ReceiptHandle=msg["ReceiptHandle"]
                )
            except Exception as e:
                # Don't delete -- message will reappear after VisibilityTimeout
                log_error(e, task)
```

### BullMQ (Node.js Queue, for SaaS Dashboard)
```typescript
import { Queue, Worker, Job } from 'bullmq';
import IORedis from 'ioredis';

const connection = new IORedis(process.env.REDIS_URL!);

// Producer
export const videoQueue = new Queue('video-generation', { connection });

export async function enqueueVideo(niche: string, scriptId: string) {
    await videoQueue.add(
        'generate',
        { niche, scriptId },
        {
            attempts: 3,
            backoff: { type: 'exponential', delay: 5000 },
            removeOnComplete: 100,  // keep last 100 completed
            removeOnFail: 500,      // keep last 500 failed
        }
    );
}

// Consumer
const worker = new Worker(
    'video-generation',
    async (job: Job) => {
        const { niche, scriptId } = job.data;
        await job.updateProgress(10);
        const result = await runVideoPipeline(niche, scriptId);
        await job.updateProgress(100);
        return result;
    },
    { connection, concurrency: 2 }
);

worker.on('completed', (job) => console.log(`Job ${job.id} completed`));
worker.on('failed', (job, err) => console.error(`Job ${job?.id} failed:`, err));
```
