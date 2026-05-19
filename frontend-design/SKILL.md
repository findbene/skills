---
name: frontend-design
description: "High-quality frontend UI design for creating distinctive, production-grade interfaces with modern aesthetics, strong visual. Triggers: 'use frontend-design', 'frontend design', 'frontend-design task'."
allowed-tools: Glob, Grep, Read
version: 2.0.0
triggers:
  - build a website
  - create a component
  - landing page
  - dashboard
  - make it look good
  - beautiful UI
  - design a page
  - frontend
  - HTML CSS
  - React component
  - styled component
  - improve the design
  - make it prettier
  - UI design
references: []
---

# Frontend Design Skill

## Triggers

\\\"frontend design\\\", \\\"UI design\\\", \\\"design this interface\\\", \\\"create a UI\\\", \\\"visual design\\\", \\\"design mockup\\\", \\\"UI mockup\\\", \\\"design the frontend\\\", \\\"make this look good\\\", \\\"U...

## When NOT to use

- Task is unrelated to frontend design — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Frontend Design needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for frontend design
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving frontend design
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken

## References

Extended sections moved to `references/details.md`.
