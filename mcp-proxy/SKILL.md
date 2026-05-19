---
name: mcp-proxy
description: 'Smart proxy routing tool calls to 22 on-demand MCP servers (firecrawl, brave-search, n8n, elevenlabs, postgres, puppeteer, runway, pika, piapi,. Triggers: "use mcp-proxy", "use MCP proxy", "mcp proxy.'
allowed-tools: Glob, Grep, Read, Skill
---

# MCP Proxy

Meta-MCP server that routes tool calls to 22 on-demand MCP servers through a single entry point.

## Why This Exists

Without the proxy, 22 MCP servers load ~25,000 tokens of tool schemas into every session — whether used or not. The proxy replaces all 22 with 4 lightweight tools (~400 tokens), spawning actual MCPs only when needed.

## Available Tools

| Tool | Purpose |
|------|---------|
| `list_skills` | List all 22 available skills with descriptions and API key status |
| `get_skill_tools` | Spawn a skill's MCP and return its full tool schemas |
| `call_skill` | Execute a specific tool from any skill |
| `get_proxy_stats` | Session token usage stats and savings breakdown |

## Workflow

### 1. Discover Available Skills
```
list_skills → returns 22 skills with descriptions
```

### 2. Inspect a Skill's Tools
```
get_skill_tools({ skill_name: "firecrawl" })
→ spawns firecrawl MCP, returns its tool schemas, shuts down
```

### 3. Call a Tool
```
call_skill({
  skill_name: "firecrawl",
  tool_name: "firecrawl_scrape",
  arguments: { url: "https://example.com" }
})
→ spawns firecrawl, calls the tool, returns results, shuts down
```

## Available Skills (22)

| Skill | Description |
|-------|-------------|
| firecrawl | Scrape, crawl, extract data from websites |
| brave-search | Web search via Brave Search |
| sequential-thinking | Step-by-step reasoning chains |
| nanobanana | AI image generation via Google Gemini |
| figma | Extract design data from Figma |
| n8n | Manage n8n workflow automations |
| elevenlabs | Speech, voice cloning, sound effects |
| postgres | PostgreSQL database queries |
| puppeteer | Browser automation |
| runway | AI video/image generation (Runway ML) |
| pika | AI video generation (Pika Labs) |
| piapi | Multi-model AI media (MJ, Flux, Kling, etc.) |
| resemble | Voice cloning and TTS |
| heygen | AI avatar videos |
| synthesia | AI avatar videos and dubbing |
| linkedin | LinkedIn scraping |
| youtube | YouTube search and playlists |
| crewai | Multi-agent workflows |
| huggingface | HF Hub search |
| autogen | Microsoft AutoGen agents |
| llamaindex | LlamaCloud RAG queries |
| blotato | Social media publishing (9 platforms) |

## Multi-Step Example

Scrape a webpage then post a summary to social media:
```
1. call_skill({ skill_name: "firecrawl", tool_name: "firecrawl_scrape", arguments: { url: "..." } })
2. [Claude summarizes the scraped content]
3. call_skill({ skill_name: "blotato", tool_name: "publish_post", arguments: { accountId: "...", platform: "twitter", text: summary, mediaUrls: [] } })
```

## Performance Notes

- First call to npx-based skills is slower (~5-10s for package download). Subsequent calls use npm cache.
- Node-based skills (local scripts) spawn in ~1-2s.
- Each call spawns a fresh subprocess. No state persists between calls.
- Timeout: 30s for connection, 120s for tool execution.

## Architecture

```
settings.json
  ├── playwright (always-on, ~15 tools)
  ├── time (always-on, ~2 tools)
  ├── filesystem (always-on, ~10 tools)
  └── mcp-proxy (always-on, 4 tools)
        └── config.json → 22 on-demand MCPs
              ├── firecrawl (spawned when needed)
              ├── brave-search (spawned when needed)
              ├── ... (20 more)
              └── blotato (spawned when needed)
```

## Token Savings

| Metric | Before | After |
|--------|--------|-------|
| Always-on MCPs | 25 | 4 |
| Tools in context | ~137 | ~31 |
| Baseline tokens | ~34,000 | ~7,800 |
| Savings | — | ~26,000 tokens/session (76%) |

## When NOT to use

- Task is unrelated to mcp proxy — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Mcp Proxy needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for mcp proxy
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving mcp proxy
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
