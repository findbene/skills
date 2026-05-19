---
name: stitch-setup
description: "Step-by-step installer for the stitch-kit plugin and Stitch MCP server. Triggers: 'use stitch-setup', 'stitch setup', 'stitch-setup task'."
allowed-tools:
  - "Bash"
  - "Read"
  - "Write"
---

# stitch-kit Setup Guide

Walks users through two setup tasks:
1. **Stitch MCP Server** — connect your AI client to Google Stitch's remote generation API → verify: step output matches expected outcome
2. **stitch-kit Plugin** — install the skills that orchestrate the Stitch workflow → verify: dependency resolves + import works

---

## When to use this skill

- User says "how do I set this up", "it's not working", "Stitch isn't available"
- Agent can't find Stitch MCP tools after running `list_tools`
- First-time setup in a new environment

---

## When NOT to use

- Task is unrelated to stitch setup — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Stitch Setup needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for stitch setup
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving stitch setup
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken

## References

Extended sections moved to `references/details.md`.
