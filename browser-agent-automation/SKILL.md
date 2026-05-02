---
name: browser-agent-automation
description: "Browser automation strategies for Claude Code using Playwright, Puppeteer, and Claude-in-Chrome for testing, scraping, and web interaction. Use this skill any time browser automation needs to be implemented, web pages need to be tested automatically, or web interactions need to be scripted. Trigger immediately on: \"browser automation\", \"Playwright\", \"Puppeteer\", \"web scraping\", \"automated testing\", \"browser test\", \"web interaction\", \"form automation\", \"E2E test\", \"UI automation\", \"screenshot automation\", \"browser script\", \"Claude-in-Chrome\", \"automate browser\". If someone says \"automate this browser interaction\" or \"test this web page automatically\" this skill MUST trigger."
---

# Browser Agent Automation

Browser automation works for both development testing and everyday browser-based tasks.

## Tool Comparison

### Playwright MCP Server
- Traditional browser automation adapted for AI agents
- Agent must search, find, and interact with elements one at a time
- Can struggle with tool execution and consumes excessive context from screenshots

### Vercel Agent Browser CLI (Recommended)
- Built on Playwright but with a smarter interaction strategy
- Gives the agent an entire list of all pages and elements on the URL upfront
- Agent sees everything it can click before choosing to click
- Much higher success rate and more context-efficient

The difference isn't the underlying technology — it's the interaction strategy. Getting a full page map before interacting leads to better decisions than element-by-element searching.

## Browser Automation for Life Tasks

Browser automation extends beyond code testing to any browser-based task:

### Recurring Tasks
- Collect social media analytics (views, engagement across platforms)
- Check email and categorize messages
- Monitor prices (flights, products, apartments)
- Scrape competitor content for analysis

### One-Off Tasks
- Research apartments, flights, products
- Fill out forms
- Collect information from multiple sources
- Generate reports from web data

### Setup Pattern
1. Connect browser automation tool (Playwright MCP or Agent Browser CLI)
2. Create a systematic prompt describing the task
3. Optionally combine with Local Knowledge MCP to make the agent a domain specialist

## Key MCP Servers

| MCP Server | Purpose |
|-----------|---------|
| Playwright MCP | Browser automation and testing |
| Agent Browser CLI | Smart browser automation (recommended) |
| Local Knowledge MCP | Turn agent into domain specialist |
| Chrome Extension | Cloud Codework browser integration |

## Best Practices

1. **Start with a clear task description** — Systematic prompts produce better automation
2. **Prefer Agent Browser CLI** over raw Playwright for most tasks
3. **Combine with knowledge bases** for domain-specific browser tasks
4. **Use for both dev and life tasks** — Browser automation isn't limited to coding
5. **Monitor and iterate** — Complex browser flows may need refinement
