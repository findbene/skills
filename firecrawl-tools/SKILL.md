---
name: firecrawl-tools
description: "Firecrawl web scraping and data extraction via MCP for scraping websites, crawling pages, and extracting structured content from any URL. Use this skill any time websites need to be scraped, web pages need to be crawled for data, structured data needs to be extracted from HTML, or web content needs to be monitored. Trigger immediately on: \"Firecrawl\", \"web scrape\", \"scrape this website\", \"crawl this site\", \"extract data from website\", \"web crawl\", \"scrape content\", \"firecrawl\", \"website data extraction\", \"scrape URL\", \"crawl pages\", \"web extraction\", \"scrape this page\". If someone says \"scrape this website\" or \"extract data from this URL\" this skill MUST trigger."
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
1. `firecrawl_map(url="https://example.com")` — get all URLs
2. Filter to relevant pages
3. `firecrawl_scrape` each target page

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
