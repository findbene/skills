---
name: "api-test-suite-builder"
description: 'Use when the user asks to generate API tests, create integration test suites, test REST endpoints, or. Triggers: "use api-test-suite-builder", "build API test suite builder", "api test suite builder".'
allowed-tools: Bash, Glob, Grep, Read
---

# API Test Suite Builder

**Tier:** POWERFUL
**Category:** Engineering
**Domain:** Testing / API Quality

---

## Overview

Scans API route definitions across frameworks (Next.js App Router, Express, FastAPI, Django REST) and
auto-generates comprehensive test suites covering auth, input validation, error codes, pagination, file
uploads, and rate limiting. Outputs ready-to-run test files for Vitest+Supertest (Node) or Pytest+httpx
(Python).

---

## Core Capabilities

- **Route detection** — scan source files to extract all API endpoints
- **Auth coverage** — valid/invalid/expired tokens, missing auth header
- **Input validation** — missing fields, wrong types, boundary values, injection attempts
- **Error code matrix** — 400/401/403/404/422/500 for each route
- **Pagination** — first/last/empty/oversized pages
- **File uploads** — valid, oversized, wrong MIME type, empty
- **Rate limiting** — burst detection, per-user vs global limits

---

## When to Use

- New API added — generate test scaffold before writing implementation (TDD)
- Legacy API with no tests — scan and generate baseline coverage
- API contract review — verify existing tests match current route definitions
- Pre-release regression check — ensure all routes have at least smoke tests
- Security audit prep — generate adversarial input tests

---

## Route Detection

### Next.js App Router
```bash
# Find all route handlers
find ./app/api -name "route.ts" -o -name "route.js" | sort

# Extract HTTP methods from each route file
grep -rn "export async function\|export function" app/api/**/route.ts | \
  grep -oE "(GET|POST|PUT|PATCH|DELETE|HEAD|OPTIONS)" | sort -u

# Full route map
find ./app/api -name "route.ts" | while read f; do
  route=$(echo $f | sed 's|./app||' | sed 's|/route.ts||')
  methods=$(grep -oE "export (async )?function (GET|POST|PUT|PATCH|DELETE)" "$f" | \
    grep -oE "(GET|POST|PUT|PATCH|DELETE)")
  echo "$methods $route"
done
```

### Express
```bash
# Find all router files
find ./src -name "*.ts" -o -name "*.js" | xargs grep -l "router\.\(get\|post\|put\|delete\|patch\)" 2>/dev/null

# Extract routes with line numbers
grep -rn "router\.\(get\|post\|put\|delete\|patch\)\|app\.\(get\|post\|put\|delete\|patch\)" \
  src/ --include="*.ts" | grep -oE "(get|post|put|delete|patch)\(['\"][^'\"]*['\"]"

# Generate route map
grep -rn "router\.\|app\." src/ --include="*.ts" | \
  grep -oE "\.(get|post|put|delete|patch)\(['\"][^'\"]+['\"]" | \
  sed "s/\.\(.*\)('\(.*\)'/\U\1 \2/"
```

### FastAPI
```bash
# Find all route decorators
grep -rn "@app\.\|@router\." . --include="*.py" | \
  grep -E "@(app|router)\.(get|post|put|delete|patch)"

# Extract with path and function name
grep -rn "@\(app\|router\)\.\(get\|post\|put\|delete\|patch\)" . --include="*.py" | \
  grep -oE "@(app|router)\.(get|post|put|delete|patch)\(['\"][^'\"]*['\"]"
```

### Django REST Framework
```bash
# urlpatterns extraction
grep -rn "path\|re_path\|url(" . --include="*.py" | grep "urlpatterns" -A 50 | \
  grep -E "path\(['\"]" | grep -oE "['\"][^'\"]+['\"]" | head -40

# ViewSet router registration
grep -rn "router\.register\|DefaultRouter\|SimpleRouter" . --include="*.py"
```

---

## Test Generation Patterns

### Auth Test Matrix

For every authenticated endpoint, generate:

| Test Case | Expected Status |
|-----------|----------------|
| No Authorization header | 401 |
| Invalid token format | 401 |
| Valid token, wrong user role | 403 |
| Expired JWT token | 401 |
| Valid token, correct role | 2xx |
| Token from deleted user | 401 |

### Input Validation Matrix

For every POST/PUT/PATCH endpoint with a request body:

| Test Case | Expected Status |
|-----------|----------------|
| Empty body `{}` | 400 or 422 |
| Missing required fields (one at a time) | 400 or 422 |
| Wrong type (string where int expected) | 400 or 422 |
| Boundary: value at min-1 | 400 or 422 |
| Boundary: value at min | 2xx |
| Boundary: value at max | 2xx |
| Boundary: value at max+1 | 400 or 422 |
| SQL injection in string field | 400 or 200 (sanitized) |
| XSS payload in string field | 400 or 200 (sanitized) |
| Null values for required fields | 400 or 422 |

---

## Example Test Files
→ See references/example-test-files.md for details

## Generating Tests from Route Scan

When given a codebase, follow this process:

1. **Scan routes** using the detection commands above → verify: findings count > 0 OR clean signal returned
2. **Read each route handler** to understand: → verify: file content matches expected shape
   - Expected request body schema
   - Auth requirements (middleware, decorators)
   - Return types and status codes
   - Business rules (ownership, role checks)
3. **Generate test file** per route group using the patterns above → verify: output exists + parses without error
4. **Name tests descriptively**: `"returns 401 when token is expired"` not `"auth test 3"` → verify: all checks pass
5. **Use factories/fixtures** for test data — never hardcode IDs → verify: all checks pass
6. **Assert response shape**, not just status code → verify: step output matches expected outcome

---

## Common Pitfalls

- **Testing only happy paths** — 80% of bugs live in error paths; test those first
- **Hardcoded test data IDs** — use factories/fixtures; IDs change between environments
- **Shared state between tests** — always clean up in afterEach/afterAll
- **Testing implementation, not behavior** — test what the API returns, not how it does it
- **Missing boundary tests** — off-by-one errors are extremely common in pagination and limits
- **Not testing token expiry** — expired tokens behave differently from invalid ones
- **Ignoring Content-Type** — test that API rejects wrong content types (xml when json expected)

---

## Best Practices

1. One describe block per endpoint — keeps failures isolated and readable → verify: file content matches expected shape
2. Seed minimal data — don't load the entire DB; create only what the test needs → verify: file content matches expected shape
3. Use `beforeAll` for shared setup, `afterAll` for cleanup — not `beforeEach` for expensive ops → verify: step output matches expected outcome
4. Assert specific error messages/fields, not just status codes → verify: step output matches expected outcome
5. Test that sensitive fields (password, secret) are never in responses → verify: all checks pass
6. For auth tests, always test the "missing header" case separately from "invalid token" → verify: all checks pass
7. Add rate limit tests last — they can interfere with other test suites if run in parallel → verify: command exit code 0

## When NOT to use

- Task is unrelated to api test suite builder — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Api Test Suite Builder needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for api test suite builder
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow
