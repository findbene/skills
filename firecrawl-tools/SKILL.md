---
name: firecrawl-tools
description: 'Firecrawl web scraping and data extraction via MCP for scraping websites, crawling pages, and extracting structured content from a. Triggers: "use firecrawl-tools", "firecrawl tools", "firecrawl task.'
allowed-tools: Glob, Grep, Read
---

# Firecrawl Tools

Scrape, crawl, and extract structured data from any website.

## Available Tools

| Tool | Purpose | When to Use |
|------|---------|-------------|
| `firecrawl_scrape` | Scrape a single URL to markdown/structured data | Reading a specific page |
| `firecrawl_crawl` | Crawl an entire site following links | Ingesting full site content |
| `firecrawl_map` | Get a sitemap of all URLs on a domain | Discovering pages before scraping |
| `firecrawl_extract` | Extract structured data using a schema | Pulling specific fields from pages |

## Common Workflows

### Read a Single Page
```
firecrawl_scrape(url="https://example.com/page")
```
Returns the page content as clean markdown, stripping navigation and ads.

### Discover and Scrape a Site
1. `firecrawl_map(url="https://example.com")` — get all URLs → verify: step output matches expected outcome
2. Filter to relevant pages → verify: step output matches expected outcome
3. `firecrawl_scrape` each target page → verify: step output matches expected outcome

### Extract Structured Data
```
firecrawl_extract(
  url="https://example.com/product",
  schema={"name": "string", "price": "number", "description": "string"}
)
```
Returns data matching the schema instead of raw markdown.

### Crawl an Entire Site
```
firecrawl_crawl(url="https://example.com", limit=50)
```
Follows links and returns content from up to `limit` pages.

## Tips

- Use `firecrawl_map` before `firecrawl_crawl` to understand site structure and set appropriate limits
- `firecrawl_scrape` is faster than `firecrawl_crawl` for single pages
- For documentation sites, crawl with a high limit to capture all pages
- Extracted structured data works best when the schema matches visible page content

## When NOT to use

- User wants to watch the page live — use wmux browser tools per project rules
- Single page-text extraction inside an existing browser session — `wmux browser get-text`
- Authenticated pages requiring session cookies — Firecrawl can't handle complex auth flows
- Pure search results — use `brave-search-tools` or web-search MCP
- Internal API data — use the API directly, not scraping

## Red Flags

| Thought | Reality |
|---------|---------|
| "`firecrawl_crawl` without limit" | Can balloon to thousands of pages and API cost; always set a limit |
| "Scrape with no schema for structured data" | Returns noisy markdown; use `firecrawl_extract` with a schema |
| "Skip `firecrawl_map`, just crawl the homepage" | Wastes budget on irrelevant pages; map first |
| "Hit it from main agent, drown the context" | Use a subagent to fetch + condense; pass only result back |

## Output Contract

Done when:
- Right tool chosen (scrape / crawl / map / extract) for the task
- Crawls have explicit `limit` set
- Structured extraction uses a typed schema
- Output condensed to relevant fields before passing back to main agent
- Errors handled (rate limit, 4xx) with retry or fallback

## Examples

### Example 1 — Extract product specs from a vendor site
- Input: "Get name + price + spec sheet from these 12 product pages"
- Action: `firecrawl_extract` per URL with `{ name, price, specs }` schema; collate into JSON
- Output: Clean JSON array of 12 products with typed fields

### Example 2 — Ingest a docs site into a wiki
- Input: "Scrape all docs from docs.acme.com"
- Action: `firecrawl_map` to enumerate URLs, filter to /docs/, `firecrawl_crawl` with limit set to enumerated count, store markdown per page
- Output: Per-page markdown files ready to feed the wiki-ingest pipeline
