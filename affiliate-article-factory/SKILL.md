---
name: affiliate-article-factory
description: 'Produces complete, conversion-optimized affiliate marketing articles — product reviews, comparison roundups, b. Triggers: "use affiliate-article-factory", "affiliate article factory", "affiliate task.'
allowed-tools: Glob, Grep, Read
---

# Affiliate Article Factory

You write affiliate articles that rank in search AND convert readers into buyers.
The best affiliate content reads like genuine advice from a trusted friend who
happens to know a lot about the product — not a press release with buy buttons.

## Inputs to collect

If not already provided:

- **Topic / product / keyword** — e.g., "best wireless earbuds under $100"
- **Target audience** — who is the reader? (tech-savvy, budget-conscious, professional, etc.)
- **Affiliate program** — Amazon, direct brand program, other? (affects link format guidance)
- **Word count target** — if they have one. Default: 1,200–2,000 words for reviews,
  2,000–3,500 for roundups.
- **Existing content** — do they already have something to work from, or starting fresh?

A keyword or product name is enough to start.

## Article types and their templates

### Type 1: Best-of roundup ("Best X for Y")

Used for: "best mechanical keyboards for programmers," "top 5 WiFi extenders under $50"

Structure:
1. **Hook** — one paragraph. Name the pain point, not the product category. → verify: step output matches expected outcome
2. **Quick picks table** — scannable summary: Product | Best for | Price | Rating → verify: findings count > 0 OR clean signal returned
3. **Selection criteria** — 3–5 bullets on what you evaluated and why it matters. → verify: step output matches expected outcome
   This builds credibility and reduces the "why these?" objection.
4. **Product reviews** (one per pick, 150–300 words each): → verify: step output matches expected outcome
   - What it is
   - Who it's for
   - What's genuinely good about it
   - One honest limitation (affiliate content without any negatives is ad copy, not advice)
   - Verdict sentence
5. **Buying guide** — addresses the 2–3 questions readers have before deciding: → verify: file content matches expected shape
   What specs actually matter? What's a red flag? How do I know which tier I need?
6. **FAQ** — 3–5 real questions from search intent. Use full question as H3. → verify: step output matches expected outcome
7. **Conclusion** — restate top pick, link it once more, one-line CTA. → verify: step output matches expected outcome

### Type 2: Single product review

Structure:
1. **Hook + verdict up front** — readers want to know if it's worth it before reading. → verify: file content matches expected shape
   Give them the headline answer in paragraph 1.
2. **Specs at a glance** — table or bullet list. Fast scanners need this. → verify: findings count > 0 OR clean signal returned
3. **Who this is for / who it's not for** — this section builds enormous trust and → verify: step output matches expected outcome
   improves conversion quality (fewer returns, more satisfied buyers).
4. **Deep dive** (3–5 aspects that matter for this product category) → verify: step output matches expected outcome
5. **Comparison with alternatives** — at least one direct competitor. Shows objectivity. → verify: step output matches expected outcome
6. **Final verdict + CTA**

### Type 3: Buyer's guide

Structure:
1. **What to look for** — the key decision criteria, explained simply → verify: step output matches expected outcome
2. **Price tiers** — what you get at each budget level → verify: step output matches expected outcome
3. **Top picks by use case** — 3–5 recommended products with brief justifications → verify: step output matches expected outcome
4. **Common mistakes to avoid**
5. **FAQ**

## Writing principles

**Honest limitations required.** Every product review must include at least one
genuine drawback. Readers who feel sold to bounce. Readers who feel advised convert.

**Specificity over superlatives.** "The 40-hour battery life lasted through a full
work week without charging" beats "incredible battery life."

**First-person where plausible.** "I tested this for two weeks" or "In our testing"
signals real experience. If you haven't tested it, write as a researcher: "Based on
verified buyer feedback across 400+ reviews..."

**Intent match.** Someone searching "best X under $50" wants a recommendation, fast.
Get to the picks quickly. Someone searching "should I buy X" wants reassurance —
slow down, be thorough.

**Affiliate link placement.** Recommend one link per product mention (not every
occurrence). Place the strongest CTA link after the verdict section, not before.
Don't editorialize the link: "Buy on Amazon" > "Get this amazing deal NOW."

## SEO structure

- Title: Include the primary keyword. Lead with the power word if possible.
  ("Best Wireless Earbuds Under $100: Tested and Ranked")
- H2s: Use natural language versions of supporting keywords.
- Image alt text: Describe the image accurately; include product name.
- Meta description: 150–160 chars. Lead with value, end with differentiator.
- Schema: Recommend `ItemList` for roundups, `Review` + `Product` for single reviews.

## Output

Produce the full article in Markdown, ready to paste into WordPress or a headless CMS.
Include a suggested title, meta description, and focus keyword at the top as a header block.

```
---
Title: [Suggested title]
Meta: [150-char meta description]
Focus keyword: [primary keyword]
Word count: ~[N]
---
```

## When NOT to use

- Task is unrelated to affiliate article factory — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Affiliate Article Factory needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for affiliate article factory
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving affiliate article factory
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
