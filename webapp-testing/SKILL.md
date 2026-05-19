---
name: webapp-testing
description: 'Web application testing using Playwright for E2E tests, interaction automation, and functional testing against local dev servers. Triggers: "use webapp-testing", "webapp testing", "webapp task".'
allowed-tools: Bash, Glob, Grep, Read
---

# Web Application Testing

Test web applications using Playwright with Python.

## Decision Tree

```
Is it static HTML?
|- Yes -> Read HTML file directly, write Playwright script
|- No (dynamic webapp)
   |- Is server already running?
      |- No -> Use with_server.py helper
      |- Yes -> Reconnaissance-then-action:
               1. Navigate + wait for networkidle → verify: step output matches expected outcome
               2. Take screenshot or inspect DOM → verify: step output matches expected outcome
               3. Identify selectors → verify: step output matches expected outcome
               4. Execute actions → verify: command exit code 0
```

## Basic Playwright Script

```python
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.launch(headless=True)
    page = browser.new_page()
    page.goto('http://localhost:3000')
    page.wait_for_load_state('networkidle')  # CRITICAL for dynamic apps

    page.click('button[type="submit"]')
    page.fill('#email', 'test@example.com')
    page.screenshot(path='screenshot.png')

    browser.close()
```

## With Server Management

```bash
# Single server
python scripts/with_server.py \
    --server "npm run dev" --port 5173 \
    -- python your_automation.py

# Multiple servers (backend + frontend)
python scripts/with_server.py \
    --server "cd backend && python server.py" --port 3000 \
    --server "cd frontend && npm run dev" --port 5173 \
    -- python your_automation.py
```

## Reconnaissance Pattern

```python
page.wait_for_load_state('networkidle')
page.screenshot(path='/tmp/inspect.png', full_page=True)
content = page.content()
buttons = page.locator('button').all()
# Identify selectors from inspection, then execute actions
```

## Common Selectors

```python
page.click('text=Sign In')               # By text
page.click('role=button[name="Submit"]')  # By role
page.click('.submit-btn')                 # By CSS
page.click('[data-testid="submit"]')      # By test ID
```

## Best Practices

- Always use `headless=True` for automated tests
- Wait for `networkidle` before inspecting dynamic apps
- Use descriptive selectors (text, role, CSS, IDs)
- Capture screenshots for debugging failures
- Always close the browser when done

## When NOT to use

- Unit/component testing (no browser needed) — use `senior-qa` or Jest/Vitest
- Cross-browser cloud testing — use `browserstack` or `playwright-pro` (parallel devices)
- Accessibility-only check — use `a11y-audit`
- Mobile native app testing — use Appium / Detox, not Playwright
- Visual regression baseline management — Playwright works but use a dedicated VR service

## Red Flags

| Rationalization | Reality |
|---|---|
| "Use `page.waitForTimeout(2000)`" | Brittle; use `waitForSelector` / `waitForResponse` / `waitForLoadState('networkidle')` |
| "Run headed in CI for visibility" | Slower and flakier; run headless with traces/screenshots on failure |
| "Use XPath for everything" | Prefer `getByRole`, `getByText`, `getByLabel` — more resilient to refactors |
| "Skip the with_server helper, start server manually" | Manual server start causes race conditions; the helper waits for healthcheck |

## Output Contract

Finished output must contain:
- Playwright script with `sync_playwright()` or `async_playwright()` context manager
- Resilient selectors (role/label/text first, CSS as fallback)
- Explicit waits (`wait_for_selector`, `wait_for_load_state`) — no arbitrary sleeps
- Screenshot/trace artifacts captured on failure
- Browser closed in `finally` or via context manager
- Server lifecycle handled (with_server.py if dev server needed)

## Examples

**Example 1 — Test a Next.js login flow**
- Input: "Test login at localhost:3000"
- Action: Use `with_server.py` → launch Chromium headless → fill email + password → click "Sign in" → wait for dashboard → assert URL + text
- Output: Python test script, screenshot on failure, dev server started+stopped, all selectors role/label based

**Example 2 — Static HTML form submission**
- Input: "Test that this static HTML form submits and shows success"
- Action: Read HTML → write Playwright script with `file://` URL → fill inputs by label → click submit → assert success element visible
- Output: Self-contained Python script, no server needed, asserts on success state
