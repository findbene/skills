# Trigger.dev Advanced Patterns

## Batch Triggering
```typescript
export const batchProcessor = task({
  id: "batch-processor",
  run: async (payload: { niche: string }) => {
    const scriptIds = Array.from({ length: 10 }, () => generateId());

    const results = await scriptTask.batchTriggerAndWait(
      scriptIds.map(id => ({
        payload: { niche: payload.niche, scriptId: id },
        options: { idempotencyKey: `script-${id}` }
      }))
    );

    const successes = results.runs.filter(r => r.ok);
    const failures = results.runs.filter(r => !r.ok);
    logger.info("Batch complete", { successes: successes.length, failures: failures.length });

    return { successes: successes.map(r => r.output), failures };
  },
});
```

## Concurrency Control
```typescript
import { task, queue } from "@trigger.dev/sdk/v3";

// Queue-level concurrency limit
export const videoQueue = queue({
  name: "video-generation",
  concurrencyLimit: 5,  // max 5 simultaneous
});

export const videoTask = task({
  id: "video-generation",
  queue: videoQueue,
  run: async (payload) => { ... },
});

// Per-key concurrency (one per user at a time)
// Use in trigger options: { concurrencyKey: payload.userId }
```

## Wait for External Event
```typescript
export const approvalWorkflow = task({
  id: "approval-workflow",
  run: async (payload: { proposalId: string }) => {
    await sendApprovalEmail(payload.proposalId);

    // Wait up to 7 days for webhook
    const decision = await wait.forEvent("proposal-decision", {
      filter: { proposalId: payload.proposalId },
      timeout: "7d",
    });

    if (!decision) return { status: "timed_out" };
    if (decision.data.approved) {
      await executeProposal(payload.proposalId);
      return { status: "approved" };
    }
    return { status: "rejected", reason: decision.data.reason };
  },
});
```

## Real-time Progress Updates
```typescript
import { task, metadata } from "@trigger.dev/sdk/v3";

export const progressiveTask = task({
  id: "progressive-task",
  run: async (payload: { items: string[] }) => {
    const total = payload.items.length;
    for (let i = 0; i < total; i++) {
      await metadata.set("progress", {
        current: i + 1,
        total,
        percentage: Math.round(((i + 1) / total) * 100),
      });
      await processItem(payload.items[i]);
    }
    return { processed: total };
  },
});
```

## Pipeline Orchestration (AI Agency Pattern)
```typescript
export const videoPipeline = task({
  id: "video-pipeline",
  run: async (payload: { niche: string }) => {
    // Step 1: Script
    const script = await scriptTask.triggerAndWait({ niche: payload.niche });
    if (!script.ok) throw new Error("Script failed");

    // Step 2: Voiceover + Visuals in parallel
    const [voice, visuals] = await Promise.all([
      voiceoverTask.triggerAndWait({ script: script.output.text }),
      visualTask.triggerAndWait({ script: script.output.text }),
    ]);

    // Step 3: Assemble
    const video = await assemblyTask.triggerAndWait({
      scriptId: script.output.id,
      audioUrl: voice.output?.audioUrl,
      imageUrls: visuals.output?.urls,
    });

    // Step 4: Quality check
    const audit = await auditorTask.triggerAndWait({ videoId: video.output?.videoId });
    if (audit.output?.score < 7) {
      // Retry with feedback
      return videoPipeline.triggerAndWait({
        niche: payload.niche,
        feedback: audit.output.recommendation,
      });
    }

    return video.output;
  },
});
```
