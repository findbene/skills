---
name: figma-tools
description: 'Figma design data extraction via MCP for accessing layouts, components, styles, colors, and design specifications from Figma files. Triggers: "use figma-tools", "figma tools", "figma task".'
allowed-tools: Glob, Grep, Read
---

# Figma Tools

Extract design data from Figma files for code implementation.

## Available Tools

| Tool | Purpose | When to Use |
|------|---------|-------------|
| `figma_get_file` | Get full file structure and metadata | Understanding overall design structure |
| `figma_get_node` | Get a specific frame/component by node ID | Implementing a particular screen or component |
| `figma_get_styles` | Extract color, text, and effect styles | Building design tokens and theme config |
| `figma_get_components` | List all components in a file | Auditing the design system |
| `figma_get_images` | Export nodes as PNG/SVG/PDF | Getting assets for implementation |

## Common Workflows

### Implement a Design Screen
1. `figma_get_file` to understand overall file structure and find the target frame → verify: step output matches expected outcome
2. `figma_get_node(node_id="...")` for the specific frame to implement → verify: step output matches expected outcome
3. `figma_get_styles` to extract colors, fonts, spacing as CSS variables → verify: step output matches expected outcome
4. `figma_get_images` to export icons and images as SVG/PNG → verify: step output matches expected outcome
5. Write the component code using extracted values → verify: output file exists + no syntax error

### Extract Design Tokens
1. `figma_get_styles` to pull all named styles → verify: step output matches expected outcome
2. Map to CSS custom properties or a theme object: → verify: step output matches expected outcome
   - Colors → `--color-primary`, `--color-secondary`
   - Typography → font families, sizes, weights, line heights
   - Effects → shadows, blurs

### Audit Component Library
1. `figma_get_components` to list all components → verify: step output matches expected outcome
2. Compare against implemented components to find gaps → verify: step output matches expected outcome
3. `figma_get_node` on each unimplemented component for specs → verify: step output matches expected outcome

## Working with Figma URLs

Figma URLs contain the file key and optionally a node ID:
```
https://figma.com/design/FILE_KEY/Name?node-id=NODE_ID
```
Extract `FILE_KEY` and `NODE_ID` from the URL to pass to the tools.

## Tips

- Always start with `figma_get_file` to orient yourself before diving into specific nodes
- Node IDs in URLs use `-` but the API expects `:` (e.g., `1-2` in URL → `1:2` in API)
- Export icons as SVG, photos as PNG/WebP
- Use style names directly as CSS variable names for consistency

## When NOT to use

- No Figma file involved — use `ui-to-code` for screenshots or `frontend-design`
- Pure design system creation without source Figma — use `design-system` or `theme-factory`
- Image/asset generation (not extraction) — use `nano-banana-pro`
- Design reviews without code output — use `design-taste-frontend`
- Bulk Figma admin (project/team management) — out of scope

## Red Flags

| Thought | Reality |
|---------|---------|
| "Use the URL node ID as-is" | API expects `:` not `-`; conversion required |
| "Skip `figma_get_file`, jump to node" | Without context you misinterpret the node; orient first |
| "Export everything as PNG" | Icons must be SVG; PNG bloats and pixelates |
| "Eyeball the color values from screenshot" | Pull exact values via `figma_get_styles` — design tokens, not approximations |

## Output Contract

Done when:
- Correct `FILE_KEY` extracted from URL
- `figma_get_file` ran for orientation before drilling into nodes
- Styles extracted via `figma_get_styles` mapped to CSS variables / theme object
- Icons exported as SVG, photos as PNG/WebP
- Node IDs converted from URL `-` to API `:` form
- Implementation matches extracted spec (colors, typography, spacing)

## Examples

### Example 1 — Implement a design screen
- Input: User pastes Figma URL for a settings page
- Action: Extract FILE_KEY + NODE_ID (convert `-`→`:`), `figma_get_file` to orient, `figma_get_node` for the settings frame, `figma_get_styles` for tokens, `figma_get_images` for icons as SVG; write the component
- Output: React component + tokens.css + `/icons/*.svg`, all values pulled from Figma, not invented

### Example 2 — Audit design system gaps
- Input: "What components in Figma are not yet built?"
- Action: `figma_get_components` to list all, cross-reference against existing component directory, report missing ones with node IDs
- Output: Gap report with priority-ordered list of unimplemented components, ready to scope
