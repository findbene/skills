---
name: quality-gates
description: "Two-pass code quality gate system for verifying code quality, test coverage, security, and production-readiness before merging or deploying. Use this skill any time code quality needs to be verified before merging, a pre-deployment quality check is needed, or a systematic quality assessment needs to be run. Trigger immediately on: \"quality gate\", \"quality check\", \"is this ready to merge\", \"pre-merge check\", \"code quality report\", \"production-ready check\", \"quality assessment\", \"pass the gates\", \"pre-deployment check\", \"merge ready\", \"ship this\", \"is this done\", \"quality review\". If someone says \"is this ready to ship?\" or \"run the quality gates\" this skill MUST trigger."
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
1. [Specific improvement]
2. [Specific improvement]

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
