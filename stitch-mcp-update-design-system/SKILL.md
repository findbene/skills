---
name: stitch-mcp-update-design-system
description: "Updates an existing Stitch Design System's theme, tokens, or guidelines. Triggers: 'use stitch-mcp-update-design-system', 'stitch mcp update design system', 'stitch-mcp-update-design-system task'."
allowed-tools:
  - "stitch*:*"
---

# Stitch MCP — Update Design System

Updates an existing Stitch Design System. Use this to modify theme properties, design tokens, or style guidelines without creating a new design system.

## Critical prerequisite

**Only use this skill when the user explicitly mentions "Stitch".**

You must have the design system's asset `name` before calling this. If you don't have one:
- List existing design systems via `stitch-mcp-list-design-systems`
- Or use the `name` returned from `stitch-mcp-create-design-system`

## When to use

- User wants to modify colors, fonts, or roundness of an existing design system
- After reviewing a design system and wanting to adjust specific properties
- Updating design tokens after a brand refresh

## Call the MCP tool

```json
{
  "name": "update_design_system",
  "arguments": {
    "designSystem": {
      "name": "assets/ds_abc123",
      "displayName": "SaaS Dashboard Theme v2",
      "theme": {
        "colorMode": "DARK",
        "font": "GEIST",
        "headlineFont": "GEIST",
        "bodyFont": "GEIST",
        "labelFont": "GEIST",
        "roundness": "ROUND_TWELVE",
        "saturation": 2,
        "customColor": "#818CF8",
        "backgroundLight": "#F9FAFB",
        "backgroundDark": "#09090B"
      },
      "designTokens": "--color-primary: #818CF8;\n--color-bg: #09090B;",
      "styleGuidelines": "Dark mode first. Geist font. Subtle indigo accent."
    }
  }
}
```

## Parameter reference

### `designSystem` — required, Asset wrapper

The object must include the `name` field (asset identifier) plus any fields you want to update:

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | **Yes** | Asset name from create or list (e.g., `assets/ds_abc123`) |
| `displayName` | string | No | Updated human-readable name |
| `theme` | DesignTheme | No | Updated visual configuration (see `stitch-mcp-create-design-system` for full reference) |
| `designTokens` | string | No | Updated CSS custom properties |
| `styleGuidelines` | string | No | Updated design rules |

**Note:** This is a full replacement, not a merge. Include all theme fields you want to keep, not just the ones you're changing.

## Output

Returns the updated Asset object:

```json
{
  "name": "assets/ds_abc123",
  "displayName": "SaaS Dashboard Theme v2",
  "designSystem": { ... }
}
```

## After updating

- Offer: "Re-apply this updated design system to existing screens?" → `stitch-mcp-apply-design-system`
- Screens generated before the update won't automatically reflect changes — they need to be re-applied

## When NOT to use

- Task is unrelated to stitch mcp update design system — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Stitch Mcp Update Design System needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for stitch mcp update design system
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving stitch mcp update design system
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
