# API Scaling & Service Architecture

## Rate Limiting

### Token Bucket Algorithm
```python
import time
import threading

class TokenBucket:
    """Thread-safe token bucket rate limiter."""

    def __init__(self, rate: float, capacity: float):
        self.rate = rate          # tokens per second
        self.capacity = capacity  # max tokens
        self.tokens = capacity
        self.last_refill = time.monotonic()
        self._lock = threading.Lock()

    def consume(self, tokens: int = 1) -> bool:
        with self._lock:
            now = time.monotonic()
            elapsed = now - self.last_refill
            self.tokens = min(
                self.capacity,
                self.tokens + elapsed * self.rate
            )
            self.last_refill = now

            if self.tokens >= tokens:
                self.tokens -= tokens
                return True
            return False

# Usage
limiter = TokenBucket(rate=100, capacity=100)  # 100 req/sec
if not limiter.consume():
    raise Exception("Too many requests")
```

### Distributed Rate Limiting (Redis)
```python
import redis
import time

def is_rate_limited(client_id: str, limit: int, window_seconds: int) -> bool:
    """Sliding window rate limit using Redis."""
    r = redis.Redis()
    key = f"rate_limit:{client_id}"
    now = time.time()
    window_start = now - window_seconds

    pipe = r.pipeline()
    # Remove old requests outside window
    pipe.zremrangebyscore(key, 0, window_start)
    # Count requests in window
    pipe.zcard(key)
    # Add current request
    pipe.zadd(key, {str(now): now})
    # Set expiry
    pipe.expire(key, window_seconds)

    results = pipe.execute()
    request_count = results[1]

    return request_count >= limit
```

### Rate Limiting Headers
```python
from fastapi import Request, Response

@app.middleware("http")
async def rate_limit_middleware(request: Request, call_next):
    client_id = request.headers.get("X-API-Key") or request.client.host

    if is_rate_limited(client_id, limit=100, window_seconds=60):
        return Response(
            status_code=429,
            headers={
                "Retry-After": "60",
                "X-RateLimit-Limit": "100",
                "X-RateLimit-Remaining": "0",
                "X-RateLimit-Reset": str(int(time.time()) + 60)
            }
        )

    response = await call_next(request)
    return response
```

---

## Load Balancing

### Nginx Load Balancer Configuration
```nginx
upstream api_servers {
    least_conn;           # route to server with fewest active connections

    server api1:8000 weight=3;   # gets 3x more traffic
    server api2:8000 weight=1;
    server api3:8000 backup;     # only used when others are down

    keepalive 32;         # persistent connections to backends
}

server {
    listen 80;

    location /api/ {
        proxy_pass http://api_servers;
        proxy_http_version 1.1;
        proxy_set_header Connection "";
        proxy_connect_timeout 5s;
        proxy_read_timeout 30s;

        # Health check
        health_check interval=10s fails=3 passes=2;
    }
}
```

Load balancing algorithms:
- `round_robin` (default) — rotate evenly
- `least_conn` — route to least busy server
- `ip_hash` — same client always goes to same server (sticky sessions)
- `random` — random selection

### Application-Level Load Balancing
```python
import itertools
import random
from typing import List

class RoundRobinBalancer:
    def __init__(self, servers: List[str]):
        self._cycle = itertools.cycle(servers)

    def next(self) -> str:
        return next(self._cycle)

class WeightedBalancer:
    def __init__(self, servers: List[tuple]):
        # servers = [("server1", weight1), ("server2", weight2)]
        self._pool = [s for s, w in servers for _ in range(w)]

    def next(self) -> str:
        return random.choice(self._pool)
```

---

## Service Mesh Patterns

### Circuit Breaker
```python
import time
from enum import Enum

class CircuitState(Enum):
    CLOSED = "closed"      # normal operation
    OPEN = "open"          # failing, reject requests
    HALF_OPEN = "half_open"  # testing recovery

class CircuitBreaker:
    def __init__(self, failure_threshold: int = 5, timeout: float = 60.0):
        self.failure_threshold = failure_threshold
        self.timeout = timeout
        self.failure_count = 0
        self.state = CircuitState.CLOSED
        self.last_failure_time = None

    def call(self, func, *args, **kwargs):
        if self.state == CircuitState.OPEN:
            if time.time() - self.last_failure_time > self.timeout:
                self.state = CircuitState.HALF_OPEN
            else:
                raise Exception("Circuit is open")

        try:
            result = func(*args, **kwargs)
            self._on_success()
            return result
        except Exception as e:
            self._on_failure()
            raise

    def _on_success(self):
        self.failure_count = 0
        self.state = CircuitState.CLOSED

    def _on_failure(self):
        self.failure_count += 1
        self.last_failure_time = time.time()
        if self.failure_count >= self.failure_threshold:
            self.state = CircuitState.OPEN
```

### Retry with Exponential Backoff
```python
import time
import random
from functools import wraps

def retry(max_attempts=3, base_delay=1.0, max_delay=60.0, jitter=True):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            for attempt in range(max_attempts):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    if attempt == max_attempts - 1:
                        raise
                    delay = min(base_delay * (2 ** attempt), max_delay)
                    if jitter:
                        delay *= (0.5 + random.random())
                    time.sleep(delay)
        return wrapper
    return decorator

@retry(max_attempts=3, base_delay=2.0)
def call_external_api(data: dict) -> dict:
    ...
```
