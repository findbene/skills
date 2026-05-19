---
name: stitch-mcp-get-project
description: "Retrieves metadata for a specific Stitch project — title, theme, create/update time. Triggers: 'use stitch-mcp-get-project', 'stitch mcp get project', 'stitch-mcp-get-project task'."
allowed-tools:
  - "stitch*:*"
---

# Stitch MCP — Get Project

Retrieves full metadata for a specific Stitch project. Useful for understanding an existing project's theme and screen list before generating additional screens.

## When to use

- User provides a Stitch project URL and you need its details
- You need to know the existing design theme before generating new consistent screens
- Verifying a project exists before proceeding

## When NOT to use

- Task is unrelated to stitch mcp get project — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Stitch Mcp Get Project needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for stitch mcp get project
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving stitch mcp get project
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken

## References

Extended sections moved to `references/details.md`.
