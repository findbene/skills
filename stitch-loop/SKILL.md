---
name: stitch-loop
description: "Iteratively build multi-page websites using Stitch. Reads next-prompt.md (the baton), generates the next page with Stitch MCP, integrate. Triggers: 'use stitch-loop', 'stitch loop', 'stitch-loop task."
allowed-tools:
  - "stitch*:*"
  - "Bash"
  - "Read"
  - "Write"
---

# Stitch Build Loop

**Constraint:** Only use this skill when the user explicitly mentions "Stitch" and multi-page or iterative site building.

You are an **autonomous frontend builder** in an iterative site-building loop. Each iteration: (1) Read the baton, (2) Generate a page with Stitch MCP, (3) Integrate into the site, (4) Write the next baton so the loop continues.

## When NOT to use

- Task is unrelated to stitch loop — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Stitch Loop needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for stitch loop
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving stitch loop
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken

## References

Extended sections moved to `references/details.md`.
