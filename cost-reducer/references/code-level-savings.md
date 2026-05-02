# Code-Level Cost Savings

## The Principle
Infrastructure cost is downstream of code decisions. Inefficient code = more compute, more I/O, more memory, more API calls.

---

## Database Query Optimization

### N+1 Query Problem
```python
# EXPENSIVE: N+1 — 1 query for users + N queries for each user's posts
users = db.query(User).all()
for user in users:
    posts = db.query(Post).filter(Post.user_id == user.id).all()  # N queries

# CHEAP: 1 query with JOIN
users = db.query(User).options(joinedload(User.posts)).all()
```

### Pagination vs Full Scan
```python
# EXPENSIVE: Load all records into memory
all_records = db.query(Record).all()
result = [r for r in all_records if r.active]

# CHEAP: Filter at database level, paginate
result = db.query(Record).filter(Record.active == True).limit(100).offset(page * 100).all()
```

### Index Coverage
- Missing index on a filtered/sorted column = full table scan on every request
- Use `EXPLAIN ANALYZE` (PostgreSQL) or `EXPLAIN` (MySQL) to detect seq scans
- Composite indexes: order matters — put high-cardinality columns first
- Covering indexes: include all columns the query needs to avoid heap fetches

---

## Caching Patterns

### Cache What's Expensive to Compute
```python
import redis
import json
from functools import wraps

redis_client = redis.Redis()

def cache(ttl_seconds=300):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            key = f"{func.__name__}:{hash(str(args) + str(kwargs))}"
            cached = redis_client.get(key)
            if cached:
                return json.loads(cached)
            result = func(*args, **kwargs)
            redis_client.setex(key, ttl_seconds, json.dumps(result))
            return result
        return wrapper
    return decorator

@cache(ttl_seconds=3600)
def expensive_computation(user_id: int):
    ...
```

### Cache Hierarchy (fastest to slowest)
1. **In-process memory** (dict/lru_cache) — microseconds, no network
2. **Redis/Memcached** — milliseconds, shared across instances
3. **CDN cache** — tens of milliseconds, reduces origin load
4. **Database query cache** — last resort, limited effectiveness

### LRU Cache for Pure Functions
```python
from functools import lru_cache

@lru_cache(maxsize=1000)
def compute_score(inputs: tuple) -> float:
    # Expensive ML inference or calculation
    ...
```

---

## API Call Optimization

### Batch Over Loop
```python
# EXPENSIVE: 1 API call per item
for item_id in item_ids:
    result = api.get_item(item_id)  # 100 API calls

# CHEAP: 1 batch API call
results = api.batch_get_items(item_ids)  # 1 API call
```

### Rate Limit Management
```python
import time
from collections import deque

class RateLimiter:
    def __init__(self, max_calls: int, period: float):
        self.max_calls = max_calls
        self.period = period
        self.calls = deque()

    def wait_if_needed(self):
        now = time.time()
        # Remove calls outside the window
        while self.calls and self.calls[0] < now - self.period:
            self.calls.popleft()
        if len(self.calls) >= self.max_calls:
            sleep_time = self.period - (now - self.calls[0])
            time.sleep(max(0, sleep_time))
        self.calls.append(time.time())
```

### LLM API Cost Reduction
- **Cache repeated prompts**: Hash the prompt, store response in Redis (TTL = 24h for stable facts)
- **Use cheaper models for simple tasks**: GPT-4o-mini for classification, Claude Haiku for formatting
- **Reduce token count**: Compress system prompts, remove redundant examples
- **Stream for UX, don't store streams**: Streaming doesn't reduce tokens but improves perceived speed
- **Prompt compression**: Use techniques like LLMLingua to compress prompts 3–5x while preserving meaning

```python
# Model selection by task complexity
def select_model(task_type: str) -> str:
    routing = {
        "classification": "claude-haiku-4-5",       # $0.25/MTok input
        "summarization": "claude-haiku-4-5",
        "generation": "claude-sonnet-4-6",           # $3/MTok input
        "complex_reasoning": "claude-opus-4-6",      # $15/MTok input
    }
    return routing.get(task_type, "claude-sonnet-4-6")
```

---

## I/O and Memory Optimization

### Stream Large Files (Don't Load Into Memory)
```python
# EXPENSIVE: Load entire file into memory
with open("large_file.csv") as f:
    data = f.read()  # Could be GB

# CHEAP: Stream line by line
with open("large_file.csv") as f:
    for line in f:  # Constant memory usage
        process(line)

# For S3 objects
import boto3
s3 = boto3.client('s3')
response = s3.get_object(Bucket='my-bucket', Key='large-file.csv')
for line in response['Body'].iter_lines():
    process(line)
```

### Async I/O for Network-Bound Work
```python
import asyncio
import aiohttp

# EXPENSIVE: Sequential API calls (10 calls x 100ms each = 1000ms)
for url in urls:
    response = requests.get(url)

# CHEAP: Concurrent API calls (10 calls x 100ms each ~= 100ms total)
async def fetch_all(urls):
    async with aiohttp.ClientSession() as session:
        tasks = [session.get(url) for url in urls]
        return await asyncio.gather(*tasks)
```

### Generator Over List Comprehension
```python
# EXPENSIVE: Creates full list in memory
results = [process(item) for item in huge_dataset]

# CHEAP: Lazy evaluation — each item processed on demand
results = (process(item) for item in huge_dataset)
```

---

## Compute Optimization

### Profile Before Optimizing
```python
import cProfile
import pstats

profiler = cProfile.Profile()
profiler.enable()
your_function()
profiler.disable()

stats = pstats.Stats(profiler)
stats.sort_stats('cumulative')
stats.print_stats(20)  # Top 20 expensive functions
```

### Vectorization (NumPy/Pandas)
```python
# SLOW: Python loop (interpreted)
result = []
for x in data:
    result.append(x * 2 + 1)

# FAST: NumPy vectorized (C-level)
import numpy as np
result = np.array(data) * 2 + 1  # 10-100x faster
```

### Connection Pooling
```python
# EXPENSIVE: New connection per request
def query():
    conn = psycopg2.connect(DATABASE_URL)  # ~50-200ms per connection
    cursor = conn.cursor()
    ...

# CHEAP: Reuse connections from pool
from psycopg2 import pool

connection_pool = pool.ThreadedConnectionPool(
    minconn=2, maxconn=20, dsn=DATABASE_URL
)

def query():
    conn = connection_pool.getconn()  # ~0ms (reuse)
    ...
    connection_pool.putconn(conn)
```
