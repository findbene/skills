---
name: team-communications
description: 'Write internal company communications — 3P updates (Progress/Plans/Problems), company-wide newsletters, FAQ roundups, incident. Triggers: "use team-communications", "team communications", "team task".'
allowed-tools: Glob, Grep, Read
---

# Internal Comms

> Originally contributed by [maximcoding](https://github.com/maximcoding) — enhanced and integrated by the claude-skills team.

Write polished internal communications by loading the right reference file, gathering context, and outputting in the company's exact format.

## Routing

Identify the communication type from the user's request, then read the matching reference file before writing anything:

| Type | Trigger phrases | Reference file |
|---|---|---|
| **3P Update** | "3P", "progress plans problems", "weekly team update", "what did we ship" | `references/3p-updates.md` |
| **Newsletter** | "newsletter", "company update", "weekly/monthly roundup", "all-hands summary" | `references/company-newsletter.md` |
| **FAQ** | "FAQ", "common questions", "what people are asking", "confusion around" | `references/faq-answers.md` |
| **General** | anything internal that doesn't match above | `references/general-comms.md` |

If the type is ambiguous, ask one clarifying question — don't guess.

## Workflow

1. **Read the reference file** for the matched type. Follow its formatting exactly. → verify: file readable + content matches expected shape
2. **Gather inputs.** Use available MCP tools (Slack, Gmail, Google Drive, Calendar) to pull real data. If no tools are connected, ask the user to provide bullet points or raw context. → verify: step output matches expected outcome
3. **Clarify scope.** Confirm: team name (for 3Ps), time period, audience, and any specific items the user wants included or excluded. → verify: step output matches expected outcome
4. **Draft.** Follow the format, tone, and length constraints from the reference file precisely. Do not invent a new format. → verify: step output matches expected outcome
5. **Present the draft** and ask if anything needs to be added, removed, or reworded. → verify: package installed + import succeeds

## Tone & Style (applies to all types)

- Use "we" — you are part of the company.
- Active voice, present tense for progress, future tense for plans.
- Concise. Every sentence should carry information. Cut filler.
- Include metrics and links wherever possible.
- Professional but approachable — not corporate-speak.
- Put the most important information first.

## When tools are unavailable

If the user hasn't connected Slack, Gmail, Drive, or Calendar, don't stall. Ask them to paste or describe what they want covered. You're formatting and sharpening — that's still valuable. Mention which tools would improve future drafts so they can connect them later.

---

## Anti-Patterns

| Anti-Pattern | Why It Fails | Better Approach |
|---|---|---|
| Writing updates without reading the reference template first | Output won't match company format — user has to reformat | Always load the matching reference file before drafting |
| Inventing metrics or accomplishments | Internal comms must be factual — fabrication destroys trust | Only include data the user provided or MCP tools retrieved |
| Using passive voice for accomplishments | "The feature was shipped" hides who did the work | "Team X shipped the feature" — active voice credits the team |
| Writing walls of text for status updates | Leadership scans, doesn't read — key info gets buried | Lead with the headline, follow with 3-5 bullet points |
| Sending without confirming audience | A team update reads differently from a company-wide newsletter | Always confirm: who will read this? |

---

## Related Skills

| Skill | Relationship |
|-------|-------------|
| `project-management/senior-pm` | Broader PM scope — status reports feed into PM reporting |
| `project-management/meeting-analyzer` | Meeting insights can feed into 3P updates and status reports |
| `project-management/confluence-expert` | Publish comms as Confluence pages for permanent record |
| `marketing-skill/content-production` | External comms — use for public-facing content, not internal |

## When NOT to use

- Task is unrelated to team communications — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Team Communications needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for team communications
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving team communications
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
