---
name: webapp-testing
description: "Web application testing using Playwright for E2E tests, interaction automation, and functional testing against local dev servers. Use this skill any time a web application needs to be tested with Playwright, E2E tests need to be written, or interactive web app testing needs to be automated against a running server. Trigger immediately on: \"test this webapp\", \"Playwright\", \"E2E test\", \"web app test\", \"browser test\", \"test locally\", \"playwright test\", \"functional test\", \"automated web test\", \"UI test\", \"test this page\", \"web test automation\", \"visual regression\", \"test the frontend\". If someone says \"test this web app\" or \"write Playwright tests for this\" this skill MUST trigger."
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
               1. Navigate + wait for networkidle
               2. Take screenshot or inspect DOM
               3. Identify selectors
               4. Execute actions
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
