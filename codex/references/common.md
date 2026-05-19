# Codex — Shared Guidance

## When NOT to use

- Task needs a domain-specific skill, not Codex delegation
- Simple operation that doesn't need Codex overhead
- User explicitly asks for raw output without skill discipline
- Different toolchain/framework required — search `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Each Codex mode needs domain-specific judgment |
| "I'll inline the context" | Context drift produces stale output; use references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Codex run completed per mode's approval policy
- Every verify step passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate

## Examples

### Example 1 — golden path
- Input: standard user request
- Action: follow mode's numbered process with verify clauses at each step
- Output: deliverable matching Output Contract

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect gap, surface clarifying question OR document assumption explicitly
- Output: deliverable + explicit note on assumption/limitation
