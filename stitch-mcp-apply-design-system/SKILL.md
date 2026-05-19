---
name: stitch-mcp-apply-design-system
description: "Applies a Stitch Design System to existing screens — updates their colors, font. Triggers: 'use stitch-mcp-apply-design-system', 'stitch mcp apply design system', 'stitch-mcp-apply-design-system task."
allowed-tools:
  - "stitch*:*"
---

# Stitch MCP — Apply Design System

Applies a previously created Stitch Design System to one or more screens. This updates the screen's visual theme (colors, font, roundness) to match the design system, ensuring visual consistency across a project.

## Critical prerequisite

**Only use this skill when the user explicitly mentions "Stitch".**

You must have:
1. A `projectId` (numeric) → verify: step output matches expected outcome
2. One or more `screenId` values (numeric) → verify: step output matches expected outcome
3. An `assetId` from a design system (from `list_design_systems` or `create_design_system`) → verify: output exists + parses without error

## When to use

- After creating a design system and wanting to apply it to screens
- User says "make all screens match this theme" or "apply the design system"
- The orchestrator's Step 5b stores an assetId and offers to apply it
- Ensuring visual consistency across a multi-screen project

## Call the MCP tool

```json
{
  "name": "apply_design_system",
  "arguments": {
    "projectId": "3780309359108792857",
    "selectedScreenIds": ["88805abc123def456", "99906xyz789ghi012"],
    "assetId": "ds_abc123"
  }
}
```

## Parameter reference

### `projectId` — numeric ID only, no prefix

```
✅ "3780309359108792857"
❌ "projects/3780309359108792857"
```

### `selectedScreenIds` — array of numeric screen IDs

```
✅ ["88805abc123def456"]
❌ ["projects/123/screens/88805abc123def456"]
```

All selected screens will have the design system applied.

### `assetId` — the design system identifier

The `name` field from a design system asset, or just the ID portion:

```
✅ "ds_abc123"
```

Get this from:
- `stitch-mcp-list-design-systems` → extract from the `name` field of each asset
- `stitch-mcp-create-design-system` → returned in the response `name` field

## Output

Returns updated screen data reflecting the applied design system.

## After applying

1. Re-fetch affected screens: `stitch-mcp-get-screen` for each screenId → verify: step output matches expected outcome
2. Show updated screenshots to the user → verify: step output matches expected outcome
3. Offer:
   - "Edit the updated screens?" → `stitch-mcp-edit-screens`
   - "Convert to code?" → framework conversion
   - "Apply to more screens?" → another `apply_design_system` call

## When NOT to use

- Task is unrelated to stitch mcp apply design system — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Stitch Mcp Apply Design System needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for stitch mcp apply design system
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving stitch mcp apply design system
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
