# Trigger.dev v3 Core Reference

## Installation
```bash
npm install @trigger.dev/sdk@beta
npx trigger.dev@beta init
```

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
    logger.info("Done", { url: result.url });
    return result;
  },
});
```

## Triggering Tasks
```typescript
// Fire and forget
const handle = await myTask.trigger({ niche: "ai_tools", scriptId: "abc123" });

// Wait for result
const result = await myTask.triggerAndWait({ niche: "ai_tools", scriptId: "abc123" });
if (result.ok) console.log(result.output);
else console.error(result.error);

// With options
await myTask.trigger(payload, {
  idempotencyKey: `video-${scriptId}`,  // prevent duplicates
  delay: "5m",
  ttl: "1h",
  tags: ["niche:ai_tools", "tier:2"],
});
```

## Scheduled Tasks (Cron)
```typescript
import { schedules } from "@trigger.dev/sdk/v3";

export const dailyPipeline = schedules.task({
  id: "daily-pipeline",
  cron: "0 6 * * *",   // 6am daily

  run: async (payload) => {
    logger.info("Daily run", { lastRunAt: payload.lastRunAt });
    await videoTask.batchTrigger(
      NICHES.map(niche => ({ payload: { niche, scriptId: generateId() } }))
    );
  },
});
```

Common cron patterns:
```
0 * * * *     — every hour
0 6 * * *     — 6am daily
0 6 * * 0     — 6am every Sunday
*/15 * * * *  — every 15 minutes
```

## Error Handling
```typescript
import { task, AbortTaskRunError, RetryAfterError } from "@trigger.dev/sdk/v3";

export const robustTask = task({
  id: "robust-task",
  retry: { maxAttempts: 5, factor: 2, minTimeoutInMs: 2000 },

  run: async (payload: { id: string }) => {
    try {
      return await externalApi.call(payload.id);
    } catch (error) {
      if (error.status === 429) {
        // Rate limited — retry after server-specified delay
        throw new RetryAfterError(
          "Rate limited",
          parseInt(error.headers?.["retry-after"] || "60")
        );
      }
      if (error.status === 404) {
        // Don't retry — resource doesn't exist
        throw new AbortTaskRunError(`Resource ${payload.id} not found`);
      }
      throw error;  // all other errors: retry with backoff
    }
  },
});
```

## Get Run Status
```typescript
import { runs } from "@trigger.dev/sdk/v3";

const run = await runs.retrieve(runId);
// run.status: "COMPLETED" | "FAILED" | "EXECUTING" | "WAITING_TO_EXECUTE"
// run.output: typed output

const result = await runs.poll(runId, { pollIntervalMs: 1000 });
```
