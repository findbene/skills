---
name: browser-agent-automation
description: 'Browser automation strategies for Claude Code using Playwright, Puppeteer, and Claude-in-Chrome for testing, scrap. Triggers: "use browser-agent-automation", "browser agent automation", "browser task.'
allowed-tools: Glob, Grep, Read
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
1. Connect browser automation tool (Playwright MCP or Agent Browser CLI) → verify: step output matches expected outcome
2. Create a systematic prompt describing the task → verify: output exists + parses without error
3. Optionally combine with Local Knowledge MCP to make the agent a domain specialist → verify: step output matches expected outcome

## Key MCP Servers

| MCP Server | Purpose |
|-----------|---------|
| Playwright MCP | Browser automation and testing |
| Agent Browser CLI | Smart browser automation (recommended) |
| Local Knowledge MCP | Turn agent into domain specialist |
| Chrome Extension | Cloud Codework browser integration |

## Best Practices

1. **Start with a clear task description** — Systematic prompts produce better automation → verify: output exists + parses without error
2. **Prefer Agent Browser CLI** over raw Playwright for most tasks → verify: user confirms
3. **Combine with knowledge bases** for domain-specific browser tasks → verify: user confirms
4. **Use for both dev and life tasks** — Browser automation isn't limited to coding → verify: user confirms
5. **Monitor and iterate** — Complex browser flows may need refinement → verify: step output matches expected outcome

## When NOT to use

- Task is unrelated to browser agent automation — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Browser Agent Automation needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for browser agent automation
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving browser agent automation
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
