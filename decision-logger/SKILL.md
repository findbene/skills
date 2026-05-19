---
name: "decision-logger"
description: "Two-layer memory architecture for board meeting decisions. Manages raw transcripts (Layer 1) and approved decisions (Layer 2. Triggers: 'use decision-logger', 'decision logger', 'decision-logger task."
allowed-tools: Bash, Glob, Grep, Read
license: MIT
metadata:
  version: 1.0.0
  author: Alireza Rezvani
  category: c-level
  domain: decision-memory
  updated: 2026-03-05
  python-tools: scripts/decision_tracker.py
---

# Decision Logger

Two-layer memory system. Layer 1 stores everything. Layer 2 stores only what the founder approved. Future meetings read Layer 2 only — this prevents hallucinated consensus from past debates bleeding into new deliberations.

## Keywords
decision log, memory, approved decisions, action items, board minutes, /cs:decisions, /cs:review, conflict detection, DO_NOT_RESURFACE

## Quick Start

```bash
python scripts/decision_tracker.py --demo             # See sample output
python scripts/decision_tracker.py --summary          # Overview + overdue
python scripts/decision_tracker.py --overdue          # Past-deadline actions
python scripts/decision_tracker.py --conflicts        # Contradiction detection
python scripts/decision_tracker.py --owner "CTO"      # Filter by owner
python scripts/decision_tracker.py --search "pricing" # Search decisions
```

---

## Commands

| Command | Effect |
|---------|--------|
| `/cs:decisions` | Last 10 approved decisions |
| `/cs:decisions --all` | Full history |
| `/cs:decisions --owner CMO` | Filter by owner |
| `/cs:decisions --topic pricing` | Search by keyword |
| `/cs:review` | Action items due within 7 days |
| `/cs:review --overdue` | Items past deadline |

---

## Two-Layer Architecture

### Layer 1 — Raw Transcripts
**Location:** `memory/board-meetings/YYYY-MM-DD-raw.md`
- Full Phase 2 agent contributions, Phase 3 critique, Phase 4 synthesis
- All debates, including rejected arguments
- **NEVER auto-loaded.** Only on explicit founder request.
- Archive after 90 days → `memory/board-meetings/archive/YYYY/`

### Layer 2 — Approved Decisions
**Location:** `memory/board-meetings/decisions.md`
- ONLY founder-approved decisions, action items, user corrections
- **Loaded automatically in Phase 1 of every board meeting**
- Append-only. Decisions are never deleted — only superseded.
- Managed by Chief of Staff after Phase 5. Never written by agents directly.

---

## Decision Entry Format

```markdown
## [YYYY-MM-DD] — [AGENDA ITEM TITLE]

**Decision:** [One clear statement of what was decided.]
**Owner:** [One person or role — accountable for execution.]
**Deadline:** [YYYY-MM-DD]
**Review:** [YYYY-MM-DD]
**Rationale:** [Why this over alternatives. 1-2 sentences.]

**User Override:** [If founder changed agent recommendation — what and why. Blank if not applicable.]

**Rejected:**
- [Proposal] — [reason] [DO_NOT_RESURFACE]

**Action Items:**
- [ ] [Action] — Owner: [name] — Due: [YYYY-MM-DD] — Review: [YYYY-MM-DD]

**Supersedes:** [DATE of previous decision on same topic, if any]
**Superseded by:** [Filled in retroactively if overridden later]
**Raw transcript:** memory/board-meetings/[DATE]-raw.md
```

---

## Conflict Detection

Before logging, Chief of Staff checks for:
1. **DO_NOT_RESURFACE violations** — new decision matches a rejected proposal → verify: step output matches expected outcome
2. **Topic contradictions** — two active decisions on same topic with different conclusions → verify: step output matches expected outcome
3. **Owner conflicts** — same action assigned to different people in different decisions → verify: step output matches expected outcome

When a conflict is found:
```
⚠️ DECISION CONFLICT
New: [text]
Conflicts with: [DATE] — [existing text]

Options: (1) Supersede old  (2) Merge  (3) Defer to founder
```

**DO_NOT_RESURFACE enforcement:**
```
🚫 BLOCKED: "[Proposal]" was rejected on [DATE]. Reason: [reason].
To reopen: founder must explicitly say "reopen [topic] from [DATE]".
```

---

## Logging Workflow (Post Phase 5)

1. Founder approves synthesis → verify: step output matches expected outcome
2. Write Layer 1 raw transcript → `YYYY-MM-DD-raw.md`
3. Check conflicts against `decisions.md` → verify: all tests pass
4. Surface conflicts → wait for founder resolution
5. Append approved entries to `decisions.md` → verify: step output matches expected outcome
6. Confirm: decisions logged, actions tracked, DO_NOT_RESURFACE flags added → verify: package installed + import succeeds

---

## Marking Actions Complete

```markdown
- [x] [Action] — Owner: [name] — Completed: [DATE] — Result: [one sentence]
```

Never delete completed items. The history is the record.

---

## File Structure

```
memory/board-meetings/
├── decisions.md       # Layer 2: append-only, founder-approved
├── YYYY-MM-DD-raw.md  # Layer 1: full transcript per meeting
└── archive/YYYY/      # Raw files after 90 days
```

---

## References
- `templates/decision-entry.md` — single entry template with field rules
- `scripts/decision_tracker.py` — CLI parser, overdue tracker, conflict detector

## When NOT to use

- Task is unrelated to decision logger — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Decision Logger needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for decision logger
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving decision logger
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
