---
name: context-optimization
description: "Manage, compress, and recover AI session context to prevent information loss from compaction. Trigger: remember this for next time, save our progress, what have we decided so far, pick up where we le."
allowed-tools: Glob, Grep, Read
---

# Context Optimization

Manages the flow of information into, through, and out of AI sessions — keeping context lean, preventing information loss from compaction, and enabling clean session handoffs.

The core problem: LLM context windows fill up. When they do, older turns get compressed or dropped. Decisions made early in a session vanish. The agent starts re-asking questions. Work gets repeated. A well-run session anticipates this and externalizes the right things before they're lost.

---

## When NOT to use

- Task is unrelated to context optimization — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Context Optimization needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for context optimization
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving context optimization
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken

## References

Extended sections moved to `references/details.md`.
