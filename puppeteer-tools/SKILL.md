---
name: puppeteer-tools
description: "Puppeteer browser automation via MCP for web testing, scraping, PDF generation, and screenshot capture. Use this skill any time browser automation needs to be done with Puppeteer, web pages need to be scraped programmatically, screenshots need to be taken automatically, or PDF generation from HTML is required. Trigger immediately on: \"Puppeteer\", \"puppeteer\", \"headless browser\", \"browser automation Puppeteer\", \"Puppeteer scrape\", \"take screenshot\", \"PDF from HTML\", \"Puppeteer test\", \"Puppeteer MCP\", \"web page automation\", \"Puppeteer crawl\", \"automated screenshot\". If someone says \"use Puppeteer to automate this\" or \"take a screenshot of this page\" this skill MUST trigger."
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
