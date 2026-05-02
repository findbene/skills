---
name: code-auditor
description: "Comprehensive codebase analysis for architecture quality, technical debt, security vulnerabilities, and code health scoring across all files. Use this skill any time a full codebase needs to be audited, technical debt needs to be quantified, architecture health needs to be assessed, or a comprehensive quality report is needed. Trigger immediately on: \"code audit\", \"codebase audit\", \"technical debt\", \"code health\", \"audit the codebase\", \"code quality report\", \"architecture audit\", \"security audit\", \"code smell\", \"full codebase review\", \"refactoring priorities\", \"code quality score\", \"legacy code analysis\", \"codebase assessment\". If someone says \"audit our codebase\" or \"what is the state of our code?\" this skill MUST trigger."
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
1. [Critical] ...
2. [High] ...
3. [High] ...

## Remediation Plan
| Issue | Effort | Impact | Priority |
|-------|--------|--------|----------|
| ...   | ...    | ...    | ...      |
```

Start with the executive summary — stakeholders need the headline before the details. Then drill into each dimension with specific findings and actionable recommendations.
