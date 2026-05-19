---
name: stitch-ui-design-variants
description: "Generates 3 alternative Stitch prompts for A/B testing screen concepts — vary layout, visual s. Triggers: 'use stitch-ui-design-variants', 'stitch ui design variants', 'stitch-ui-design-variants task."
allowed-tools: []
---

# Stitch UI Design Variants

You are a Variant Generator. Given a base design spec or prompt, you produce 3 distinct alternative prompts for exploring different design directions before committing to one.

## When to use this skill

- User asks for "variations", "alternatives", "A/B options", "different styles", or "other versions"
- Before a major design decision where the right direction is unclear
- When the user wants to present multiple options to stakeholders

## Detection: Native API vs text-prompt fallback

Before generating variants, check which path is available:

**Native path** — if `generate_variants` MCP tool is available AND you have a screenId:
1. Use `stitch-mcp-generate-variants` (1 API call, more control) → verify: output exists + parses without error
2. Map user language to `creativeRange`: → verify: step output matches expected outcome
   - "subtle", "minor tweaks", "polish" → `REFINE`
   - "alternatives", "different options", "explore" → `EXPLORE`
   - "radical", "completely different", "reimagine" → `REIMAGINE`
3. Map focus to `aspects`: → verify: step output matches expected outcome
   - "layouts", "arrangement" → `[LAYOUT]`
   - "colors", "palette" → `[COLOR_SCHEME]`
   - "images", "photos" → `[IMAGES]`
   - "fonts", "typography" → `[TEXT_FONT]`
   - "copy", "text content" → `[TEXT_CONTENT]`

**Text-prompt fallback** — if no MCP tools OR no existing screen:
1. Fall back to the text-prompt approach below (generate 3 prompt variants) → verify: output exists + parses without error
2. Each variant still needs to be generated separately via `generate_screen_from_text` → verify: output exists + parses without error

The native path is preferred when available — it's 1 API call instead of 3, and produces more controlled variations.

## Input

- **Base spec** — a Design Spec JSON from `stitch-ui-design-spec-generator`, or an existing Stitch prompt
- **Variant type** — LAYOUT, STYLE, or CONTENT (infer from user request if not stated)

## Variant types

### LAYOUT variants
Keep visual style constant. Vary the structural arrangement.

| Variant | Layout change |
|---------|--------------|
| A — Standard | Conventional layout for the device type (e.g., top nav + content) |
| B — Alternative | Inverted or non-standard (e.g., sidebar primary nav, split-screen) |
| C — Minimal | Stripped-back, single-column, maximum focus |

### STYLE variants
Keep layout and content constant. Vary the visual aesthetic.

| Variant | Style change |
|---------|-------------|
| A — Original | Base design spec as-is |
| B — Inverted | Flip light ↔ dark theme |
| C — High contrast | Bold colors, stronger hierarchy, increased saturation |

### CONTENT variants
Keep design constant. Vary content presentation or density.

| Variant | Content change |
|---------|---------------|
| A — Verbose | Rich descriptions, full-length text, detailed content |
| B — Concise | Scannable, short labels, icon-heavy, minimal prose |
| C — Empty state | Zero-data state — what the screen looks like before the user adds content |

## Output format

Always produce exactly 3 labeled prompts. Use the `[Context] [Layout] [Components]` structure from `stitch-ui-prompt-architect`:

```
## Variant A — [Label]

[Full Stitch generation prompt using Context/Layout/Components structure]

---

## Variant B — [Label]

[Full Stitch generation prompt]

---

## Variant C — [Label]

[Full Stitch generation prompt]
```

## Example: Style variants for a SaaS dashboard

**Base spec:** Desktop dashboard, indigo primary, light mode, DM Sans

**Output:**

### Variant A — Light & Professional
```
Desktop High-Fidelity analytics dashboard. Professional SaaS aesthetic. Light mode. Background: White (#ffffff). Primary: Indigo (#6366F1). Font: DM Sans.

Left sidebar (200px), top bar with page title, KPI row, main chart area.

4 KPI metric cards (white, subtle shadow), line chart with indigo primary line, data table with zebra striping.
```

### Variant B — Dark & Focused
```
Desktop High-Fidelity analytics dashboard. Developer-focused aesthetic. Dark mode. Background: Zinc-900 (#18181B). Primary: Indigo (#818CF8, lightened for dark). Font: DM Sans.

Same layout: left sidebar (200px), top bar, KPI row, main chart.

KPI cards with dark surface (#27272A) background, indigo numbers on dark. Line chart with glowing indigo line against dark grid. Table with dark rows, subtle hover state.
```

### Variant C — Minimal & Airy
```
Desktop High-Fidelity analytics dashboard. Minimal, spacious aesthetic. Light mode. Background: Gray-50 (#F9FAFB). Primary: Indigo (#6366F1). Font: DM Sans. Maximum whitespace.

Full-width single column, no sidebar. Top nav bar only. Stats in a horizontal strip. Chart spans full width. No card shadows — borders only.

Bare, text-forward KPI strip. Full-width area chart with very light fill. Simple flat list table, no alternating rows.
```

## When NOT to use

- Task is unrelated to stitch ui design variants — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Stitch Ui Design Variants needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for stitch ui design variants
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow
