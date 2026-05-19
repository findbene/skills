---
name: tech-debt-tracker
description: 'Scan codebases for technical debt, score severity, track trends, and generate prioritized remediation plans. Triggers: "use tech-debt-tracker", "tech debt tracker", "tech task".'
allowed-tools: Glob, Grep, Read
---

# Tech Debt Tracker

**Tier**: POWERFUL 🔥  
**Category**: Engineering Process Automation  
**Expertise**: Code Quality, Technical Debt Management, Software Engineering

## Overview

Tech debt is one of the most insidious challenges in software development - it compounds over time, slowing down development velocity, increasing maintenance costs, and reducing code quality. This skill provides a comprehensive framework for identifying, analyzing, prioritizing, and tracking technical debt across codebases.

Tech debt isn't just about messy code - it encompasses architectural shortcuts, missing tests, outdated dependencies, documentation gaps, and infrastructure compromises. Like financial debt, it accrues "interest" through increased development time, higher bug rates, and reduced team velocity.

## What This Skill Provides

This skill offers three interconnected tools that form a complete tech debt management system:

1. **Debt Scanner** - Automatically identifies tech debt signals in your codebase → verify: findings count > 0 OR clean signal returned
2. **Debt Prioritizer** - Analyzes and prioritizes debt items using cost-of-delay frameworks → verify: step output matches expected outcome
3. **Debt Dashboard** - Tracks debt trends over time and provides executive reporting → verify: step output matches expected outcome

Together, these tools enable engineering teams to make data-driven decisions about tech debt, balancing new feature development with maintenance work.

## Technical Debt Classification Framework
→ See references/debt-frameworks.md for details

## Implementation Roadmap

### Phase 1: Foundation (Weeks 1-2)
1. Set up debt scanning infrastructure → verify: findings count > 0 OR clean signal returned
2. Establish debt taxonomy and scoring criteria → verify: step output matches expected outcome
3. Scan initial codebase and create baseline inventory → verify: output exists + parses without error
4. Train team on debt identification and reporting → verify: step output matches expected outcome

### Phase 2: Process Integration (Weeks 3-4)
1. Integrate debt tracking into sprint planning → verify: step output matches expected outcome
2. Establish debt budgets and allocation rules → verify: step output matches expected outcome
3. Create stakeholder reporting templates → verify: output exists + parses without error
4. Set up automated debt scanning in CI/CD → verify: findings count > 0 OR clean signal returned

### Phase 3: Optimization (Weeks 5-6)
1. Refine scoring algorithms based on team feedback → verify: step output matches expected outcome
2. Implement trend analysis and predictive metrics → verify: step output matches expected outcome
3. Create specialized debt reduction initiatives → verify: output exists + parses without error
4. Establish cross-team debt coordination processes → verify: step output matches expected outcome

### Phase 4: Maturity (Ongoing)
1. Continuous improvement of detection algorithms → verify: step output matches expected outcome
2. Advanced analytics and prediction models → verify: step output matches expected outcome
3. Integration with planning and project management tools → verify: step output matches expected outcome
4. Organization-wide debt management best practices → verify: step output matches expected outcome

## Success Criteria

**Quantitative Metrics:**
- 25% reduction in debt interest rate within 6 months
- 15% improvement in development velocity
- 30% reduction in production defects
- 20% faster code review cycles

**Qualitative Metrics:**
- Improved developer satisfaction scores
- Reduced context switching during feature development
- Faster onboarding for new team members
- Better predictability in feature delivery timelines

## Common Pitfalls and How to Avoid Them

### 1. Analysis Paralysis
**Problem**: Spending too much time analyzing debt instead of fixing it.
**Solution**: Set time limits for analysis, use "good enough" scoring for most items.

### 2. Perfectionism
**Problem**: Trying to eliminate all debt instead of managing it.
**Solution**: Focus on high-impact debt, accept that some debt is acceptable.

### 3. Ignoring Business Context
**Problem**: Prioritizing technical elegance over business value.
**Solution**: Always tie debt work to business outcomes and customer impact.

### 4. Inconsistent Application
**Problem**: Some teams adopt practices while others ignore them.
**Solution**: Make debt tracking part of standard development workflow.

### 5. Tool Over-Engineering
**Problem**: Building complex debt management systems that nobody uses.
**Solution**: Start simple, iterate based on actual usage patterns.

Technical debt management is not just about writing better code - it's about creating sustainable development practices that balance short-term delivery pressure with long-term system health. Use these tools and frameworks to make informed decisions about when and how to invest in debt reduction.

## When NOT to use

- Task is unrelated to tech debt tracker — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Tech Debt Tracker needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for tech debt tracker
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving tech debt tracker
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
