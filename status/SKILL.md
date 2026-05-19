---
name: "status"
description: "Show DAG state, agent progress, and branch status for an AgentHub session. Triggers: 'use status', 'status', 'status task'."
allowed-tools: Bash, Glob, Grep, Read
command: /hub:status
---

# /hub:status — Session Status

Display the current state of an AgentHub session: agent branches, commit counts, frontier status, and board updates.

## Usage

```
/hub:status                        # Status for latest session
/hub:status 20260317-143022        # Status for specific session
```

## What It Does

1. Run session overview: → verify: command exit code 0
```bash
python {skill_path}/scripts/session_manager.py --status {session-id}
```

2. Run DAG analysis: → verify: command exit code 0
```bash
python {skill_path}/scripts/dag_analyzer.py --status --session {session-id}
```

3. Read recent board updates: → verify: file content matches expected shape
```bash
python {skill_path}/scripts/board_manager.py --read progress
```

## Output Format

```
Session: 20260317-143022 (running)
Task: Optimize API response time below 100ms
Agents: 3 | Base: dev

AGENT    BRANCH                                        COMMITS  STATUS     LAST UPDATE
agent-1  hub/20260317-143022/agent-1/attempt-1         3        frontier   2026-03-17 14:35:10
agent-2  hub/20260317-143022/agent-2/attempt-1         5        frontier   2026-03-17 14:36:45
agent-3  hub/20260317-143022/agent-3/attempt-1         2        frontier   2026-03-17 14:34:22

Recent Board Activity:
  [progress] agent-1: Implemented caching, running tests
  [progress] agent-2: Hash map approach working, benchmarking
  [results]  agent-2: Final result posted
```

Example output for a content task:

```
Session: 20260317-151200 (running)
Task: Draft 3 competing taglines for product launch
Agents: 3 | Base: dev

AGENT    BRANCH                                        COMMITS  STATUS     LAST UPDATE
agent-1  hub/20260317-151200/agent-1/attempt-1         2        frontier   2026-03-17 15:18:30
agent-2  hub/20260317-151200/agent-2/attempt-1         2        frontier   2026-03-17 15:19:12
agent-3  hub/20260317-151200/agent-3/attempt-1         1        frontier   2026-03-17 15:17:55

Recent Board Activity:
  [progress] agent-1: Storytelling angle draft complete, refining CTA
  [progress] agent-2: Benefit-led draft done, testing urgency variant
  [results]  agent-3: Final result posted
```

## After Status

If all agents have posted results:
- Suggest `/hub:eval` to rank results

If some agents are still running:
- Show which are done vs in-progress
- Suggest waiting or checking again later

## When NOT to use

- Task is unrelated to status — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Status needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for status
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving status
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
