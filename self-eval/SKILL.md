---
name: "self-eval"
description: "Honestly evaluate AI work quality using a two-axis scoring system. Use after completing a task, code review, or work session to get an unbiase. Triggers: 'use self-eval', 'self eval', 'self-eval task."
allowed-tools: Glob, Grep, Read
license: "MIT"
---

# Self-Eval: Honest Work Evaluation

ultrathink

**Tier:** STANDARD
**Category:** Engineering / Quality
**Dependencies:** None (prompt-only, no external tools required)

## Examples

### Example 1: Feature Implementation

```
/self-eval added pagination to the user list API
```

Output:
```
## When NOT to use

- Task is unrelated to self eval — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Self Eval needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for self eval
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## References

Extended sections moved to `references/details.md`.
