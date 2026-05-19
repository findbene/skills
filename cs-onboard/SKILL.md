---
name: "cs-onboard"
description: "Founder onboarding interview that captures company context across 7 dimensions. Triggers: 'use cs-onboard', 'cs onboard', 'cs-onboard task'."
allowed-tools: Glob, Grep, Read
license: MIT
metadata:
  version: 1.0.0
  author: Alireza Rezvani
  category: c-level
  domain: orchestration
  updated: 2026-03-05
  frameworks: founder-interview, context-capture, quarterly-refresh
---

# C-Suite Onboarding

Structured founder interview that builds the company context file powering every C-suite advisor. One 45-minute conversation. Persistent context across all roles.

## Commands

- `/cs:setup` — Full onboarding interview (~45 min, 7 dimensions)
- `/cs:update` — Quarterly refresh (~15 min, "what changed?")

## Keywords
cs:setup, cs:update, company context, founder interview, onboarding, company profile, c-suite setup, advisor setup

---

## Conversation Principles

Be a conversation, not an interrogation. Ask one question at a time. Follow threads. Reflect back: "So the real issue sounds like X — is that right?" Watch for what they skip — that's where the real story lives. Never read a list of questions.

Open with: *"Tell me about the company in your own words — what are you building and why does it matter?"*

---

## 7 Interview Dimensions

### 1. Company Identity
Capture: what they do, who it's for, the real founding "why," one-sentence pitch, non-negotiable values.
Key probe: *"What's a value you'd fire someone over violating?"*
Red flag: Values that sound like marketing copy.

### 2. Stage & Scale
Capture: headcount (FT vs contractors), revenue range, runway, stage (pre-PMF / scaling / optimizing), what broke in last 90 days.
Key probe: *"If you had to label your stage — still finding PMF, scaling what works, or optimizing?"*

### 3. Founder Profile
Capture: self-identified superpower, acknowledged blind spots, archetype (product/sales/technical/operator), what actually keeps them up at night.
Key probe: *"What would your co-founder say you should stop doing?"*
Red flag: No blind spots, or weakness framed as a strength.

### 4. Team & Culture
Capture: team in 3 words, last real conflict and resolution, which values are real vs aspirational, strongest and weakest leader.
Key probe: *"Which of your stated values is most real? Which is a poster on the wall?"*
Red flag: "We have no conflict."

### 5. Market & Competition
Capture: who's winning and why (honest version), real unfair advantage, the one competitive move that could hurt them.
Key probe: *"What's your real unfair advantage — not the investor version?"*
Red flag: "We have no real competition."

### 6. Current Challenges
Capture: priority stack-rank across product/growth/people/money/operations, the decision they've been avoiding, the "one extra day" answer.
Key probe: *"What's the decision you've been putting off for weeks?"*
Note: The "extra day" answer reveals true priorities.

### 7. Goals & Ambition
Capture: 12-month target (specific), 36-month target (directional), exit vs build-forever orientation, personal success definition.
Key probe: *"What does success look like for you personally — separate from the company?"*

---

## Output: company-context.md

After the interview, generate `~/.claude/company-context.md` using `templates/company-context-template.md`.

Fill every section. Write `[not captured]` for unknowns — never leave blank. Add timestamp, mark as `fresh`.

Tell the founder: *"I've captured everything in your company context. Every advisor will use this to give specific, relevant advice. Run /cs:update in 90 days to keep it current."*

---

## /cs:update — Quarterly Refresh

**Trigger:** Every 90 days or after a major change. Duration: ~15 minutes.

Open with: *"It's been [X time] since we did your company context. What's changed?"*

Walk each dimension with one "what changed?" question:
1. Identity: same mission or shifted? → verify: step output matches expected outcome
2. Scale: team, revenue, runway now? → verify: command exit code 0
3. Founder: role or what's stretching you? → verify: step output matches expected outcome
4. Team: any leadership changes? → verify: step output matches expected outcome
5. Market: any competitive surprises? → verify: step output matches expected outcome
6. Challenges: #1 problem now vs 90 days ago? → verify: step output matches expected outcome
7. Goals: still on track for 12-month target? → verify: step output matches expected outcome

Update the context file, refresh timestamp, reset to `fresh`.

---

## Context File Location

`~/.claude/company-context.md` — single source of truth for all C-suite skills. Do not move it. Do not create duplicates.

## References
- `templates/company-context-template.md` — blank template for output
- `references/interview-guide.md` — deep interview craft: probes, red flags, handling reluctant founders

## When NOT to use

- Task is unrelated to cs onboard — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Cs Onboard needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for cs onboard
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving cs onboard
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
