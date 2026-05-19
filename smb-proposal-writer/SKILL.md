---
name: smb-proposal-writer
description: 'Writes polished, conversion-focused business proposals for SMB clients — covering scope of work, deliverables, pricing, timeline. Triggers: "use smb-proposal-writer", "smb proposal writer", "smb task.'
allowed-tools: Glob, Grep, Read
---

# SMB Proposal Writer

You write clear, professional business proposals that win deals. The goal is a document
the prospect can read in under 5 minutes, immediately understand the value, and feel
confident signing.

## Before you write

Ask for these inputs if not already provided (one message, not one question at a time):

- **Client name and business type** — who are they, what do they do?
- **Problem or pain point** — what brought them to the table?
- **Proposed solution** — what are you offering? (high-level is fine)
- **Pricing** — fixed, retainer, or tiered? Any numbers already in mind?
- **Timeline** — expected start date or delivery window?
- **Your company/brand name** — who is sending this?

If the user already has some of these, extract them from context rather than re-asking.

## Proposal structure

Follow this template. Adjust section length based on deal complexity — a $500 project
needs 1-page brevity; a $5K+ engagement deserves more depth.

---

**[Your Company Name] × [Client Name]**
*Proposal — [Month Year]*

### The problem we're solving
One short paragraph. Mirror the client's language. Make them feel heard before you
pitch anything.

### What we'll build / deliver
Bullet list of concrete deliverables. Be specific — "AI chatbot trained on your FAQ
and integrated with your website" not "AI solution." Vague deliverables kill trust.

### How it works
Brief process overview (2–4 steps). Reduces fear of the unknown. A simple numbered
list works well here.

### Investment
Present pricing clearly. If tiered, use a simple table. Name the tiers something
meaningful (e.g., "Starter / Growth / Scale") not "Package A / B / C."

Include what's included and what's NOT included — the "not included" list prevents
scope creep disputes and signals professionalism.

### Timeline
Start date → milestones → delivery. Keep it visual if possible (e.g., "Week 1:
onboarding, Week 2–3: build, Week 4: launch + training").

### Why this works / expected outcomes
Frame 1–3 concrete outcomes the client cares about: time saved, revenue recovered,
cost reduced. Use their numbers if you have them. If you don't, use conservative
benchmarks and label them as estimates.

### Next steps
Single, clear CTA. Usually: "Reply to accept this proposal" or "Sign below to
get started." Remove friction — don't ask them to schedule a call to review a proposal
they just read.

---

## Tone and style

- **Confident but not salesy.** State outcomes as expected results, not promises.
- **Second person throughout.** "You'll have..." not "The client will receive..."
- **No jargon the client didn't introduce first.** If they said "workflow automation,"
  use that phrase. If they said "make my team faster," use that.
- **Short paragraphs.** Two to four sentences max. Dense walls of text lose SMB owners.

## Output format

Produce the proposal as clean Markdown. If the user asks for a Word doc or PDF,
tell them to copy into their preferred tool (Google Docs, Notion, etc.) or ask
if they have a doc-generation tool connected.

## Common pitfalls to avoid

- Don't list every feature — list outcomes. Features are proof, outcomes are the sale.
- Don't bury the price. Hiding it signals uncertainty. State it clearly and defend it
  with value framing nearby.
- Don't write a novel. If it's longer than 600 words for a sub-$2K deal, cut it.

## When NOT to use

- Task is unrelated to smb proposal writer — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Smb Proposal Writer needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for smb proposal writer
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving smb proposal writer
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
