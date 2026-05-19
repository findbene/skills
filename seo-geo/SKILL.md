---
name: seo-geo
description: 'Generative Engine Optimization (GEO) and AI search visibility analysis. Optimizes content for Google AI Overviews, AI Mode (200+ countries), Cha. Triggers: "use seo-geo", "optimize seo geo", "seo geo.'
user-invokable: true
argument-hint: "[url]"
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - WebFetch
---

# AI Search / GEO Optimization (February 2026)

## When NOT to use

- Traditional Google SERP ranking (10 blue links) — use `seo-page` / `seo-technical`
- Content quality assessment beyond AI citability — use `seo-content`
- Schema markup setup only — use `seo-schema`
- Local pack / Map results optimization — use `seo-local`
- Keyword research not focused on AI answer engines — use `seo-plan`

## Red Flags

| Rationalization | Reality |
|---|---|
| "Backlinks still matter most" | Per Ahrefs Dec 2025 study, brand mentions correlate ~3x higher than backlinks for AI citations; rebalance off-page work |
| "ChatGPT and AI Overviews are basically the same" | Only 11% domain overlap on cited results — optimize per platform, do not assume transfer |
| "Skip llms.txt, robots.txt is enough" | llms.txt is rapidly becoming an expected file for LLM crawlers; absence excludes you from some pipelines |
| "Long paragraphs are fine, AI will summarize" | Optimal citable passage is 134-167 words and self-contained; long paragraphs get chunked badly |

## Output Contract

Finished output must contain:
- Citability score (passage length, self-contained answer blocks, first-40-words rule) with examples flagged
- Brand mention audit (YouTube, Reddit, Wikipedia, LinkedIn presence) vs backlink profile
- Platform-specific recommendation: AI Overviews, ChatGPT Search, Perplexity, Bing Copilot
- llms.txt audit (present? content quality?)
- robots.txt directives for AI crawlers (GPTBot, OAI-SearchBot, PerplexityBot, Google-Extended)
- Schema markup recommendations specifically for AI extraction (FAQPage, HowTo, Article with author + datePublished)
- Top 3 quick wins ranked by AI-visibility impact

## Examples

**Example 1 — AI visibility audit for a SaaS landing page**
- Input: `/seo-geo https://example.com/product`
- Action: WebFetch → measure passage length distribution → check for self-contained answer blocks → audit llms.txt → check robots.txt for GPTBot/PerplexityBot → identify brand mentions on Reddit/YouTube
- Output: Citability 38/100, brand mentions weak on YouTube (0) and Reddit (3), llms.txt missing, GPTBot allowed, 6 paragraphs flagged as not-citable, 3 quick wins (add llms.txt, restructure FAQ, publish 5 YouTube demos)

**Example 2 — Compare AI visibility across competitors**
- Input: `/seo-geo` for our homepage + 3 competitor URLs
- Action: Score each on citability + brand mention proxies → check who appears in Perplexity test queries → identify why competitor X gets cited and we do not
- Output: Comparison table, finding that competitor wins on Wikipedia presence and structured FAQ; recommendation to publish Wikipedia-style brand entity page and add FAQPage schema

## References

Extended sections moved to `references/details.md`.
