---
name: feature-planning
description: "Feature planning specialist for breaking down feature requests into detailed implementation plans with tasks, dependencies, and acceptance criteria. Use this skill any time a new feature needs to be planned, implementation tasks need to be decomposed, or a feature spec needs to be turned into actionable work items. Trigger immediately on: \"plan this feature\", \"feature planning\", \"break this down\", \"implementation plan\", \"feature spec\", \"task breakdown\", \"user story\", \"acceptance criteria\", \"feature roadmap\", \"feature decomposition\", \"how do I build this\", \"what are the tasks\", \"feature scope\". If someone says \"plan how to build this feature\" or \"what tasks do I need?\" this skill MUST trigger."
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
1. Step 1
2. Step 2
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
1. Story 1.1: [User story]
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
