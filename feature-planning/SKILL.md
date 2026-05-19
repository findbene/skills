---
name: feature-planning
description: 'Feature planning specialist for breaking down feature requests into detailed implementation plans with tasks, dependencies, and ac. Triggers: "use feature-planning", "feature planning", "feature task.'
allowed-tools: Glob, Grep, Read
---

# Feature Planning

Break features into implementable plans with clear tasks.

## Process

### 1. Understand the Feature
```markdown
## Feature Analysis
- **Goal:** What problem does this solve?
- **Users:** Who benefits?
- **Scope:** What's included/excluded?
- **Dependencies:** What must exist first?
```

### 2. Decompose into Epics
```markdown
## Epics (3-7 per feature)

### Epic 1: [Name]
**Goal:** [Outcome]
**Acceptance Criteria:**
- [ ] Criterion 1
- [ ] Criterion 2
```

### 3. Break Epics into Stories
```markdown
## Stories (3-5 per epic)

### Story 1.1: [As a user, I want...]
**Points:** [1-8]
**Acceptance Criteria:**
- [ ] Given/When/Then
**Technical Notes:**
- Implementation hints
```

### 4. Define Tasks
```markdown
## Tasks (1-6 per story)

### Task 1.1.1: [Action verb + component]
**Estimate:** [hours]
**Files:**
- src/components/Feature.tsx
- src/api/feature.ts
**Steps:**
1. Step 1 → verify: step output matches expected outcome
2. Step 2 → verify: step output matches expected outcome
**Done when:**
- [ ] Verification criteria
```

## Output Template

```markdown
# Feature: [Name]

## Overview
[Brief description]

## Epics

### Epic 1: Core Implementation
**Stories:**
1. Story 1.1: [User story] → verify: step output matches expected outcome
   - Task 1.1.1: Create data model
   - Task 1.1.2: Build API endpoint
   - Task 1.1.3: Implement UI component

### Epic 2: Integration
...

## Dependencies
- [ ] Prerequisite 1

## Risks
| Risk | Mitigation |
|------|-----------|
| Risk 1 | Plan |

## Timeline
| Phase | Duration | Deliverable |
|-------|----------|-------------|
| Phase 1 | X days | Epic 1 complete |
```

## Best Practices

- **INVEST stories:** Independent, Negotiable, Valuable, Estimable, Small, Testable
- **Vertical slices:** Each story delivers user value
- **Clear done criteria:** Measurable acceptance criteria
- **Right-size tasks:** 2-8 hours each
- **Dependencies explicit:** Block diagrams if complex

## When NOT to use

- Task is unrelated to feature planning — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Feature Planning needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for feature planning
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving feature planning
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
