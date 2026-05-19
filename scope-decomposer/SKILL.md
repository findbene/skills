---
name: scope-decomposer
description: 'Project scope decomposition into Epics and Stories for sprint planning, backlog creation, and work estimation. Triggers: "use scope-decomposer", "scope decomposer", "scope task".'
allowed-tools: Glob, Grep, Read
---

# Scope Decomposer

Transform scope into Epics → Stories in one pass.

## Process

### Input: Project Scope
```markdown
## Project: [Name]
**Vision:** [One sentence]
**Goals:** 1. Goal 1  2. Goal 2
**In Scope:** Feature A, Feature B
**Out of Scope:** Not this, Not that
```

### Step 1: Generate Epics (3-7)

```markdown
## Epic: [E1] User Authentication
**Goal:** Secure user access to the platform
**Business Value:** Foundation for all user features
**Size:** L (2-3 sprints)
**Success Criteria:**
- Users can register and login
- Sessions are secure
```

### Step 2: Decompose to Stories (3-5 per Epic)

```markdown
#### [E1-S1] User Registration
**As a** new user **I want to** create an account **So that** I can access the platform

**Acceptance Criteria:**
- [ ] Email validation
- [ ] Password strength check
- [ ] Confirmation email sent

**Points:** 5
**Priority:** P1
```

### Step 3: RICE Prioritization

| Story | Reach | Impact | Confidence | Effort | Score |
|-------|-------|--------|------------|--------|-------|
| E1-S1 | 1000 | 3 | 90% | 5 | 540 |
| E1-S2 | 1000 | 3 | 95% | 3 | 950 |

**RICE Formula:** (Reach x Impact x Confidence) / Effort

### Priority Levels
- **P1:** Must have (MVP)
- **P2:** Should have (v1.0)
- **P3:** Could have (future)
- **P4:** Won't have (out of scope)

## Backlog Template

```markdown
# Product Backlog: [Project Name]

## Epics Overview
| ID | Epic | Stories | Points | Priority |
|----|------|---------|--------|----------|
| E1 | Auth | 5 | 21 | P1 |

## Sprint Planning
### Sprint 1 (13 pts)
- E1-S1: User Registration (5)
- E1-S2: User Login (3)
- E1-S3: Password Reset (5)
```

## Best Practices

- 3-7 Epics (more means scope creep)
- 3-5 Stories per Epic (more means epic too large)
- INVEST criteria for every story
- Vertical slices delivering user value
- Clear in/out scope boundaries

## When NOT to use

- Task is unrelated to scope decomposer — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Scope Decomposer needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for scope decomposer
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving scope decomposer
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
