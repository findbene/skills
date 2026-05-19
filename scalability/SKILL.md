---
name: scalability
description: "Scalable system architecture design covering load balancing, caching strategies, horizontal scaling, database sharding, and high-availab. Triggers: 'use scalability', 'scalability', 'scalability task."
allowed-tools: Glob, Grep, Read
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
1. **Profile first** — measure response times end-to-end; identify the slowest component → verify: step output matches expected outcome
2. **Find the constraint** — is it CPU, memory, I/O, network, or DB connections? → verify: step output matches expected outcome
3. **Remove the constraint** — optimize the specific bottleneck, not the whole system → verify: step output matches expected outcome
4. **Repeat** — the next bottleneck will surface → verify: step output matches expected outcome

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

## Triggers

\\\"scalability\\\", \\\"scale this\\\", \\\"handle more traffic\\\", \\\"bottleneck\\\", \\\"load balancing\\\", \\\"horizontal scaling\\\", \\\"caching strategy\\\", \\\"database sharding\\\", \\\"high availability\\\", ...

## When NOT to use

- Greenfield architecture decisions — use `senior-architect`
- Single-process performance tuning — use `performance-profiler`
- Database schema design only — use `database-designer` / `database-schema-designer`
- Cloud cost optimization — use `cost-reducer`
- Premature scaling for systems under 10 req/s — usually a vertical-scale-and-monitor situation

## Red Flags

| Rationalization | Reality |
|---|---|
| "We need microservices to scale" | Modular monolith scales to 10k req/s with good caching; microservices add ops burden, not performance |
| "Cache everything aggressively" | Stale data + cache stampedes can be worse than no cache; cache with eviction + stampede protection |
| "Sharding will solve it" | Sharding adds query complexity and rebalancing pain; exhaust vertical scale + read replicas first |
| "Autoscale will handle the spike" | Cold start + warm-up windows often miss the spike entirely; pre-scale on known patterns |

## Output Contract

Finished output must contain:
- Current capacity measurement (req/s, p95 latency, db connections, etc.)
- Bottleneck identified with evidence (profile, slow query log, etc.)
- Scaling strategy: vertical first → read replicas → caching → horizontal → sharding (in that order, justifying each step)
- Caching plan (what to cache, TTL, invalidation strategy, stampede protection)
- Load balancing approach with health checks
- Failure modes documented (cache miss storm, db failover, autoscale lag)
- Rollback plan if scaling change degrades service

## Examples

**Example 1 — API endpoint hitting rate limits**
- Input: "Our /api/feed endpoint times out under 1000 req/s, p95 = 4s"
- Action: Profile DB → 80% time in 1 query → add covering index + read replica → add Redis cache (60s TTL with jittered expiry, stampede lock) → load test → iterate
- Output: Index migration, read-replica config, Redis cache layer with stampede lock, load-test report showing 3000 req/s sustained at p95 = 220ms

**Example 2 — Plan for 10x traffic growth**
- Input: "We expect 10x traffic in 6 months, what should we do?"
- Action: Capacity model current → identify bottlenecks (db CPU, single web tier) → propose phased plan: read replicas Q1, Redis cache Q2, horizontal web Q2, sharding deferred unless db CPU exceeds 70%
- Output: 1-page capacity plan, dated milestones, monitoring SLOs, decision tree for sharding
