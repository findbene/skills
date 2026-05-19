---
name: "report"
description: 'Generate test report. Use when user says "test report", "results summary", "test status", "show results", "test dashboard", or "how did tests go". Triggers: ''use report'', ''report'', ''report task.'
allowed-tools: Bash, Glob, Grep, Read
---

# Smart Test Reporting

Generate test reports that plug into the user's existing workflow. Zero new tools.

## Steps

### 1. Run Tests (If Not Already Run)

Check if recent test results exist:

```bash
ls -la test-results/ playwright-report/ 2>/dev/null
```

If no recent results, run tests:

```bash
npx playwright test --reporter=json,html,list 2>&1 | tee test-output.log
```

### 2. Parse Results

Read the JSON report:

```bash
npx playwright test --reporter=json 2> /dev/null
```

Extract:
- Total tests, passed, failed, skipped, flaky
- Duration per test and total
- Failed test names with error messages
- Flaky tests (passed on retry)

### 3. Detect Report Destination

Check what's configured and route automatically:

| Check | If found | Action |
|---|---|---|
| `TESTRAIL_URL` env var | TestRail configured | Push results via `/pw:testrail push` |
| `SLACK_WEBHOOK_URL` env var | Slack configured | Post summary to Slack |
| `.github/workflows/` | GitHub Actions | Results go to PR comment via artifacts |
| `playwright-report/` | HTML reporter | Open or serve the report |
| None of the above | Default | Generate markdown report |

### 4. Generate Report

#### Markdown Report (Always Generated)

```markdown
# Test Results — {{date}}

## Summary
- ✅ Passed: {{passed}}
- ❌ Failed: {{failed}}
- ⏭️ Skipped: {{skipped}}
- 🔄 Flaky: {{flaky}}
- ⏱️ Duration: {{duration}}

## Failed Tests
| Test | Error | File |
|---|---|---|
| {{name}} | {{error}} | {{file}}:{{line}} |

## Flaky Tests
| Test | Retries | File |
|---|---|---|
| {{name}} | {{retries}} | {{file}} |

## By Project
| Browser | Passed | Failed | Duration |
|---|---|---|---|
| Chromium | X | Y | Zs |
| Firefox | X | Y | Zs |
| WebKit | X | Y | Zs |
```

Save to `test-reports/{{date}}-report.md`.

#### Slack Summary (If Webhook Configured)

```bash
curl -X POST "$SLACK_WEBHOOK_URL" \
  -H 'Content-Type: application/json' \
  -d '{
    "text": "🧪 Test Results: ✅ {{passed}} | ❌ {{failed}} | ⏱️ {{duration}}\n{{failed_details}}"
  }'
```

#### TestRail Push (If Configured)

Invoke `/pw:testrail push` with the JSON results.

#### HTML Report

```bash
npx playwright show-report
```

Or if in CI:
```bash
echo "HTML report available at: playwright-report/index.html"
```

### 5. Trend Analysis (If Historical Data Exists)

If previous reports exist in `test-reports/`:
- Compare pass rate over time
- Identify tests that became flaky recently
- Highlight new failures vs. recurring failures

## Output

- Summary with pass/fail/skip/flaky counts
- Failed test details with error messages
- Report destination confirmation
- Trend comparison (if historical data available)
- Next action recommendation (fix failures or celebrate green)

## When NOT to use

- Task is unrelated to report — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Report needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for report
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving report
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
