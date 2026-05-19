---
name: seo-dataforseo
description: "'Live SEO data via the DataForSEO MCP server (dataforseo-mcp-server, available via mcp-proxy). Triggers: 'use seo-dataforseo', 'optimize seo dataforseo', 'seo dataforseo'."
compatibility: Requires DATAFORSEO_LOGIN and DATAFORSEO_PASSWORD environment variables. DataForSEO API account needed (dataforseo.com).
---

# DataForSEO: Live SEO Data (Extension)

Live search data via the DataForSEO MCP server. Provides real-time SERP results,
keyword metrics, backlink profiles, on-page analysis, content analysis, business
listings, AI visibility checking, and LLM mention tracking across
9 API modules with 79 MCP tools.

## Prerequisites

This skill is served via **mcp-proxy** (`dataforseo-mcp-server`). No installation needed.

**Check availability:** Before using any DataForSEO tool, verify the MCP server
is connected by checking if `serp_organic_live_advanced` or any DataForSEO tool
is available in the current tool list.

**Credentials required:** The server needs `DATAFORSEO_LOGIN` and `DATAFORSEO_PASSWORD`
set in the mcp-proxy environment. If tools are available but return 401/auth errors,
ask the user to add their DataForSEO credentials to `~/.claude/mcp-proxy/config.json`
under the `"dataforseo"` → `"env"` block.

## API Credit Awareness

DataForSEO charges per API call. Be efficient:
- Prefer bulk endpoints over multiple single calls
- Use default parameters (US, English) unless user specifies otherwise
- Cache results mentally within a session; don't re-fetch the same data
- Warn user before running expensive operations (full backlink crawls, large keyword lists)

## Quick Reference

| Command | What it does |
|---------|-------------|
| `/seo dataforseo serp <keyword>` | Google organic SERP results |
| `/seo dataforseo serp-youtube <keyword>` | YouTube search results |
| `/seo dataforseo youtube <video_id>` | YouTube video deep analysis |
| `/seo dataforseo keywords <seed>` | Keyword ideas and suggestions |
| `/seo dataforseo volume <keywords>` | Search volume for keywords |
| `/seo dataforseo difficulty <keywords>` | Keyword difficulty scores |
| `/seo dataforseo intent <keywords>` | Search intent classification |
| `/seo dataforseo trends <keyword>` | Google Trends data |
| `/seo dataforseo backlinks <domain>` | Full backlink profile |
| `/seo dataforseo competitors <domain>` | Competitor domain analysis |
| `/seo dataforseo ranked <domain>` | Ranked keywords for domain |
| `/seo dataforseo intersection <domains>` | Keyword/backlink overlap |
| `/seo dataforseo traffic <domains>` | Bulk traffic estimation |
| `/seo dataforseo subdomains <domain>` | Subdomains with ranking data |
| `/seo dataforseo top-searches <domain>` | Top queries mentioning domain |
| `/seo dataforseo onpage <url>` | On-page analysis (Lighthouse + parsing) |
| `/seo dataforseo tech <domain>` | Technology stack detection |
| `/seo dataforseo whois <domain>` | WHOIS registration data |
| `/seo dataforseo content <keyword/url>` | Content analysis and trends |
| `/seo dataforseo listings <keyword>` | Business listings search |
| `/seo dataforseo ai-scrape <query>` | ChatGPT web scraper for GEO |
| `/seo dataforseo ai-mentions <keyword>` | LLM mention tracking for GEO |

---

## SERP Analysis

### `/seo dataforseo serp <keyword>`

Fetch Google organic search results in real time.

**MCP tools:** `serp_organic_live_advanced`

**Default parameters:** location_code=2840 (US), language_code=en, device=desktop, depth=100

**Also supports:** The `serp_organic_live_advanced` tool supports Google, Bing, and Yahoo via the `se` parameter. Specify "bing" or "yahoo" to switch search engines.

**Output:** Rank, URL, title, description, domain, featured snippets, AI overview references, People Also Ask.

### `/seo dataforseo serp-youtube <keyword>`

Fetch YouTube search results for GEO analysis. YouTube mentions correlate most strongly with AI citations.

**MCP tools:** `serp_youtube_organic_live_advanced`

**Output:** Video title, channel, views, upload date, description, URL.

### `/seo dataforseo youtube <video_id>`

Run deep analysis of a specific YouTube video: info, comments, and subtitles. YouTube mentions have the strongest correlation (0.737) with AI visibility, making this critical for GEO analysis.

**MCP tools:** `serp_youtube_video_info_live_advanced`, `serp_youtube_video_comments_live_advanced`, `serp_youtube_video_subtitles_live_advanced`

**Parameters:** video_id (the YouTube video ID, e.g., "dQw4w9WgXcQ")

**Output:** Video metadata (title, channel, views, likes, description), top comments with engagement, subtitle/transcript text.

---

## Keyword Research

### `/seo dataforseo keywords <seed>`

Generate keyword ideas, suggestions, and related terms from a seed keyword. Use when the user needs topic expansion or a keyword universe.

**MCP tools:** `dataforseo_labs_google_keyword_ideas`, `dataforseo_labs_google_keyword_suggestions`, `dataforseo_labs_google_related_keywords`

**Default parameters:** location_code=2840 (US), language_code=en, limit=50

**Output:** Keyword, search volume, CPC, competition level, keyword difficulty, trend.

### `/seo dataforseo volume <keywords>`

Pull search volume and metrics for a list of keywords.

**MCP tools:** `kw_data_google_ads_search_volume`

**Parameters:** keywords (array, comma-separated), location_code, language_code

**Output:** Keyword, monthly search volume, CPC, competition, monthly trend data.

### `/seo dataforseo difficulty <keywords>`

Score keyword difficulty to assess ranking competitiveness.

**MCP tools:** `dataforseo_labs_bulk_keyword_difficulty`

**Parameters:** keywords (array), location_code, language_code

**Output:** Keyword, difficulty score (0-100), interpretation (Easy/Medium/Hard/Very Hard).

### `/seo dataforseo intent <keywords>`

Classify keywords by search intent (informational, navigational, commercial, transactional).

**MCP tools:** `dataforseo_labs_search_intent`

**Parameters:** keywords (array), location_code, language_code

**Output:** Keyword, intent type (informational, navigational, commercial, transactional), confidence score.

### `/seo dataforseo trends <keyword>`

Analyze keyword trend direction and seasonality using Google Trends data.

**MCP tools:** `kw_data_google_trends_explore`

**Parameters:** keywords (array), location_code, date_from, date_to, language_code

**Output:** Keyword, time series data, trend direction, seasonality signals.

---

## Domain & Competitor Analysis

### `/seo dataforseo backlinks <domain>`

Pull a comprehensive backlink profile: total links, referring domains, spam score, anchors, and new/lost trends.

**MCP tools:** `backlinks_summary`, `backlinks_backlinks`, `backlinks_anchors`, `backlinks_referring_domains`, `backlinks_bulk_spam_score`, `backlinks_timeseries_summary`

**Default parameters:** limit=100 per sub-call

**Output:** Total backlinks, referring domains, domain rank, spam score, top anchors, new/lost backlinks over time, dofollow ratio, top referring domains.

### `/seo dataforseo competitors <domain>`

Identify competing domains, measure keyword overlap, and estimate their traffic.

**MCP tools:** `dataforseo_labs_google_competitors_domain`, `dataforseo_labs_google_domain_rank_overview`, `dataforseo_labs_bulk_traffic_estimation`

**Output:** Competitor domains, keyword overlap %, estimated traffic, domain rank, common keywords.

### `/seo dataforseo ranked <domain>`

List all keywords a domain ranks for with positions, URLs, and traffic share.

**MCP tools:** `dataforseo_labs_google_ranked_keywords`, `dataforseo_labs_google_relevant_pages`

**Default parameters:** limit=100, location_code=2840

**Output:** Keyword, position, URL, search volume, traffic share, SERP features.

### `/seo dataforseo intersection <domain1> <domain2> [...]`

Find shared keywords and backlink sources across 2-20 domains to expose keyword gaps and link-building opportunities.

**MCP tools:** `dataforseo_labs_google_domain_intersection`, `backlinks_domain_intersection`

**Parameters:** domains (2-20 array)

**Output:** Shared keywords with positions per domain, shared backlink sources, unique keywords per domain.

### `/seo dataforseo traffic <domains>`

Estimate organic search traffic and traffic cost for one or more domains.

**MCP tools:** `dataforseo_labs_bulk_traffic_estimation`

**Parameters:** domains (array)

**Output:** Domain, estimated organic traffic, estimated traffic cost, top keywords.

### `/seo dataforseo subdomains <domain>`

Enumerate all subdomains with their ranking data and estimated traffic.

**MCP tools:** `dataforseo_labs_google_subdomains`

**Parameters:** target (domain), location_code, language_code

**Output:** Subdomain, ranked keywords count, estimated traffic, organic cost.

### `/seo dataforseo top-searches <domain>`

Find the most popular search queries mentioning a specific domain in results.

**MCP tools:** `dataforseo_labs_google_top_searches`

**Parameters:** target (domain), location_code, language_code

**Output:** Query, search volume, domain position, SERP features, traffic share.

---

## Technical / On-Page

### `/seo dataforseo onpage <url>`

Run on-page analysis with Lighthouse audit and content parsing. Use when the user asks for a technical SEO audit, performance score, or Core Web Vitals check on a specific URL.

**MCP tools:** `on_page_instant_pages`, `on_page_content_parsing`, `on_page_lighthouse`

**Usage:**
- `on_page_instant_pages`:Quick page analysis (status codes, meta tags, content size, page timing, broken links, on-page checks)
- `on_page_content_parsing`:Extract and parse page content (plain text, word count, structure)
- `on_page_lighthouse`:Full Lighthouse audit (performance score, accessibility, best practices, SEO, Core Web Vitals)

**Output:** Pages crawled, status codes, meta tags, titles, content size, load times, Lighthouse scores, broken links, resource analysis.

### `/seo dataforseo tech <domain>`

Detect the full technology stack used on a domain (CMS, analytics, CDN, frameworks).

**MCP tools:** `domain_analytics_technologies_domain_technologies`

**Output:** Technology name, version, category (CMS, analytics, CDN, framework, etc.).

### `/seo dataforseo whois <domain>`

Retrieve WHOIS registration data for a domain.

**MCP tools:** `domain_analytics_whois_overview`

**Output:** Registrar, creation date, expiration date, nameservers, registrant info (if public).

---

## Content & Business Data

### `/seo dataforseo content <keyword/url>`

Analyze content quality, search content by topic, and track phrase trends over time.

**MCP tools:** `content_analysis_search`, `content_analysis_summary`, `content_analysis_phrase_trends`

**Parameters:** keyword (for search/trends) or URL (for summary)

**Output:** Content matches with quality scores, sentiment analysis, readability metrics, phrase trend data over time.

### `/seo dataforseo listings <keyword>`

Search business listings for local SEO competitive intelligence.

**MCP tools:** `business_data_business_listings_search`

**Parameters:** keyword, location (optional)

**Output:** Business name, description, category, address, phone, domain, rating, review count, claimed status.

---

## AI Visibility / GEO

### `/seo dataforseo ai-scrape <query>`

Scrape ChatGPT web search results for a query to see which sources it cites. Use for real GEO visibility checks on target keywords.

**MCP tools:** `ai_optimization_chat_gpt_scraper`

**Parameters:** query, location_code (optional), language_code (optional). Use `ai_optimization_chat_gpt_scraper_locations` to look up available locations.

**Output:** ChatGPT response content, cited sources/URLs, referenced domains.

### `/seo dataforseo ai-mentions <keyword>`

Track LLM mentions of brands, domains, and topics across multiple AI platforms. Use whenever the user needs actual AI visibility data — not estimated.

**MCP tools:** `ai_opt_llm_ment_search`, `ai_opt_llm_ment_top_domains`, `ai_opt_llm_ment_top_pages`, `ai_opt_llm_ment_agg_metrics`

**Parameters:** keyword, location_code (optional), language_code (optional). Use `ai_opt_llm_ment_loc_and_lang` for available locations/languages and `ai_optimization_llm_models` for supported LLM models.

**Workflow:**
1. Search LLM mentions with `ai_opt_llm_ment_search` (find mentions of a brand/keyword across LLM responses) → verify: step output matches expected outcome
2. Get top cited domains with `ai_opt_llm_ment_top_domains` (which domains are most cited for this topic) → verify: step output matches expected outcome
3. Get top cited pages with `ai_opt_llm_ment_top_pages` (which specific pages are most cited) → verify: step output matches expected outcome
4. Get aggregate metrics with `ai_opt_llm_ment_agg_metrics` (overall mention volume, trends) → verify: step output matches expected outcome

**Output:** LLM mention count, top cited domains with frequency, top cited pages, mention trends over time, cross-platform visibility scores.

**Advanced:** Use `ai_opt_llm_ment_cross_agg_metrics` for cross-model comparison (how mentions differ across ChatGPT, Claude, Perplexity, etc.).

---

## Available Utility Tools

Use these DataForSEO tools internally as needed — no dedicated commands:

- `serp_locations`:Location code lookups for SERP queries
- `serp_youtube_locations`:Location code lookups for YouTube queries
- `kw_data_google_ads_locations`:Location lookups for keyword data
- `kw_data_dfs_trends_demography`:Demographic data for trend analysis
- `kw_data_dfs_trends_subregion_interests`:Subregion interest data for trends
- `kw_data_dfs_trends_explore`:DFS proprietary trends data
- `kw_data_google_trends_categories`:Google Trends category lookups
- `dataforseo_labs_google_keyword_overview`:Quick keyword metrics overview
- `dataforseo_labs_google_historical_serp`:Historical SERP results for a keyword
- `dataforseo_labs_google_serp_competitors`:Competitors for a specific SERP
- `dataforseo_labs_google_keywords_for_site`:Keywords a site ranks for (alternative to ranked)
- `dataforseo_labs_google_page_intersection`:Page-level intersection analysis
- `dataforseo_labs_google_historical_rank_overview`:Historical domain rank data
- `dataforseo_labs_google_historical_keyword_data`:Historical keyword metrics
- `dataforseo_labs_available_filters`:Available filter options for Labs endpoints
- `backlinks_competitors`:Find domains with similar backlink profiles
- `backlinks_bulk_backlinks`:Bulk backlink counts for multiple targets
- `backlinks_bulk_new_lost_referring_domains`:Bulk new/lost referring domains
- `backlinks_bulk_new_lost_backlinks`:Bulk new/lost backlinks
- `backlinks_bulk_ranks`:Bulk rank overview for multiple targets
- `backlinks_bulk_referring_domains`:Bulk referring domain counts
- `backlinks_domain_pages_summary`:Summary of pages on a domain
- `backlinks_domain_pages`:List pages on a domain with backlink data
- `backlinks_page_intersection`:Shared backlink sources at page level
- `backlinks_referring_networks`:Referring network analysis
- `backlinks_timeseries_new_lost_summary`:Track new/lost backlinks over time
- `backlinks_bulk_pages_summary`:Bulk page summaries
- `backlinks_available_filters`:Available filter options for Backlinks endpoints
- `domain_analytics_whois_available_filters`:WHOIS filter options
- `domain_analytics_technologies_available_filters`:Technology detection filter options
- `ai_opt_kw_data_loc_and_lang`:AI optimization keyword data locations/languages
- `ai_optimization_keyword_data_search_volume`:AI-specific keyword volume data
- `ai_optimization_llm_response`:Direct LLM response analysis
- `ai_optimization_llm_mentions_filters`:Available filters for LLM mentions
- `ai_optimization_chat_gpt_scraper_locations`:Available locations for ChatGPT scraper

## Cross-Skill Integration

When DataForSEO MCP tools are available, use live data to augment other claude-seo skills:

- **seo-audit**:Spawn `seo-dataforseo` agent for real SERP, backlink, on-page, and listings data
- **seo-technical**:Use `on_page_instant_pages` / `on_page_lighthouse` for real crawl data, `domain_analytics_technologies_domain_technologies` for stack detection
- **seo-content**:Use `kw_data_google_ads_search_volume`, `dataforseo_labs_bulk_keyword_difficulty`, `dataforseo_labs_search_intent` for real keyword metrics, `content_analysis_summary` for content quality
- **seo-page**:Use `serp_organic_live_advanced` for real SERP positions, `backlinks_summary` for link data
- **seo-geo**:Use `ai_optimization_chat_gpt_scraper` for real ChatGPT visibility, `ai_opt_llm_ment_search` for LLM mention tracking
- **seo-plan**:Use `dataforseo_labs_google_competitors_domain`, `dataforseo_labs_google_domain_intersection`, `dataforseo_labs_bulk_traffic_estimation` for real competitive intelligence

## Error Handling

- **MCP server not connected**: Report that the DataForSEO MCP server is not reachable via mcp-proxy. Suggest checking that `dataforseo` is listed in `~/.claude/mcp-proxy/config.json` and that mcp-proxy is running
- **API authentication failed**: Report invalid credentials. Suggest checking DataForSEO API login/password in MCP config
- **Rate limit exceeded**: Report the limit hit and suggest waiting before retrying
- **No results returned**: Report "no data found" for the query rather than guessing. Suggest broadening the query or checking location/language codes
- **Invalid location code**: Report the error and suggest using the locations lookup tool to find the correct code

## Output Formatting

Match existing claude-seo output patterns:
- Use tables for comparative data
- Prioritize issues as Critical > High > Medium > Low
- Include specific, actionable recommendations
- Show scores as XX/100 where applicable
- Note data source as "DataForSEO (live)" to distinguish from static analysis

## Triggers

dataforseo, live SERP, real keyword data, actual search volume, backlink data, competitor keywords, AI visibility check, LLM mentions, real search data, keyword difficulty, check rankings, traffic estimate, who links to"
user-invokable: true
argument-hint: "[command] [query]"
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - WebFetch
  - Write

## When NOT to use

- Task is unrelated to seo dataforseo — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Seo Dataforseo needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for seo dataforseo
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow


## References

See `references/details.md` for extended sections.

## Examples

### Example 1 — Standard case
- Input: User invokes this skill for the typical use case
- Action: Follow the numbered process above end-to-end
- Output: Result matching the Output Contract

### Example 2 — Edge case
- Input: Unusual or boundary input matching the When-NOT triggers
- Action: Either route to the right skill or apply the documented fallback
- Output: Either correct hand-off or graceful no-op
