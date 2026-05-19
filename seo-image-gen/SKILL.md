---
name: seo-image-gen
description: "'AI image generation for SEO assets using Gemini via nanobanana-mcp. Triggers: 'use seo-image-gen', 'optimize seo image gen', 'seo image gen'."
compatibility: Requires nanobanana-mcp MCP server for AI image generation
---

# SEO Image Gen: AI Image Generation for SEO Assets (Extension)

Generate production-ready images for SEO use cases using Gemini's image generation
via the banana Creative Director pipeline. Map SEO needs to optimized domain modes,
aspect ratios, and resolution defaults.

## Triggers

OG image, social preview image, Open Graph image, hero image, blog image, product photo, infographic, SEO image, create visual, banner, thumbnail, featured image, schema image, Pinterest pin, Twitter card image"
user-invokable: true
argument-hint: "[og|hero|product|infographic|custom|batch] <description>"
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - WebFetch
  - Write

## When NOT to use

- Task is unrelated to seo image gen — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Seo Image Gen needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for seo image gen
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving seo image gen
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken

## References

Extended sections moved to `references/details.md`.
