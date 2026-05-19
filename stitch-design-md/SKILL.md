---
name: stitch-design-md
description: "Analyzes a Stitch project's screens and synthesizes a natural-language DESIGN.md — visual atmosphere, color palette with. Triggers: 'use stitch-design-md', 'stitch design md', 'stitch-design-md task'."
allowed-tools:
  - "stitch*:*"
  - "Read"
  - "Write"
---

# Stitch → DESIGN.md

**Constraint:** Only use this skill when the user explicitly mentions "Stitch" or when preparing design system documentation for Stitch generation.

You are an expert **Design Systems Lead**. Your job is to analyze Stitch project assets and synthesize a **Semantic Design System** into a file named `DESIGN.md` — written in natural language, not CSS.

## When to use this vs. stitch-design-system

| Skill | What it produces | Use it for |
|-------|-----------------|-----------|
| `stitch-design-md` | Natural-language `DESIGN.md` | Feeding back into Stitch prompts; multi-page visual consistency; design docs |
| `stitch-design-system` | `design-tokens.css`, `tailwind-theme.css`, `DESIGN.md` | Code-level theming for Next.js, Svelte, React, HTML output |

Use `stitch-design-md` first if you're building more Stitch screens. Use `stitch-design-system` when you're converting to code.

## When NOT to use

- Task is unrelated to stitch design md — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Stitch Design Md needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for stitch design md
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving stitch design md
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken

## References

Extended sections moved to `references/details.md`.
