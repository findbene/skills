---
name: product-marketing-context
description: "Create and maintain a product marketing context document — a structured reference file that ca. Triggers: 'use product-marketing-context', 'product marketing context', 'product-marketing-context task."
allowed-tools: Glob, Grep, Read
metadata:
  version: 2.0.0
---

# Product Marketing Context

You help users create and maintain a product marketing context document. This captures foundational positioning and messaging information that other marketing skills reference, so users don't repeat themselves.

The document is stored at `.agents/product-marketing-context.md`.

## Workflow

### Step 1: Check for Existing Context

First, check if `.agents/product-marketing-context.md` already exists. Also check `.claude/product-marketing-context.md` for older setups — if found there but not in `.agents/`, offer to move it.

**If it exists:**
- Read it and summarize what's captured
- Ask which sections they want to update
- Only gather info for those sections

**If it doesn't exist, offer two options:**

1. **Auto-draft from codebase** (recommended): You'll study the repo—README, landing pages, marketing copy, package.json, etc.—and draft a V1 of the context document. The user then reviews, corrects, and fills gaps. This is faster than starting from scratch. → verify: file readable + content matches expected shape

2. **Start from scratch**: Walk through each section conversationally, gathering info one section at a time. → verify: step output matches expected outcome

Most users prefer option 1. After presenting the draft, ask: "What needs correcting? What's missing?"

### Step 2: Gather Information

**If auto-drafting:**
1. Read the codebase: README, landing pages, marketing copy, about pages, meta descriptions, package.json, any existing docs → verify: file readable + content matches expected shape
2. Draft all sections based on what you find → verify: step output matches expected outcome
3. Present the draft and ask what needs correcting or is missing → verify: step output matches expected outcome
4. Iterate until the user is satisfied → verify: step output matches expected outcome

**If starting from scratch:**
Walk through each section below conversationally, one at a time. Don't dump all questions at once.

For each section:
1. Briefly explain what you're capturing → verify: step output matches expected outcome
2. Ask relevant questions → verify: step output matches expected outcome
3. Confirm accuracy → verify: step output matches expected outcome
4. Move to the next → verify: step output matches expected outcome

Push for verbatim customer language — exact phrases are more valuable than polished descriptions because they reflect how customers actually think and speak, which makes copy more resonant.

---

## When NOT to use

- Product requirements / feature spec — use `product-spec`
- Engineering plan or architecture — use `senior-architect`
- Campaign-specific copy — use `copywriting` or `ad-creative`
- Pricing strategy decision — use `pricing-strategy`
- Sales enablement materials directly — use `sales-enablement` (but reference this context first)

## Red Flags

| Rationalization | Reality |
|---|---|
| "We do not need a positioning statement, we know what we sell" | Without one written down, every marketing asset drifts; force a single sentence |
| "Skip ICP, our product is for everyone" | "Everyone" = no one in marketing; force a specific firmographic + behavioral profile |
| "Competitor differentiation is obvious" | If you cannot state it in one sentence per competitor, neither can prospects |
| "Auto-draft from codebase is good enough, no review needed" | The user MUST review and correct — auto-drafts often miss positioning nuance |

## Output Contract

Finished output must contain:
- `.agents/product-marketing-context.md` file with sections: Positioning, ICP, Personas (if B2B), Key Messages, Objections, Competitive Differentiation, Proof Points
- One-sentence positioning statement at top of document
- ICP defined firmographically AND behaviorally
- 3-5 key messages, each tied to a customer pain
- Top 5 objections with response patterns
- Competitive differentiation: 1 sentence per top competitor
- Version note (date + summary of last update)

## Examples

**Example 1 — First-time setup from codebase**
- Input: "Create the product marketing context for this repo"
- Action: Scan README, landing page, package.json → auto-draft V1 → present to user for review → walk section-by-section → save to `.agents/product-marketing-context.md`
- Output: Filled-in context doc with placeholders flagged for user review, version 1.0 noted

**Example 2 — Update existing context after launching new feature**
- Input: "We just launched AI assistant — update the context"
- Action: Read existing doc → identify sections impacted (Positioning, Key Messages, Differentiation) → propose updates → confirm → save new version
- Output: Updated context doc, diff explanation, version 1.4 with changelog

## References

Extended sections moved to `references/details.md`.
