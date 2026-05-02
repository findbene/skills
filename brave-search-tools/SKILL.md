---
name: brave-search-tools
description: "Real-time web search using Brave Search for current information, research, news, and fact-checking via MCP. Use this skill any time current information is needed from the web, real-time search needs to be performed, or up-to-date facts need to be retrieved. Trigger immediately on: \"search the web\", \"look this up\", \"current news\", \"latest information\", \"Brave Search\", \"web search\", \"find online\", \"search for\", \"look up\", \"real-time data\", \"recent events\", \"today news\", \"web results\", \"online search\". If someone says \"search for X\" or \"what is the latest on Y\" this skill MUST trigger."
---

# Brave Search Tools

Search the web for real-time information, research, and fact-checking.

## Available Tools

| Tool | Purpose | When to Use |
|------|---------|-------------|
| `brave_web_search` | Search the web and return results with snippets | General web queries, research, fact-checking |
| `brave_local_search` | Search for local businesses and places | Finding restaurants, stores, services nearby |

## Common Workflows

### General Web Search
```
brave_web_search(query="Next.js 15 new features 2025")
```
Returns titles, URLs, and text snippets from top results.

### Research a Topic
1. `brave_web_search` with a broad query to understand the landscape
2. Review snippets to identify the most relevant sources
3. Use `firecrawl_scrape` on the best URLs for full content

### Find Local Businesses
```
brave_local_search(query="Ethiopian restaurant Atlanta GA")
```
Returns business names, addresses, ratings, and hours.

### Fact-Check a Claim
```
brave_web_search(query="exact claim text site:reuters.com OR site:apnews.com")
```
Use site filters to target authoritative sources.

## Tips

- Keep queries specific — "React Server Components caching behavior" beats "React help"
- Add year or "latest" for time-sensitive topics
- Use `site:` operator to restrict results to specific domains
- Combine with firecrawl-tools: search to find URLs, then scrape for full content
- For local searches, include city/state for better results
