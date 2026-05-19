---
name: brave-search-tools
description: 'Real-time web search using Brave Search for current information, research, news, and fact-checking via MCP. Triggers: "use brave-search-tools", "brave search tools", "brave task".'
allowed-tools: Glob, Grep, Read
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
1. `brave_web_search` with a broad query to understand the landscape → verify: step output matches expected outcome
2. Review snippets to identify the most relevant sources → verify: step output matches expected outcome
3. Use `firecrawl_scrape` on the best URLs for full content → verify: step output matches expected outcome

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

## When NOT to use

- Task is unrelated to brave search tools — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Brave Search Tools needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for brave search tools
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving brave search tools
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
