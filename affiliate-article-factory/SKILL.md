---
name: affiliate-article-factory
description: >
  Produces complete, conversion-optimized affiliate marketing articles — product
  reviews, comparison roundups, best-of lists, and buyer's guides — ready to publish
  or lightly edit. Use this skill whenever the user wants to write affiliate content,
  product reviews, "best X for Y" articles, comparison pieces, or monetized blog
  posts. Also trigger when they mention Amazon Associates, affiliate links, product
  roundups, or want to create content for a niche site. Especially relevant for
  remotetechgear.com or similar affiliate properties.
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
1. **Hook** — one paragraph. Name the pain point, not the product category.
2. **Quick picks table** — scannable summary: Product | Best for | Price | Rating
3. **Selection criteria** — 3–5 bullets on what you evaluated and why it matters.
   This builds credibility and reduces the "why these?" objection.
4. **Product reviews** (one per pick, 150–300 words each):
   - What it is
   - Who it's for
   - What's genuinely good about it
   - One honest limitation (affiliate content without any negatives is ad copy, not advice)
   - Verdict sentence
5. **Buying guide** — addresses the 2–3 questions readers have before deciding:
   What specs actually matter? What's a red flag? How do I know which tier I need?
6. **FAQ** — 3–5 real questions from search intent. Use full question as H3.
7. **Conclusion** — restate top pick, link it once more, one-line CTA.

### Type 2: Single product review

Structure:
1. **Hook + verdict up front** — readers want to know if it's worth it before reading.
   Give them the headline answer in paragraph 1.
2. **Specs at a glance** — table or bullet list. Fast scanners need this.
3. **Who this is for / who it's not for** — this section builds enormous trust and
   improves conversion quality (fewer returns, more satisfied buyers).
4. **Deep dive** (3–5 aspects that matter for this product category)
5. **Comparison with alternatives** — at least one direct competitor. Shows objectivity.
6. **Final verdict + CTA**

### Type 3: Buyer's guide

Structure:
1. **What to look for** — the key decision criteria, explained simply
2. **Price tiers** — what you get at each budget level
3. **Top picks by use case** — 3–5 recommended products with brief justifications
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
