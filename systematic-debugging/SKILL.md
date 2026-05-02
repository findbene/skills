---
name: systematic-debugging
description: "Four-phase debugging methodology (Reproduce, Isolate, Diagnose, Fix) for finding root causes before writing fixes. Use this skill any time a bug is being investigated, an error is occurring and the cause is unknown, or someone has been trying random fixes without success. Trigger immediately on: \"debug\", \"bug\", \"not working\", \"broken\", \"error\", \"exception\", \"crashes\", \"unexpected behavior\", \"stack trace\", \"why is this failing\", \"keeps throwing\", \"troubleshoot\", \"fix this issue\", \"something is wrong\". If someone pastes an error message or says \"it is broken and I do not know why\" this skill MUST trigger."
---

# Systematic Debugging

A disciplined four-phase approach centered on finding root causes before implementing fixes.

## The Iron Law

> **NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST**

Random fix attempts waste time and mask underlying issues. Always understand the problem before changing code.

## Phase 1: Root Cause Investigation

Before attempting ANY fix:

1. **Read error messages carefully** — every word matters
2. **Examine stack traces** — trace execution path
3. **Reproduce consistently** — can you trigger it reliably?
4. **Review recent changes** — what changed before the bug appeared?
5. **Gather diagnostic evidence** — logs, metrics, state snapshots
6. **Trace data flow** — follow data backward through the call stack

### Investigation Questions
- What is the expected vs actual behavior?
- When did this start happening?
- What conditions trigger it?

## Phase 2: Pattern Analysis

1. Find similar working code — what does correct behavior look like?
2. Study reference implementations completely
3. Document differences between working and broken
4. Map dependencies
5. List assumptions that must be true

## Phase 3: Hypothesis and Testing

1. **Form a specific hypothesis** — write it down
2. **Design a minimal test** — change ONE variable
3. **Predict the outcome** — what should happen if you're right?
4. **Execute the test** — make the minimal change
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

1. Write a failing test first — capture the bug
2. Implement a single, focused fix — minimal change
3. Verify the fix — test passes
4. Check for regressions — other tests still pass
5. Document the fix

## Critical Checkpoint

> If you've attempted **3+ fixes** without success, STOP.

Question the architecture: Is the approach fundamentally flawed? Are you fixing symptoms, not causes?

## Red Flags Requiring Reset

Return to Phase 1 if you catch yourself thinking:
- "Quick fix for now..."
- "Just try changing X..."
- "Let me just add this workaround..."

## Anti-Patterns to Avoid

1. **Shotgun debugging** — random changes hoping something works
2. **Print statement overload** — adding logs without a hypothesis
3. **Tunnel vision** — fixating on one area without evidence
4. **Premature fixing** — changing code before understanding the problem
