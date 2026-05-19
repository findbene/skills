---
name: "browserstack"
description: 'Run tests on BrowserStack. Use when user mentions "browserstack", "cross-browser", "cloud testing", "browser matrix", "test on safari", "test on firefox", or "browser compatibility". Triggers: ''use browserstack'', ''browserstack'', ''browserstack task.'
allowed-tools: Bash, Glob, Grep, Read
---

# BrowserStack Integration

Run Playwright tests on BrowserStack's cloud grid for cross-browser and cross-device testing.

## Prerequisites

Environment variables must be set:
- `BROWSERSTACK_USERNAME` — your BrowserStack username
- `BROWSERSTACK_ACCESS_KEY` — your access key

If not set, inform the user how to get them from [browserstack.com/accounts/settings](https://www.browserstack.com/accounts/settings) and stop.

## Capabilities

### 1. Configure for BrowserStack

```
/pw:browserstack setup
```

Steps:
1. Check current `playwright.config.ts` → verify: all checks pass
2. Add BrowserStack connect options: → verify: dependency resolves + import works

```typescript
// Add to playwright.config.ts
import { defineConfig } from '@playwright/test';

const isBS = !!process.env.BROWSERSTACK_USERNAME;

export default defineConfig({
  // ... existing config
  projects: isBS ? [
    {
      name: "chromelatestwindows-11",
      use: {
        connectOptions: {
          wsEndpoint: `wss://cdp.browserstack.com/playwright?caps=${encodeURIComponent(JSON.stringify({
            'browser': 'chrome',
            'browser_version': 'latest',
            'os': 'Windows',
            'os_version': '11',
            'browserstack.username': process.env.BROWSERSTACK_USERNAME,
            'browserstack.accessKey': process.env.BROWSERSTACK_ACCESS_KEY,
          }))}`,
        },
      },
    },
    {
      name: "firefoxlatestwindows-11",
      use: {
        connectOptions: {
          wsEndpoint: `wss://cdp.browserstack.com/playwright?caps=${encodeURIComponent(JSON.stringify({
            'browser': 'playwright-firefox',
            'browser_version': 'latest',
            'os': 'Windows',
            'os_version': '11',
            'browserstack.username': process.env.BROWSERSTACK_USERNAME,
            'browserstack.accessKey': process.env.BROWSERSTACK_ACCESS_KEY,
          }))}`,
        },
      },
    },
    {
      name: "webkitlatestos-x-ventura",
      use: {
        connectOptions: {
          wsEndpoint: `wss://cdp.browserstack.com/playwright?caps=${encodeURIComponent(JSON.stringify({
            'browser': 'playwright-webkit',
            'browser_version': 'latest',
            'os': 'OS X',
            'os_version': 'Ventura',
            'browserstack.username': process.env.BROWSERSTACK_USERNAME,
            'browserstack.accessKey': process.env.BROWSERSTACK_ACCESS_KEY,
          }))}`,
        },
      },
    },
  ] : [
    // ... local projects fallback
  ],
});
```

3. Add npm script: `"test:e2e:cloud": "npx playwright test --project='chrome@*' --project='firefox@*' --project='webkit@*'"` → verify: dependency resolves + import works

### 2. Run Tests on BrowserStack

```
/pw:browserstack run
```

Steps:
1. Verify credentials are set
2. Run tests with BrowserStack projects: → verify: command exit code 0
   ```bash
   BROWSERSTACK_USERNAME=$BROWSERSTACK_USERNAME \
   BROWSERSTACK_ACCESS_KEY=$BROWSERSTACK_ACCESS_KEY \
   npx playwright test --project='chrome@*' --project='firefox@*'
   ```
3. Monitor execution → verify: step output matches expected outcome
4. Report results per browser → verify: step output matches expected outcome

### 3. Get Build Results

```
/pw:browserstack results
```

Steps:
1. Call `browserstack_get_builds` MCP tool → verify: step output matches expected outcome
2. Get latest build's sessions → verify: all checks pass
3. For each session: → verify: step output matches expected outcome
   - Status (pass/fail)
   - Browser and OS
   - Duration
   - Video URL
   - Log URLs
4. Format as summary table → verify: step output matches expected outcome

### 4. Check Available Browsers

```
/pw:browserstack browsers
```

Steps:
1. Call `browserstack_get_browsers` MCP tool → verify: step output matches expected outcome
2. Filter for Playwright-compatible browsers → verify: step output matches expected outcome
3. Display available browser/OS combinations → verify: step output matches expected outcome

### 5. Local Testing

```
/pw:browserstack local
```

For testing localhost or staging behind firewall:
1. Install BrowserStack Local: `npm install -D browserstack-local` → verify: dependency resolves + import works
2. Add local tunnel to config → verify: dependency resolves + import works
3. Provide setup instructions → verify: step output matches expected outcome

## MCP Tools Used

| Tool | When |
|---|---|
| `browserstack_get_plan` | Check account limits |
| `browserstack_get_browsers` | List available browsers |
| `browserstack_get_builds` | List recent builds |
| `browserstack_get_sessions` | Get sessions in a build |
| `browserstack_get_session` | Get session details (video, logs) |
| `browserstack_update_session` | Mark pass/fail |
| `browserstack_get_logs` | Get text/network logs |

## Output

- Cross-browser test results table
- Per-browser pass/fail status
- Links to BrowserStack dashboard for video/screenshots
- Any browser-specific failures highlighted

## When NOT to use

- Task is unrelated to browserstack — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Browserstack needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for browserstack
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving browserstack
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
