---
name: quality-gates
description: 'Two-pass code quality gate system for verifying code quality, test coverage, security, and production-readiness before merging or deploy. Triggers: "use quality-gates", "quality gates", "quality task.'
allowed-tools: Glob, Grep, Read
---

# Quality Gates

Two-pass quality assurance: Code Quality + Test Quality.

## Pass 1: Code Quality

### 1.1 Static Analysis
- ESLint passes (0 errors)
- Prettier formatted
- TypeScript strict mode, no `any` types
- Cyclomatic complexity < 10
- Function length < 50 lines
- File length < 300 lines
- Nesting depth < 4

### 1.2 Security Check
- No hardcoded secrets
- Dependencies scanned (npm audit)
- SQL injection prevention
- XSS prevention
- CSRF protection
- Input validation

### 1.3 Code Standards
- Single responsibility
- Dependency injection
- No circular imports
- Proper error handling
- Clear, descriptive naming
- No dead code or console.logs

## Pass 2: Test Quality

### 2.1 Coverage Gates

| Metric | Block | Warn | Pass |
|--------|-------|------|------|
| Statements | <70% | <80% | >=80% |
| Branches | <65% | <75% | >=75% |
| Functions | <70% | <80% | >=80% |

### 2.2 Test Standards
- Deterministic (no flaky tests)
- Isolated (no shared state)
- Fast execution
- Clear assertions and meaningful names

### 2.3 Test Distribution
All critical paths tested. Unit, integration, and E2E tests all passing.

## Quality Report Template

```markdown
# Quality Gate Report

## Summary
| Gate | Status | Score |
|------|--------|-------|
| Code Quality | PASS/FAIL | X/100 |
| Security | PASS/FAIL | X/100 |
| Test Coverage | PASS/WARN/FAIL | X/80 |
| Test Quality | PASS/FAIL | X/100 |

**Overall:** PASS/FAIL (X/100)

## Recommendations
1. [Specific improvement] → verify: step output matches expected outcome
2. [Specific improvement] → verify: step output matches expected outcome

## Approved By
- [ ] Code review complete
- [ ] QA sign-off
- [ ] Ready for merge
```

## Gate Thresholds

| Metric | Block | Warn | Pass |
|--------|-------|------|------|
| Lint Errors | >0 | - | 0 |
| Coverage | <70% | <80% | >=80% |
| Complexity | >15 | >10 | <=10 |
| Security | Critical | High | None |
| Test Failures | >0 | - | 0 |

## When NOT to use

- Task is unrelated to quality gates — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Quality Gates needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for quality gates
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving quality gates
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
