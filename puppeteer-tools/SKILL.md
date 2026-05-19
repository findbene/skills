---
name: puppeteer-tools
description: 'Puppeteer browser automation via MCP for web testing, scraping, PDF generation, and screenshot capture. Triggers: "use puppeteer-tools", "puppeteer tools", "puppeteer task".'
allowed-tools: Glob, Grep, Read
---

# Puppeteer Tools

Browser automation via Puppeteer MCP server — navigate, interact, screenshot, and scrape web pages.

## Available Tools

### Navigation & Screenshots
| Tool | Purpose |
|------|---------|
| `puppeteer_navigate` | Navigate to a URL |
| `puppeteer_screenshot` | Take a screenshot (full page or element) |

### Interaction
| Tool | Purpose |
|------|---------|
| `puppeteer_click` | Click an element by CSS selector |
| `puppeteer_fill` | Fill an input field with text |
| `puppeteer_select` | Select a dropdown option |
| `puppeteer_hover` | Hover over an element |

### JavaScript Execution
| Tool | Purpose |
|------|---------|
| `puppeteer_evaluate` | Execute JavaScript in the browser console |

## Common Workflows

### Take a Screenshot
```
puppeteer_navigate(url="http://localhost:3000")
puppeteer_screenshot(name="homepage", width=1280, height=720)
```

### Fill and Submit a Form
```
puppeteer_navigate(url="http://localhost:3000/rsvp")
puppeteer_fill(selector="#name", value="John Doe")
puppeteer_fill(selector="#email", value="john@example.com")
puppeteer_select(selector="#meal", value="vegetarian")
puppeteer_click(selector="button[type='submit']")
puppeteer_screenshot(name="rsvp-submitted")
```

### Extract Page Data
```
puppeteer_navigate(url="https://example.com")
puppeteer_evaluate(script="document.title")
puppeteer_evaluate(script="document.querySelectorAll('h1').length")
```

### Test Responsive Layout
```
puppeteer_navigate(url="http://localhost:3000")
puppeteer_screenshot(name="desktop", width=1440, height=900)
puppeteer_screenshot(name="tablet", width=768, height=1024)
puppeteer_screenshot(name="mobile", width=375, height=812)
```

### Wait for Dynamic Content
```
puppeteer_navigate(url="http://localhost:3000")
puppeteer_evaluate(script="await new Promise(r => setTimeout(r, 2000))")
puppeteer_screenshot(name="after-animation")
```

## Tips

- Screenshots are returned as base64 images — useful for visual regression testing
- Use CSS selectors for `click`, `fill`, `select`, and `hover`
- `puppeteer_evaluate` runs in the page context — you can access the DOM, `window`, `document`
- The browser launches in headless mode by default
- For complex scraping, use `puppeteer_evaluate` to extract structured data as JSON
- Combine with Firecrawl for large-scale scraping; use Puppeteer for interactive page automation

## When NOT to use

- Task is unrelated to puppeteer tools — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Puppeteer Tools needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for puppeteer tools
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving puppeteer tools
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
