---
name: scalability
description: "Scalable system architecture design covering load balancing, caching strategies, horizontal scaling, database sharding, and high-availability patterns. Use this skill any time a system needs to scale, performance bottlenecks need to be addressed architecturally, horizontal scaling strategies need to be designed, or high-availability systems need to be built. Trigger immediately on: \"scalability\", \"scale this\", \"handle more traffic\", \"bottleneck\", \"load balancing\", \"horizontal scaling\", \"caching strategy\", \"database sharding\", \"high availability\", \"performance at scale\", \"scaling architecture\", \"auto-scaling\", \"CDN\", \"microservices scaling\". If someone says \"this needs to scale\" or \"we are getting too much traffic\" this skill MUST trigger."
version: 1.0.0
triggers:
  - scalability
  - scale the system
  - handle more load
  - high traffic
  - throughput
  - performance bottleneck
  - load balancing
  - horizontal scaling
  - vertical scaling
  - autoscaling
  - rate limiting
  - queue system
references:
  - references/api-and-services.md
  - references/caching-and-queues.md
  - references/database-scaling.md
  - references/infrastructure.md
---

# Scalability Skill

## Purpose
Design and implement system architectures that handle increasing load gracefully — from 10 to 10,000 requests per second — without requiring full rewrites.

## Activation
Load this skill when the user asks about:
- System performance under load
- Scaling to handle more users, requests, or data volume
- Throughput bottlenecks and how to remove them
- Queue systems, async processing, and event-driven architecture
- Database scaling strategies (read replicas, sharding, caching)
- Infrastructure autoscaling (Kubernetes HPA, AWS ASG, Cloud Run)
- API rate limiting and load balancing

## The Scalability Ladder

Before suggesting any scaling approach, identify where the system currently sits:

```
Level 1: Single machine, single process
Level 2: Multi-process / multi-threaded (vertical scale)
Level 3: Load-balanced multiple instances (horizontal scale)
Level 4: Async workers + queue system (decoupled scale)
Level 5: Microservices + event bus (independent scale per service)
Level 6: Global distribution (CDN, multi-region, edge compute)
```

Most systems only need to climb to Level 3-4. Over-engineering to Level 6 for a product with 1,000 users is a waste.

## Bottleneck Identification Protocol

Before scaling anything:
1. **Profile first** — measure response times end-to-end; identify the slowest component
2. **Find the constraint** — is it CPU, memory, I/O, network, or DB connections?
3. **Remove the constraint** — optimize the specific bottleneck, not the whole system
4. **Repeat** — the next bottleneck will surface

```python
# Quick response time profiling
import time
import logging

def timed(func):
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        elapsed = (time.perf_counter() - start) * 1000
        logging.info(f"{func.__name__} took {elapsed:.2f}ms")
        return result
    return wrapper
```

## Decision Framework

| Problem | Solution |
|---|---|
| Slow DB queries | Index optimization -> query rewrite -> read replica |
| High API latency | Caching -> async processing -> horizontal scaling |
| Repeated work | Caching (in-memory -> Redis -> CDN) |
| Spiky traffic | Queue + workers (decouple ingestion from processing) |
| DB connection exhaustion | Connection pooling (PgBouncer) |
| Single node limits | Horizontal scaling + load balancer |
| Stateful scaling issues | Externalize state (Redis sessions, S3 files) |
| Unpredictable load | Autoscaling (K8s HPA, AWS ASG, serverless) |

## Key Principles
- **Scale out before up** — horizontal scaling is usually cheaper and more resilient than bigger machines
- **Stateless services scale easily** — externalize all state (sessions -> Redis, files -> S3)
- **Async decouple systems** — queues prevent one slow component from blocking everything
- **Cache aggressively** — if it's read more than written, it should be cached
- **Graceful degradation** — design for partial failure; serve cached content if DB is slow
