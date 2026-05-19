---
name: systematic-debugging
description: 'Four-phase debugging methodology (Reproduce, Isolate, Diagnose, Fix) for finding root causes before writing fixes. Triggers: "use systematic-debugging", "systematic debugging", "systematic task".'
allowed-tools: Glob, Grep, Read
---

# Systematic Debugging

A disciplined four-phase approach centered on finding root causes before implementing fixes.

## The Iron Law

> **NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST**

Random fix attempts waste time and mask underlying issues. Always understand the problem before changing code.

## Phase 1: Root Cause Investigation

Before attempting ANY fix:

1. **Read error messages carefully** — every word matters → verify: file content matches expected shape
2. **Examine stack traces** — trace execution path → verify: step output matches expected outcome
3. **Reproduce consistently** — can you trigger it reliably? → verify: output exists + parses without error
4. **Review recent changes** — what changed before the bug appeared? → verify: step output matches expected outcome
5. **Gather diagnostic evidence** — logs, metrics, state snapshots → verify: step output matches expected outcome
6. **Trace data flow** — follow data backward through the call stack → verify: step output matches expected outcome

### Investigation Questions
- What is the expected vs actual behavior?
- When did this start happening?
- What conditions trigger it?

## Phase 2: Pattern Analysis

1. Find similar working code — what does correct behavior look like? → verify: step output matches expected outcome
2. Study reference implementations completely → verify: step output matches expected outcome
3. Document differences between working and broken → verify: step output matches expected outcome
4. Map dependencies → verify: step output matches expected outcome
5. List assumptions that must be true → verify: step output matches expected outcome

## Phase 3: Hypothesis and Testing

1. **Form a specific hypothesis** — write it down → verify: output exists + parses without error
2. **Design a minimal test** — change ONE variable → verify: all checks pass
3. **Predict the outcome** — what should happen if you're right? → verify: step output matches expected outcome
4. **Execute the test** — make the minimal change → verify: command exit code 0
5. **Verify results** — did it match prediction?
6. **Iterate or proceed**

### Hypothesis Format
```markdown
**Hypothesis**: [Specific cause]
**Test**: [Minimal change to verify]
**Expected**: [Predicted outcome]
**Actual**: [What happened]
**Conclusion**: [Confirmed/Refuted/Needs more data]
```

## Phase 4: Implementation

1. Write a failing test first — capture the bug → verify: output exists + parses without error
2. Implement a single, focused fix — minimal change → verify: diff matches intended change
3. Verify the fix — test passes
4. Check for regressions — other tests still pass → verify: all checks pass
5. Document the fix → verify: diff matches intended change

## Critical Checkpoint

> If you've attempted **3+ fixes** without success, STOP.

Question the architecture: Is the approach fundamentally flawed? Are you fixing symptoms, not causes?

## Red Flags Requiring Reset

Return to Phase 1 if you catch yourself thinking:
- "Quick fix for now..."
- "Just try changing X..."
- "Let me just add this workaround..."

## Anti-Patterns to Avoid

1. **Shotgun debugging** — random changes hoping something works → verify: step output matches expected outcome
2. **Print statement overload** — adding logs without a hypothesis → verify: file content matches expected shape
3. **Tunnel vision** — fixating on one area without evidence → verify: diff matches intended change
4. **Premature fixing** — changing code before understanding the problem → verify: diff matches intended change

## When NOT to use

- Task is unrelated to systematic debugging — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for systematic debugging
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving systematic debugging
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
