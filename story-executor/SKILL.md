---
name: story-executor
description: 'User story execution specialist for implementing stories from tasks through to completion with automated validation and acceptance crite. Triggers: "use story-executor", "story executor", "story task.'
allowed-tools: Glob, Grep, Read
---

# Story Executor

Automated story execution: Tasks → Implementation → Tests → Done.

## Execution Pipeline

### Phase 1: Task Breakdown
```markdown
## Story: [ID] [Title]

### Tasks
1. [ ] Task 1: [Action] - 2h → verify: user confirms
2. [ ] Task 2: [Action] - 3h → verify: user confirms
3. [ ] Write tests - 2h → verify: output exists + parses without error
4. [ ] Integration test - 1h → verify: all checks pass

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
1. E2E tests — happy path flows, critical user journeys → verify: all checks pass
2. Integration tests — API endpoints, database operations → verify: all checks pass
3. Unit tests — business logic, edge cases → verify: all checks pass

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

1. **Task Order:** Dependencies first → verify: user confirms
2. **Test Early:** Write tests with implementation → verify: output exists + parses without error
3. **Commit Often:** After each task → verify: git status clean
4. **Verify Always:** Run tests before marking done
5. **Document Blockers:** Immediately when found → verify: step output matches expected outcome

## When NOT to use

- Task is unrelated to story executor — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Story Executor needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for story executor
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving story executor
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
