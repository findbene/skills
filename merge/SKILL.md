---
name: "merge"
description: "Merge the winning agent's branch into base, archive losers, and clean up worktrees. Triggers: 'use merge', 'merge', 'merge task'."
allowed-tools: Bash, Glob, Grep, Read
command: /hub:merge
---

# /hub:merge — Merge Winner

Merge the best agent's branch into the base branch, archive losing branches via git tags, and clean up worktrees.

## Usage

```
/hub:merge                                       # Merge winner of latest session
/hub:merge 20260317-143022                       # Merge winner of specific session
/hub:merge 20260317-143022 --agent agent-2       # Explicitly choose winner
```

## What It Does

### 1. Identify Winner

If `--agent` specified, use that. Otherwise, use the #1 ranked agent from the most recent `/hub:eval`.

### 2. Merge Winner

```bash
git checkout {base_branch}
git merge --no-ff hub/{session-id}/{winner}/attempt-1 \
  -m "hub: merge {winner} from session {session-id}

Task: {task}
Winner: {winner}
Session: {session-id}"
```

### 3. Archive Losers

For each non-winning agent:

```bash
# Create archive tag (preserves commits forever)
git tag hub/archive/{session-id}/{agent-id} hub/{session-id}/{agent-id}/attempt-1

# Delete branch ref (commits preserved via tag)
git branch -D hub/{session-id}/{agent-id}/attempt-1
```

### 4. Clean Up Worktrees

```bash
python {skill_path}/scripts/session_manager.py --cleanup {session-id}
```

### 5. Post Merge Summary

Write `.agenthub/board/results/merge-summary.md`:

```markdown
---
author: coordinator
timestamp: {now}
channel: results
---

## Merge Summary

- **Session**: {session-id}
- **Winner**: {winner}
- **Merged into**: {base_branch}
- **Archived**: {loser-1}, {loser-2}, ...
- **Worktrees cleaned**: {count}
```

### 6. Update State

```bash
python {skill_path}/scripts/session_manager.py --update {session-id} --state merged
```

## Safety

- **Confirm with user** before merging — show the diff summary first
- **Never force-push** — merge is always `--no-ff` for clear history
- **Archive, don't delete** — losing agents' commits are preserved via tags
- **Clean worktrees** — don't leave orphan directories on disk

## After Merge

Tell the user:
- Winner merged into `{base_branch}`
- Losers archived with tags `hub/archive/{session-id}/agent-{N}`
- Worktrees cleaned up
- Session state: `merged`

## When NOT to use

- Task is unrelated to merge — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Merge needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for merge
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving merge
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
