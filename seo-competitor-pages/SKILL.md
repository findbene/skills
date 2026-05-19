---
name: seo-competitor-pages
description: 'Generate and optimize SEO competitor comparison and alternatives pages that convert. Triggers: "use seo-competitor-pages", "optimize seo competitor pages", "seo competitor pages".'
user-invokable: true
argument-hint: "[url or generate] [competitor]"
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - WebFetch
---

# Competitor Comparison & Alternatives Pages

Create high-converting comparison and alternatives pages that target
competitive intent keywords with accurate, structured content.

## When NOT to use

- General SEO audit on a single page — use `seo-audit` or `seo-page`
- Non-comparison content (blog post, how-to) — use `seo-content`
- AI/LLM citation optimization — use `seo-geo`
- Local SEO comparison — use `seo-local`
- Writing competitor ads / battlecards (sales) — use `competitive-intel` or `competitor-alternatives`

## Red Flags

| Rationalization | Reality |
|---|---|
| "Just trash the competitor, we are obviously better" | Biased comparison pages get demoted by Google and lose trust; use a verifiable feature matrix and concede where competitor wins |
| "Pricing data is stale, ship anyway" | Outdated pricing is a credibility killer and trademark/legal risk; add "as of [date]" and quarterly review cadence |
| "Skip schema markup, the page already ranks" | Comparison/Review schema lifts SERP CTR ~15-25%; absence is a free win left on the table |
| "Just compare us vs them, skip the third alternative" | Single-competitor pages convert worse than 3-5 alternative pages; users compare more than two options |

## Output Contract

Finished output must contain:
- Page type explicitly identified (vs, alternatives, roundup, matrix)
- Feature matrix table with verifiable data and source links per row
- Pricing freshness note ("as of [date]") and review cadence
- Balanced verdict — at least one category where each competitor wins
- Target keyword listed in `<h1>`, slug, and meta title
- Schema markup recommendation (Review, Product, ItemList, FAQPage as appropriate)
- Internal-link suggestions to product pages and to related comparisons

## Examples

**Example 1 — Generate "Notion vs Coda" page**
- Input: `/seo-competitor-pages generate Notion-vs-Coda`
- Action: Fetch both product pages → build feature matrix (databases, formulas, AI, pricing, free tier, API) → write balanced verdict (Notion wins on docs, Coda wins on formulas) → propose Review schema → add 3 related comparison links
- Output: Markdown page with H1 "Notion vs Coda (2026 Comparison)", 12-row feature matrix with source URLs, pricing dated, FAQ section, Review schema JSON-LD

**Example 2 — Audit existing "Best CRM tools 2026" page**
- Input: `/seo-competitor-pages https://example.com/best-crm-tools`
- Action: WebFetch the page → check ranking criteria transparency → verify pricing freshness → check schema → identify missing alternatives → flag bias toward own product
- Output: Audit report listing 5 issues (criteria not stated upfront, 2 prices outdated, missing ItemList schema, 3 alternatives missing, conclusion too promotional), each with fix

## References

Extended sections moved to `references/details.md`.
