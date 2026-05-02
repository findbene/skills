---
name: figma-tools
description: "Figma design data extraction via MCP for accessing layouts, components, styles, colors, and design specifications from Figma files. Use this skill any time Figma design files need to be accessed, design tokens need to be extracted from Figma, component specs need to be read, or Figma data needs to be converted to code. Trigger immediately on: \"Figma\", \"Figma file\", \"extract from Figma\", \"Figma design\", \"Figma component\", \"Figma token\", \"Figma MCP\", \"Figma layout\", \"Figma API\", \"design spec from Figma\", \"Figma styles\", \"implement this Figma design\", \"Figma frame\". If someone shares a Figma link or says \"get this from Figma\" this skill MUST trigger."
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
1. `figma_get_file` to understand overall file structure and find the target frame
2. `figma_get_node(node_id="...")` for the specific frame to implement
3. `figma_get_styles` to extract colors, fonts, spacing as CSS variables
4. `figma_get_images` to export icons and images as SVG/PNG
5. Write the component code using extracted values

### Extract Design Tokens
1. `figma_get_styles` to pull all named styles
2. Map to CSS custom properties or a theme object:
   - Colors → `--color-primary`, `--color-secondary`
   - Typography → font families, sizes, weights, line heights
   - Effects → shadows, blurs

### Audit Component Library
1. `figma_get_components` to list all components
2. Compare against implemented components to find gaps
3. `figma_get_node` on each unimplemented component for specs

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
