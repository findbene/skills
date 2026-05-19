---
name: stitch-mcp-list-design-systems
description: "Lists all Stitch Design Systems, optionally filtered by project. Triggers: 'use stitch-mcp-list-design-systems', 'stitch mcp list design systems', 'stitch-mcp-list-design-systems task'."
allowed-tools:
  - "stitch*:*"
---

# Stitch MCP — List Design Systems

Lists all available Stitch Design Systems. These are reusable theme configurations (colors, fonts, roundness, saturation) that can be applied to screens for visual consistency across a project.

## Critical prerequisite

**Only use this skill when the user explicitly mentions "Stitch".**

## When to use

- Before applying a design system — need to find the `assetId`
- User asks "what design systems do I have?"
- The orchestrator checks for existing design systems during project setup (Step 4b)
- Before creating a new design system — check if one already exists

## Call the MCP tool

```json
{
  "name": "list_design_systems",
  "arguments": {
    "projectId": "3780309359108792857"
  }
}
```

### `projectId` — numeric ID only, optional

```
✅ "3780309359108792857"
❌ "projects/3780309359108792857"
```

If omitted, returns all design systems across all projects.

## Output schema

Returns an array of Asset objects:

```json
{
  "assets": [
    {
      "name": "assets/ds_abc123",
      "displayName": "SaaS Dashboard Theme",
      "designSystem": {
        "theme": {
          "colorMode": "LIGHT",
          "font": "DM_SANS",
          "roundness": "ROUND_EIGHT",
          "saturation": 3,
          "customColor": "#6366F1",
          "backgroundLight": "#FFFFFF",
          "backgroundDark": "#18181B"
        },
        "designTokens": "...",
        "styleGuidelines": "..."
      }
    }
  ]
}
```

## After listing

Present as a readable table:

| # | Name | Font | Color | Mode | Asset ID |
|---|------|------|-------|------|----------|
| 1 | SaaS Dashboard Theme | DM Sans | #6366F1 | Light | `ds_abc123` |

Then offer:
- "Apply one of these to a screen?" → `stitch-mcp-apply-design-system`
- "Update an existing design system?" → `stitch-mcp-update-design-system`
- "Create a new design system?" → `stitch-mcp-create-design-system`

Extract the `name` field from each asset — this is the `assetId` needed for `apply_design_system`.

## When NOT to use

- Task is unrelated to stitch mcp list design systems — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Stitch Mcp List Design Systems needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for stitch mcp list design systems
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving stitch mcp list design systems
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
