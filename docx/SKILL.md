---
name: docx
description: 'Create, read, edit, and format Word documents (.docx) using python-docx (Python) or the docx npm package (Node.js). Triggers: "use docx", "process docx", "docx".'
allowed-tools: Glob, Grep, Read
---

# DOCX — Word Document Processing

Create, read, and edit `.docx` files programmatically.

The primary library is `python-docx` for Python projects. Use the `docx` npm package if you're already in a Node.js context. For reading/converting existing documents, `pandoc` handles almost anything.

---

## When NOT to use

- Task is unrelated to docx — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Docx needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for docx
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving docx
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken

## References

Extended sections moved to `references/details.md`.
