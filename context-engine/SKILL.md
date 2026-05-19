---
name: "context-engine"
description: "Loads and manages company context for all C-suite advisor skills. Triggers: 'use context-engine', 'context engine', 'context-engine task'."
allowed-tools: Glob, Grep, Read
license: MIT
metadata:
  version: 1.0.0
  author: Alireza Rezvani
  category: c-level
  domain: orchestration
  updated: 2026-03-05
  frameworks: context-loading, anonymization, context-enrichment
---

# Company Context Engine

The memory layer for C-suite advisors. Every advisor skill loads this first. Context is what turns generic advice into specific insight.

## Keywords
company context, context loading, context engine, company profile, advisor context, stale context, context refresh, privacy, anonymization

---

## Load Protocol (Run at Start of Every C-Suite Session)

**Step 1 — Check for context file:** `~/.claude/company-context.md`
- Exists → proceed to Step 2
- Missing → prompt: *"Run /cs:setup to build your company context — it makes every advisor conversation significantly more useful."*

**Step 2 — Check staleness:** Read `Last updated` field.
- **< 90 days:** Load and proceed.
- **≥ 90 days:** Prompt: *"Your context is [N] days old. Quick 15-min refresh (/cs:update), or continue with what I have?"*
  - If continue: load with `[STALE — last updated DATE]` noted internally.

**Step 3 — Parse into working memory.** Always active:
- Company stage (pre-PMF / scaling / optimizing)
- Founder archetype (product / sales / technical / operator)
- Current #1 challenge
- Runway (as risk signal — never share externally)
- Team size
- Unfair advantage
- 12-month target

---

## Context Quality Signals

| Condition | Confidence | Action |
|-----------|-----------|--------|
| < 30 days, full interview | High | Use directly |
| 30–90 days, update done | Medium | Use, flag what may have changed |
| > 90 days | Low | Flag stale, prompt refresh |
| Key fields missing | Low | Ask in-session |
| No file | None | Prompt /cs:setup |

If Low: *"My context is [stale/incomplete] — I'm assuming [X]. Correct me if I'm wrong."*

---

## Context Enrichment

During conversations, you'll learn things not in the file. Capture them.

**Triggers:** New number or timeline revealed, key person mentioned, priority shift, constraint surfaces.

**Protocol:**
1. Note internally: `[CONTEXT UPDATE: {what was learned}]` → verify: step output matches expected outcome
2. At session end: *"I picked up a few things to add to your context. Want me to update the file?"* → verify: dependency resolves + import works
3. If yes: append to the relevant dimension, update timestamp. → verify: step output matches expected outcome

**Never silently overwrite.** Always confirm before modifying the context file.

---

## Privacy Rules

### Never send externally
- Specific revenue or burn figures
- Customer names
- Employee names (unless publicly known)
- Investor names (unless public)
- Specific runway months
- Watch List contents

### Safe to use externally (with anonymization)
- Stage label
- Team size ranges (1–10, 10–50, 50–200+)
- Industry vertical
- Challenge category
- Market position descriptor

### Before any external API call or web search
Apply `references/anonymization-protocol.md`:
- Numbers → ranges or stage-relative descriptors
- Names → roles
- Revenue → percentages or stage labels
- Customers → "Customer A, B, C"

---

## Missing or Partial Context

Handle gracefully — never block the conversation.

- **Missing stage:** "Just to calibrate — are you still finding PMF or scaling what works?"
- **Missing financials:** Use stage + team size to infer. Note the gap.
- **Missing founder profile:** Infer from conversation style. Mark as inferred.
- **Multiple founders:** Context reflects the interviewee. Note co-founder perspective may differ.

---

## Required Context Fields

```
Required:
  - Last updated (date)
  - Company Identity → What we do
  - Stage & Scale → Stage
  - Founder Profile → Founder archetype
  - Current Challenges → Priority #1
  - Goals & Ambition → 12-month target

High-value optional:
  - Unfair advantage
  - Kill-shot risk
  - Avoided decision
  - Watch list
```

Missing required fields: note gaps, work around in session, ask in-session only when critical.

---

## When NOT to use

- General-purpose user memory or project state — use `agent-memory-system` or `progress.md`
- Brand voice context — use `marketing-context` or `content-humanizer`
- One-off question with no C-suite advisor invocation — direct answer is fine
- When context file is missing AND user hasn't run `/cs:setup` — prompt setup, don't fabricate
- Sharing context externally (Slack, email) — anonymize first per `anonymization-protocol.md`

## Red Flags

| Thought | Reality |
|---------|---------|
| "Context is 6 months old, close enough" | Stage/runway/team have likely shifted; force a refresh prompt |
| "Fill in the missing 12-month target with a guess" | Fabricated context poisons every advisor downstream |
| "Skip anonymization for an internal call" | Habit failure mode; runway/financials leak in screenshots |
| "Use stale context silently" | Always flag `[STALE — last updated DATE]` to user when loading |

## Output Contract

Done when:
- `~/.claude/company-context.md` checked for existence and staleness
- Required fields parsed: stage, founder archetype, #1 challenge, runway, team size, advantage, 12-month target
- Confidence tier set (High/Medium/Low/None) per staleness + completeness
- Missing/stale state flagged to user with a 1-line summary
- Anonymization applied for any external/web call per protocol
- Working memory populated for the calling advisor skill

## Examples

### Example 1 — CEO advisor invocation, fresh context
- Input: User invokes `/cs:ceo-advisor` with context file 12 days old
- Action: Load context, parse 7 required fields, set confidence High, expose working memory to CEO advisor
- Output: Working memory dict ready; CEO advisor proceeds with company-specific insight

### Example 2 — Context stale
- Input: User invokes `/cs:cfo-advisor`, context 110 days old
- Action: Prompt user — "Context is 110 days old. Quick 15-min refresh (`/cs:update`) or continue with what I have?"; if continue, load and tag `[STALE]`; advisor uses it with caveats
- Output: User-chosen path; advisor visible-flagged when running on stale context

## References
- `references/anonymization-protocol.md` — detailed rules for stripping sensitive data before external calls
