---
name: "testrail"
description: 'Sync tests with TestRail. Use when user mentions "testrail", "test management", "test cases", "test run", "sync test cases", "push results to testrail", or "import from testrail". Triggers: ''use testrail'', ''testrail'', ''testrail task.'
allowed-tools: Bash, Glob, Grep, Read
---

# TestRail Integration

Bidirectional sync between Playwright tests and TestRail test management.

## Prerequisites

Environment variables must be set:
- `TESTRAIL_URL` — e.g., `https://your-instance.testrail.io`
- `TESTRAIL_USER` — your email
- `TESTRAIL_API_KEY` — API key from TestRail

If not set, inform the user how to configure them and stop.

## Capabilities

### 1. Import Test Cases → Generate Playwright Tests

```
/pw:testrail import --project <id> --suite <id>
```

Steps:
1. Call `testrail_get_cases` MCP tool to fetch test cases → verify: all checks pass
2. For each test case: → verify: all checks pass
   - Read title, preconditions, steps, expected results
   - Map to a Playwright test using appropriate template
   - Include TestRail case ID as test annotation: `test.info().annotations.push({ type: 'testrail', description: 'C12345' })`
3. Generate test files grouped by section → verify: output exists + parses without error
4. Report: X cases imported, Y tests generated → verify: output exists + parses without error

### 2. Push Test Results → TestRail

```
/pw:testrail push --run <id>
```

Steps:
1. Run Playwright tests with JSON reporter: → verify: command exit code 0
   ```bash
   npx playwright test --reporter=json > test-results.json
   ```
2. Parse results: map each test to its TestRail case ID (from annotations) → verify: all checks pass
3. Call `testrail_add_result` MCP tool for each test: → verify: all checks pass
   - Pass → status_id: 1
   - Fail → status_id: 5, include error message
   - Skip → status_id: 2
4. Report: X results pushed, Y passed, Z failed → verify: git status clean

### 3. Create Test Run

```
/pw:testrail run --project <id> --name "Sprint 42 Regression"
```

Steps:
1. Call `testrail_add_run` MCP tool → verify: command exit code 0
2. Include all test case IDs found in Playwright test annotations → verify: all checks pass
3. Return run ID for result pushing → verify: command exit code 0

### 4. Sync Status

```
/pw:testrail status --project <id>
```

Steps:
1. Fetch test cases from TestRail → verify: all checks pass
2. Scan local Playwright tests for TestRail annotations → verify: all checks pass
3. Report coverage: → verify: step output matches expected outcome
   ```
   TestRail cases: 150
   Playwright tests with TestRail IDs: 120
   Unlinked TestRail cases: 30
   Playwright tests without TestRail IDs: 15
   ```

### 5. Update Test Cases in TestRail

```
/pw:testrail update --case <id>
```

Steps:
1. Read the Playwright test for this case ID → verify: file content matches expected shape
2. Extract steps and expected results from test code → verify: all checks pass
3. Call `testrail_update_case` MCP tool to update steps → verify: all checks pass

## MCP Tools Used

| Tool | When |
|---|---|
| `testrail_get_projects` | List available projects |
| `testrail_get_suites` | List suites in project |
| `testrail_get_cases` | Read test cases |
| `testrail_add_case` | Create new test case |
| `testrail_update_case` | Update existing case |
| `testrail_add_run` | Create test run |
| `testrail_add_result` | Push individual result |
| `testrail_get_results` | Read historical results |

## Test Annotation Format

All Playwright tests linked to TestRail include:

```typescript
test('should login successfully', async ({ page }) => {
  test.info().annotations.push({
    type: 'testrail',
    description: 'C12345',
  });
  // ... test code
});
```

This annotation is the bridge between Playwright and TestRail.

## Output

- Operation summary with counts
- Any errors or unmatched cases
- Link to TestRail run/results

## When NOT to use

- Jira-based test management — use `jira-expert`
- Test writing without TestRail sync — use `senior-qa` or `webapp-testing`
- BrowserStack run reporting — use `browserstack`
- Coverage reports — use `coverage`
- Generic playwright result reporting — use `report` skill

## Red Flags

| Rationalization | Reality |
|---|---|
| "Hard-code TESTRAIL_API_KEY for speed" | Always env vars; committing API keys is a credential leak |
| "Match cases by title fuzzy" | Use stable Case IDs in test annotations; fuzzy matching causes silent mismatches |
| "Push results before verifying suite ID" | Wrong suite ID corrupts unrelated runs; always confirm IDs first |
| "Skip the operation summary" | Without counts and unmatched-case list, sync silently drops data |

## Output Contract

Finished output must contain:
- Env-var preflight (`TESTRAIL_URL`, `TESTRAIL_USER`, `TESTRAIL_API_KEY`) — fail fast if missing
- Operation type (import / export / sync) declared
- Case ID mapping (test file/title → TestRail case ID)
- Summary: total imported / exported / skipped / errored
- Unmatched cases listed (with reason: no annotation, missing in TestRail, etc.)
- Direct link to TestRail run/results
- Idempotent run-id (so re-running does not create duplicate runs)

## Examples

**Example 1 — Import TestRail cases as Playwright stubs**
- Input: `/pw:testrail import --project 1 --suite 42`
- Action: Verify env vars → fetch cases via TestRail API → generate Playwright test stubs with case ID annotations → write to `tests/` → produce summary
- Output: 87 test stubs created, 3 cases skipped (deprecated), link to TestRail suite

**Example 2 — Push Playwright run results to TestRail**
- Input: `/pw:testrail push --run 123 --results playwright-results.json`
- Action: Read Playwright JSON → map by case ID annotation → POST to TestRail run → 4 unmatched flagged
- Output: 50 results posted, 4 unmatched (listed with file paths), TestRail run link
