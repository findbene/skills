---
name: "init"
description: "Create a new AgentHub collaboration session with task, agent count, and evaluation criteria. Triggers: 'use init', 'init', 'init task'."
allowed-tools: Bash, Glob, Grep, Read
command: /hub:init
---

# /hub:init — Create New Session

Initialize an AgentHub collaboration session. Creates the `.agenthub/` directory structure, generates a session ID, and configures evaluation criteria.

## Usage

```
/hub:init                                                    # Interactive mode
/hub:init --task "Optimize API" --agents 3 --eval "pytest bench.py" --metric p50_ms --direction lower
/hub:init --task "Refactor auth" --agents 2                  # No eval (LLM judge mode)
```

## What It Does

### If arguments provided

Pass them to the init script:

```bash
python {skill_path}/scripts/hub_init.py \
  --task "{task}" --agents {N} \
  [--eval "{eval_cmd}"] [--metric {metric}] [--direction {direction}] \
  [--base-branch {branch}]
```

### If no arguments (interactive mode)

Collect each parameter:

1. **Task** — What should the agents do? (required) → verify: user confirms
2. **Agent count** — How many parallel agents? (default: 3) → verify: step output matches expected outcome
3. **Eval command** — Command to measure results (optional — skip for LLM judge mode) → verify: step output matches expected outcome
4. **Metric name** — What metric to extract from eval output (required if eval command given) → verify: step output matches expected outcome
5. **Direction** — Is lower or higher better? (required if metric given) → verify: step output matches expected outcome
6. **Base branch** — Branch to fork from (default: current branch) → verify: step output matches expected outcome

### Output

```
AgentHub session initialized
  Session ID: 20260317-143022
  Task: Optimize API response time below 100ms
  Agents: 3
  Eval: pytest bench.py --json
  Metric: p50_ms (lower is better)
  Base branch: dev
  State: init

Next step: Run /hub:spawn to launch 3 agents
```

For content or research tasks (no eval command → LLM judge mode):

```
AgentHub session initialized
  Session ID: 20260317-151200
  Task: Draft 3 competing taglines for product launch
  Agents: 3
  Eval: LLM judge (no eval command)
  Base branch: dev
  State: init

Next step: Run /hub:spawn to launch 3 agents
```

## Baseline Capture

If `--eval` was provided, capture a baseline measurement after session creation:

1. Run the eval command in the current working directory → verify: command exit code 0
2. Extract the metric value from stdout → verify: step output matches expected outcome
3. Append `baseline: {value}` to `.agenthub/sessions/{session-id}/config.yaml` → verify: step output matches expected outcome
4. Display: `Baseline captured: {metric} = {value}` → verify: step output matches expected outcome

This baseline is used by `result_ranker.py --baseline` during evaluation to show deltas. If the eval command fails at this stage, warn the user but continue — baseline is optional.

## After Init

Tell the user:
- Session created with ID `{session-id}`
- Baseline metric (if captured)
- Next step: `/hub:spawn` to launch agents
- Or `/hub:spawn {session-id}` if multiple sessions exist

## When NOT to use

- Task is unrelated to init — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Init needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for init
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving init
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
