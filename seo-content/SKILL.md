---
name: seo-content
description: 'Content quality and E-E-A-T analysis with AI citation readiness assessment. Triggers: "use seo-content", "optimize seo content", "seo content".'
user-invokable: true
argument-hint: "[url]"
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - WebFetch
---

# Content Quality & E-E-A-T Analysis

## When NOT to use

- Technical SEO (crawl, indexation, schema) — use `seo-technical`
- Comparison or alternatives pages — use `seo-competitor-pages`
- AI/LLM citation analysis — use `seo-geo`
- Local pages and Google Business Profile — use `seo-local`
- Keyword research or content planning — use `seo-plan` or `seo-aeo-research-pack`

## Red Flags

| Rationalization | Reality |
|---|---|
| "Word count over 2000, must be good content" | Word count is not a ranking factor — comprehensive answers win, not padding |
| "AI-generated content is fine if it ranks" | Sept 2025 QRG penalizes low E-E-A-T; AI content without first-hand experience signals gets demoted |
| "Author bio not needed, brand is the author" | YMYL and most categories now require named expert authors with credentials |
| "Skip the Experience signals, we are a B2B SaaS" | Even B2B benefits from case studies, original data, before/after — these are the strongest Experience signal |

## Output Contract

Finished output must contain:
- E-E-A-T score broken down by each of the four pillars with rationale
- Word-count comparison vs page-type minimum (with reminder it is not a ranking factor)
- Readability score (Flesch reading ease) with target band
- List of missing Experience signals (no first-hand data, no case study, no original photos)
- Author/credential audit
- Specific fix list ranked by impact, not just "improve E-E-A-T"
- AI citation readiness note (passage length, structured answers)

## Examples

**Example 1 — Audit YMYL health article**
- Input: `/seo-content https://example.com/best-supplements-for-sleep`
- Action: WebFetch → score E-E-A-T → check for medical reviewer, citations to PubMed, author credentials, original data → measure readability → flag missing trust signals
- Output: E (2/5 — no first-hand testing), E (3/5 — author bio exists, no MD review), A (2/5 — few authoritative citations), T (3/5 — HTTPS but no medical disclaimer), 8-item fix list

**Example 2 — Pre-publish content review**
- Input: "Review this draft before I publish: [pasted 1800 words]"
- Action: Score against `references/eeat-framework.md` → check for buried answer (first 60 words) → check passage length for AI citation → check internal-link opportunities
- Output: E-E-A-T scorecard, readability 62 (good), 3 paragraphs flagged for AI-citability rewrite, 5 internal-link suggestions, 2 missing Experience signals

## References

Extended sections moved to `references/details.md`.
