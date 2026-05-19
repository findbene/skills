---
name: "loop"
description: "Start an autonomous experiment loop with user-selected interval (10min, 1h, daily, weekly, monthly). Uses CronCreate for scheduling. Triggers: 'use loop', 'loop', 'loop task'."
allowed-tools: Glob, Grep, Read
command: /ar:loop
---

# /ar:loop — Autonomous Experiment Loop

Start a recurring experiment loop that runs at a user-selected interval.

## Usage

```
/ar:loop engineering/api-speed             # Start loop (prompts for interval)
/ar:loop engineering/api-speed 10m         # Every 10 minutes
/ar:loop engineering/api-speed 1h          # Every hour
/ar:loop engineering/api-speed daily       # Daily at ~9am
/ar:loop engineering/api-speed weekly      # Weekly on Monday ~9am
/ar:loop engineering/api-speed monthly     # Monthly on 1st ~9am
/ar:loop stop engineering/api-speed        # Stop an active loop
```

## What It Does

### Step 1: Resolve experiment

If no experiment specified, list experiments and let user pick.

### Step 2: Select interval

If interval not provided as argument, present options:

```
Select loop interval:
  1. Every 10 minutes  (rapid — stay and watch) → verify: step output matches expected outcome
  2. Every hour         (background — check back later) → verify: all checks pass
  3. Daily at ~9am      (overnight experiments) → verify: step output matches expected outcome
  4. Weekly on Monday   (long-running experiments) → verify: command exit code 0
  5. Monthly on 1st     (slow experiments) → verify: step output matches expected outcome
```

Map to cron expressions:

| Interval | Cron Expression | Shorthand |
|----------|----------------|-----------|
| 10 minutes | `*/10 * * * *` | `10m` |
| 1 hour | `7 * * * *` | `1h` |
| Daily | `57 8 * * *` | `daily` |
| Weekly | `57 8 * * 1` | `weekly` |
| Monthly | `57 8 1 * *` | `monthly` |

### Step 3: Create the recurring job

Use `CronCreate` with this prompt (fill in the experiment details):

```
You are running autoresearch experiment "{domain}/{name}".

1. Read .autoresearch/{domain}/{name}/config.cfg for: target, evaluate_cmd, metric, metric_direction → verify: file content matches expected shape
2. Read .autoresearch/{domain}/{name}/program.md for strategy and constraints → verify: file content matches expected shape
3. Read .autoresearch/{domain}/{name}/results.tsv for experiment history → verify: file content matches expected shape
4. Run: git checkout autoresearch/{domain}/{name} → verify: command exit code 0

Then do exactly ONE iteration:
- Review results.tsv: what worked, what failed, what hasn't been tried
- Edit the target file with ONE change (strategy escalation based on run count)
- Commit: git add {target} && git commit -m "experiment: {description}"
- Evaluate: python {skill_path}/scripts/run_experiment.py --experiment {domain}/{name} --single
- Read the output (KEEP/DISCARD/CRASH)

Rules:
- ONE change per experiment
- NEVER modify the evaluator
- If 5 consecutive crashes in results.tsv, delete this cron job (CronDelete) and alert
- After every 10 experiments, update Strategy section of program.md

Current best metric: {read from results.tsv or "no baseline yet"}
Total experiments so far: {count from results.tsv}
```

### Step 4: Store loop metadata

Write to `.autoresearch/{domain}/{name}/loop.json`:

```json
{
  "cron_id": "{id from CronCreate}",
  "interval": "{user selection}",
  "started": "{ISO timestamp}",
  "experiment": "{domain}/{name}"
}
```

### Step 5: Confirm to user

```
Loop started for {domain}/{name}
  Interval: {interval description}
  Cron ID: {id}
  Auto-expires: 3 days (CronCreate limit)

  To check progress: /ar:status
  To stop the loop:  /ar:loop stop {domain}/{name}

  Note: Recurring jobs auto-expire after 3 days.
  Run /ar:loop again to restart after expiry.
```

## Stopping a Loop

When user runs `/ar:loop stop {experiment}`:

1. Read `.autoresearch/{domain}/{name}/loop.json` to get the cron ID → verify: file content matches expected shape
2. Call `CronDelete` with that ID → verify: step output matches expected outcome
3. Delete `loop.json` → verify: step output matches expected outcome
4. Confirm: "Loop stopped for {experiment}. {n} experiments completed." → verify: step output matches expected outcome

## Important Limitations

- **3-day auto-expiry**: CronCreate jobs expire after 3 days. For longer experiments, the user must re-run `/ar:loop` to restart. Results persist — the new loop picks up where the old one left off.
- **One loop per experiment**: Don't start multiple loops for the same experiment.
- **Concurrent experiments**: Multiple experiments can loop simultaneously ONLY if they're on different git branches (which they are by default — each experiment gets `autoresearch/{domain}/{name}`).

## When NOT to use

- Task is unrelated to loop — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Loop needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for loop
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving loop
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
