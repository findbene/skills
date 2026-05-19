---
name: code-auditor
description: 'Comprehensive codebase analysis for architecture quality, technical debt, security vulnerabilities, and code health scoring across all files. Triggers: "use code-auditor", "code auditor", "code task".'
allowed-tools: Glob, Grep, Read
---

# Code Auditor

Comprehensive codebase analysis across 8 dimensions. Use this for full-project health assessments, not individual PR reviews.

## Audit Dimensions

### 1. Architecture Analysis
Evaluate pattern (Monolith/Microservices/Modular), layer separation, circular dependencies, and god classes. Recommend extraction and decomposition where needed.

### 2. Code Quality
Measure complexity, duplication, and tech debt. Identify hot spots — files with high complexity AND high churn are the highest-priority targets for refactoring.

### 3. Security Audit
Categorize findings by severity (Critical/High/Medium/Low). Common findings: SQL injection, missing rate limiting, exposed credentials, console logging of user data.

### 4. Performance Analysis
Check for N+1 queries, missing indexes, bundle size, unused exports, slow endpoints, and missing caching.

### 5. Testing Coverage
Map coverage by area (Services/API/Components/Utils). Identify gaps — missing E2E tests for critical flows and missing error state tests are the most common.

### 6. Dependencies
Flag outdated major versions, known vulnerabilities, and unused packages.

### 7. Documentation
Assess README, API docs, architecture docs, and onboarding guides for completeness and currency.

### 8. DevOps & Observability
Check CI/CD automation, security scanning, error tracking, logging, and metrics.

## Audit Report Template

```markdown
# Codebase Audit Report

## Executive Summary
- **Health Score:** X/100
- **Critical Issues:** N
- **High Priority:** N
- **Tech Debt:** ~N hours

## Top Priorities
1. [Critical] ... → verify: step output matches expected outcome
2. [High] ... → verify: step output matches expected outcome
3. [High] ... → verify: step output matches expected outcome

## Remediation Plan
| Issue | Effort | Impact | Priority |
|-------|--------|--------|----------|
| ...   | ...    | ...    | ...      |
```

Start with the executive summary — stakeholders need the headline before the details. Then drill into each dimension with specific findings and actionable recommendations.

## When NOT to use

- Task is unrelated to code auditor — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Code Auditor needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for code auditor
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving code auditor
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
