---
name: trigger-dev
description: "Trigger.dev background jobs, scheduled tasks, and long-running workflow management for reliable async processing. Use this skill any time background jobs need to be built with Trigger.dev, scheduled tasks need to run on cron, long-running workflows need to be managed, or event-driven background processing needs to be implemented. Trigger immediately on: \"Trigger.dev\", \"trigger.dev\", \"background job\", \"scheduled task\", \"cron job\", \"long-running task\", \"background workflow\", \"job queue\", \"Trigger.dev task\", \"async job\", \"event-driven job\", \"background processing\", \"scheduled workflow\", \"Trigger.dev cron\". If someone says \"run this in the background\" or \"schedule this task with Trigger.dev\" this skill MUST trigger."
version: 1.0.0
triggers:
  - trigger.dev
  - triggerdev
  - background job
  - scheduled task
  - long-running task
  - job queue
  - task queue
  - background worker
  - cron job
  - retry logic
  - batch job
  - durable task
  - idempotent job
references:
  - references/core-reference.md
  - references/config-reference.md
  - references/advanced-reference.md
---

# Trigger.dev Skill

## Purpose
Design, implement, and operate background job systems using Trigger.dev v3 — the TypeScript-native platform for durable, observable, and reliable background task execution.

## Activation
Load this skill when the user asks about:
- Building background jobs or workers in TypeScript/Node.js
- Scheduled (cron) jobs and recurring tasks
- Long-running tasks exceeding HTTP timeout limits
- Job retry logic and failure handling
- Batch processing large datasets
- Concurrency control for background work
- Real-time job monitoring
- Trigger.dev-specific APIs, configuration, or deployment

## Key Concepts

### Why Trigger.dev?
| Problem | Solution |
|---|---|
| HTTP timeout (30s) | Tasks run for hours/days without timeout |
| No retry on failure | Built-in retry with exponential backoff |
| No visibility | Real-time dashboard, logs, traces |
| Manual queue setup | Zero-infrastructure job queuing |
| No concurrency control | Queue-level concurrency limits |

### Task Lifecycle
```
task.trigger() → Queue → Worker picks up → Execute →
  [Success] → Complete
  [Failure] → Retry (with backoff) → [Max retries] → Dead letter → Alert
```

### Idempotency (Critical)
Every task MUST be idempotent — safe to run multiple times:
```typescript
export const generateVideo = task({
  id: "generate-video",
  run: async (payload: { scriptId: string }) => {
    // Idempotency check first
    const existing = await db.query.videos.findFirst({
      where: eq(videos.scriptId, payload.scriptId)
    });
    if (existing?.status === "completed") return existing;

    // Upsert (not insert) handles retries safely
    await db.insert(videos)
      .values({ script_id: payload.scriptId, status: "processing" })
      .onConflictDoUpdate({ target: videos.scriptId, set: { status: "processing" } });

    const result = await callVideoAPI(payload.scriptId);
    await db.update(videos)
      .set({ status: "completed", url: result.url })
      .where(eq(videos.scriptId, payload.scriptId));

    return result;
  }
});
```

## When to Use Trigger.dev vs Alternatives

| Scenario | Best Choice |
|---|---|
| TypeScript/Node.js background jobs | Trigger.dev |
| Python background jobs | Celery + Redis |
| Simple cron jobs | n8n Schedule Trigger |
| Event-driven workflows | n8n Webhook |
| ML/GPU tasks | Modal, RunPod |
| Short serverless tasks (< 15min) | AWS Lambda |
| Long-running pipelines (hours) | Trigger.dev or AWS Step Functions |

## Basic Task
```typescript
import { task, logger } from "@trigger.dev/sdk/v3";

export const myTask = task({
  id: "my-task",
  retry: {
    maxAttempts: 3,
    factor: 2,
    minTimeoutInMs: 1000,
    maxTimeoutInMs: 30000,
    randomize: true,
  },
  machine: { preset: "small-1x" },
  maxDuration: 3600,
  run: async (payload: { niche: string; scriptId: string }, { ctx }) => {
    logger.info("Starting", { scriptId: payload.scriptId, attempt: ctx.attempt.number });
    const result = await generateVideo(payload);
    return result;
  },
});
```

## Triggering
```typescript
// Fire and forget
const handle = await myTask.trigger({ niche: "ai_tools", scriptId: "abc" });

// Wait for result
const result = await myTask.triggerAndWait({ niche: "ai_tools", scriptId: "abc" });
if (result.ok) console.log(result.output);

// With options
await myTask.trigger(payload, {
  idempotencyKey: `video-${scriptId}`,
  delay: "5m",
  ttl: "1h",
  tags: ["niche:ai_tools"],
});
```
