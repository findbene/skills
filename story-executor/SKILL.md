---
name: story-executor
description: "User story execution specialist for implementing stories from tasks through to completion with automated validation and acceptance criteria checking. Use this skill any time user stories need to be implemented, story acceptance criteria need to be met systematically, or structured story-driven development needs to be followed. Trigger immediately on: \"execute story\", \"story execution\", \"implement this story\", \"user story\", \"acceptance criteria\", \"story task\", \"story implementation\", \"story completion\", \"work on this story\", \"execute user story\", \"story workflow\", \"implement story\". If someone says \"implement this user story\" or \"work on story X\" this skill MUST trigger."
---

# Story Executor

Automated story execution: Tasks → Implementation → Tests → Done.

## Execution Pipeline

### Phase 1: Task Breakdown
```markdown
## Story: [ID] [Title]

### Tasks
1. [ ] Task 1: [Action] - 2h
2. [ ] Task 2: [Action] - 3h
3. [ ] Write tests - 2h
4. [ ] Integration test - 1h

**Total Estimate:** 8h
```

### Phase 2: Execute Each Task

For each task, document:
- Files to modify
- Implementation steps
- Verification criteria
- Status (pending/in-progress/complete)

### Phase 3: Test Execution (E2E First)

**Priority order:**
1. E2E tests — happy path flows, critical user journeys
2. Integration tests — API endpoints, database operations
3. Unit tests — business logic, edge cases

### Phase 4: Review & Rework

Self-review checklist:
- Code follows patterns
- Tests pass
- No console.logs
- Error handling complete
- Types correct

## Execution Template

```markdown
# Story Execution: [ID]
## Status: In Progress | Done | Blocked

## Tasks Progress
| # | Task | Est | Actual | Status |
|---|------|-----|--------|--------|
| 1 | Create model | 2h | 1.5h | Done |
| 2 | Build API | 3h | - | In Progress |

## Implementation Log
### Task 1: Create Model (Done)
- Created User type in types/user.ts
- Added validation schema
- Committed: abc123

## Done Criteria
- [ ] All tasks complete
- [ ] Tests passing
- [ ] Code reviewed
- [ ] Merged to main
```

## Automation Rules

1. **Task Order:** Dependencies first
2. **Test Early:** Write tests with implementation
3. **Commit Often:** After each task
4. **Verify Always:** Run tests before marking done
5. **Document Blockers:** Immediately when found
