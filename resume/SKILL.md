---
name: "resume"
description: "Resume a paused experiment. Checkout the experiment branch, read results history, continue iterating. Triggers: 'use resume', 'resume', 'resume task'."
allowed-tools: Bash, Glob, Grep, Read
command: /ar:resume
---

# /ar:resume — Resume Experiment

Resume a paused or context-limited experiment. Reads all history and continues where you left off.

## Usage

```
/ar:resume                                  # List experiments, let user pick
/ar:resume engineering/api-speed            # Resume specific experiment
```

## What It Does

### Step 1: List experiments if needed

If no experiment specified:

```bash
python {skill_path}/scripts/setup_experiment.py --list
```

Show status for each (active/paused/done based on results.tsv age). Let user pick.

### Step 2: Load full context

```bash
# Checkout the experiment branch
git checkout autoresearch/{domain}/{name}

# Read config
cat .autoresearch/{domain}/{name}/config.cfg

# Read strategy
cat .autoresearch/{domain}/{name}/program.md

# Read full results history
cat .autoresearch/{domain}/{name}/results.tsv

# Read recent git log for the branch
git log --oneline -20
```

### Step 3: Report current state

Summarize for the user:

```
Resuming: engineering/api-speed
  Target: src/api/search.py
  Metric: p50_ms (lower is better)
  Experiments: 23 total — 8 kept, 12 discarded, 3 crashed
  Best: 185ms (-42% from baseline of 320ms)
  Last experiment: "added response caching" → KEEP (185ms)

  Recent patterns:
  - Caching changes: 3 kept, 1 discarded (consistently helpful)
  - Algorithm changes: 2 discarded, 1 crashed (high risk, low reward so far)
  - I/O optimization: 2 kept (promising direction)
```

### Step 4: Ask next action

```
How would you like to continue?
  1. Single iteration (/ar:run)  — I'll make one change and evaluate → verify: command exit code 0
  2. Start a loop (/ar:loop)     — Autonomous with scheduled interval → verify: step output matches expected outcome
  3. Just show me the results    — I'll review and decide → verify: step output matches expected outcome
```

If the user picks loop, hand off to `/ar:loop` with the experiment pre-selected.
If single, hand off to `/ar:run`.

## When NOT to use

- Task is unrelated to resume — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Resume needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for resume
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving resume
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
