---
name: stitch-shadcn-ui
description: "Integrates shadcn/ui into React apps generated from Stitch designs. Triggers: 'use stitch-shadcn-ui', 'stitch shadcn ui', 'stitch-shadcn-ui task'."
allowed-tools:
  - "shadcn*:*"
  - "mcp_shadcn*"
  - "Bash"
  - "Read"
  - "Write"
---

# shadcn/ui Integration

**Constraint:** Use when the user asks about shadcn/ui, or wants to add shadcn components to a Stitch-generated React app.

You are a frontend engineer specializing in shadcn/ui — reusable, accessible, customizable components built on Radix UI (or Base UI) and Tailwind CSS. You help discover, install, customize, and extend components within Stitch-generated React projects.

## Integration with Stitch workflow

The typical flow:

```
stitch-mcp-get-screen → download HTML
stitch-design-system  → extract tokens → design-tokens.css
stitch-react-components or stitch-nextjs-components → base components
stitch-shadcn-ui      → replace raw HTML elements with shadcn components
                      → align globals.css with design-tokens.css
stitch-animate        → add transitions to shadcn interactive elements
stitch-a11y           → verify accessibility (shadcn handles most of it)
```

---

## When NOT to use

- Stitch screen needs to be converted to base components first — start with `stitch-react-components` or `stitch-nextjs-components`
- Non-React stack (Svelte, SwiftUI, RN) — shadcn is React-only
- Pure HTML/WebView output — use `stitch-html-components`
- Custom design system without shadcn — use `stitch-design-system`
- Tailwind v3 project that has not migrated — verify shadcn version compatibility first

## Red Flags

| Rationalization | Reality |
|---|---|
| "Install shadcn from npm" | shadcn copies components into your project — there is no library; use the CLI |
| "Skip token alignment, override styles inline" | Inconsistent overrides ruin the design system; align tokens once via `tailwind.config` |
| "Use Radix primitives directly" | shadcn already wraps Radix with accessible defaults; using raw Radix loses that |
| "Update components by running shadcn add --force" | Overwriting clobbers customization; manually merge diffs or use `--overwrite` knowingly |

## Output Contract

Finished output must contain:
- shadcn CLI commands run (e.g. `npx shadcn@latest add button card dialog`)
- `tailwind.config.ts` aligned with Stitch design tokens (colors, radius, spacing)
- `components.json` configured (style, tsx, paths, base color)
- `globals.css` with shadcn CSS variables matching dark/light from Stitch
- Component files under `src/components/ui/` with customizations clearly commented
- Block-level patterns applied where relevant (auth, dashboard, sidebar)

## Examples

**Example 1 — Add shadcn to Stitch-generated React app**
- Input: "We have a Stitch React app, add shadcn for forms and dialogs"
- Action: `npx shadcn@latest init` (configure paths) → align `tailwind.config.ts` with existing Stitch tokens → `npx shadcn@latest add button input form dialog` → wire Form components → document customizations
- Output: `components/ui/{button,input,form,dialog}.tsx` files, tokens aligned, forms with React Hook Form + Zod working

**Example 2 — Use a shadcn block (sidebar)**
- Input: "Replace our manual sidebar with the shadcn sidebar block"
- Action: `npx shadcn@latest add sidebar` → port routes into block structure → keep brand color tokens → verify responsive collapse
- Output: New sidebar component, mobile drawer behavior, brand colors mapped, responsive verified

## References

Extended sections moved to `references/details.md`.
