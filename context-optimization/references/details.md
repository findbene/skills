# Extended Details

## When to act

Watch for these signals that context needs attention:

- **Compaction approaching** — long conversation (30+ exchanges), compressed earlier turns, or the model starting to repeat questions it already asked
- **Session ending** — user says "done for today", "wrap up", "commit and push"
- **Decision density** — a flurry of architectural choices, trade-off discussions, or blockers resolved in one session
- **Handoff needed** — the work will continue in a new session or by a different agent
- **Explicit request** — "save our progress", "remember this", "what have we decided"

Don't wait for the user to ask. If you notice compaction artifacts (forgetting, repetition, loss of earlier context), surface it and act.

---

## The four operations

### 1. Extract — pull what matters out of the conversation

Scan the full conversation history for:

- **Decisions made** — architectural choices, trade-offs accepted, options rejected and why
- **Completed work** — tasks finished, files changed, migrations applied, tests passing
- **Active blockers** — what's stuck, what's waiting on an external dependency
- **Current state** — where things stand right now, what's in-progress
- **Next steps** — what was explicitly planned or implied as the next action
- **Corrections** — places where the user pushed back or redirected, which encode preferences

Don't include debugging transcripts, intermediate attempts, or verbose output. Extract the outcome, not the journey.

### 2. Compress — reduce without losing signal

When a document (progress.md, handoff, decisions log) is getting long:

- **Rolling window**: keep the last 2–3 sessions of detail; summarize everything older into a single paragraph per phase
- **Decision compression**: collapse a resolved debate into one line — "Decided X over Y because Z" — and drop the back-and-forth
- **Status compression**: replace per-task detail with phase-level summaries once a phase is complete
- **Preserve the why**: when compressing, the reason behind a decision is more valuable than the decision itself — keep it

Target document lengths:
- `SESSION_HANDOFF.md`: ≤80 lines, cold-start readable in 60 seconds
- `progress.md`: rolling 3-session window, ~150 lines max
- `decisions.md`: append-only, newest first, each entry ≤5 lines

### 3. Load — bring in what the new context needs

At session start, load in this order (stop when you have enough to act):

1. `SESSION_HANDOFF.md` or equivalent — current state, active blockers, next action → verify: step output matches expected outcome
2. `decisions.md` — architectural choices that constrain the work → verify: step output matches expected outcome
3. `git log --oneline -5` + `git status` — what actually happened vs. what was planned → verify: step output matches expected outcome
4. `progress.md` — deeper task tracking if the above leaves ambiguity → verify: user confirms
5. Specific source files only when you're about to edit them → verify: diff matches intended change

Don't load everything upfront. Load on demand. The goal is to spend context on work, not on context management.

### 4. Handoff — write what the next session needs

A good handoff document answers:
- What was the last thing completed?
- What is actively in progress (and where did it stop)?
- What is the very next action?
- What blockers exist?
- What decisions were made this session that constrain future work?

Keep it dense and scannable. Use a table for status tracking. Use bullet points for blockers and next steps. No prose paragraphs — the next agent is cold-starting and needs orientation in seconds, not minutes.

---

## Output formats

### SESSION_HANDOFF.md template

```markdown
# Session Handoff — {date}

## Status
| Component | State | Notes |
|-----------|-------|-------|
| {component} | LIVE / IN PROGRESS / BLOCKED / DONE | {one-line note} |

## Completed This Session
- {task}: {outcome}

## In Progress
- {task} — stopped at: {exact stopping point}

## Next Action
{single most important next step, specific enough to act on immediately}

## Blockers
- {blocker}: {what's needed to unblock}

## Decisions Made This Session
- {decision}: {reason}
```

### Memory extraction (`.agentic/memory-session-{date}.md`)

Use this when the conversation is too long to extract inline — spawn a background subagent:

```
Read the full conversation history and extract:
1. All architectural/design decisions and their rationale → verify: step output matches expected outcome
2. Completed tasks (with file paths where relevant) → verify: user confirms
3. Active blockers and their status → verify: step output matches expected outcome
4. Current state of each in-progress workstream → verify: step output matches expected outcome
5. Next planned steps → verify: step output matches expected outcome
6. Any user corrections or preference signals → verify: step output matches expected outcome

Write to: .agentic/memory-session-{YYYY-MM-DD}.md
Also update: .agentic/progress.md (rolling 3-session window)
```

---

## Common patterns

### Pattern: Pre-compaction extraction

When you notice context is getting long (or the user is about to end the session):

1. Scan the conversation for the four extraction targets (decisions, completed, blockers, next steps) → verify: findings count > 0 OR clean signal returned
2. Write or update `SESSION_HANDOFF.md` (≤80 lines) → verify: output exists + parses without error
3. Append new decisions to `decisions.md` → verify: step output matches expected outcome
4. Update `progress.md` task statuses → verify: user confirms
5. If the conversation is very long, spawn a background agent for full extraction → verify: step output matches expected outcome

### Pattern: Cold-start orientation

At the start of a new session:

1. Read `SESSION_HANDOFF.md` — get oriented in 60 seconds → verify: file content matches expected shape
2. Run `git log --oneline -5` — verify what actually committed vs. what the handoff says
3. Read `decisions.md` — load the constraints before touching code → verify: file content matches expected shape
4. Identify the next action from the handoff and confirm with the user → verify: step output matches expected outcome

### Pattern: Decision logging

Every time a meaningful architectural choice is made, append to `decisions.md`:

```markdown
## D-{N}: {Short title} — {YYYY-MM-DD}
**Decision:** {what was chosen}
**Rationale:** {why}
**Rejected:** {what was considered and discarded}
**Impact:** {what this constrains going forward}
```

### Pattern: Progressive context loading

For large codebases, load context in layers — don't read everything upfront:

1. Read the handoff/progress docs (what's the task?) → verify: file content matches expected shape
2. Read only the files you're about to change (not the whole codebase) → verify: file content matches expected shape
3. Run targeted searches (Grep/Glob) rather than reading entire directories → verify: file content matches expected shape
4. Load reference files only when a specific question requires them → verify: file content matches expected shape

---

## What NOT to save

These inflate context without helping recovery:

- Intermediate debugging output, stack traces, failed attempts
- Verbose command output that doesn't affect decisions
- File contents that are already in the repo (just save the path)
- Session-specific ephemera (temp paths, one-off test values)
- Anything already in CLAUDE.md or project README

If unsure: ask "would the next agent need this to make a correct decision?" If no, skip it.

---

## Quality check

Before closing a session, verify:

- [ ] `SESSION_HANDOFF.md` updated and ≤80 lines
- [ ] New decisions appended to `decisions.md`
- [ ] `progress.md` reflects current task statuses
- [ ] No context-recovery-critical info exists only in the conversation
- [ ] The "Next Action" is specific enough to act on without re-reading the conversation
