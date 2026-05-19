---
name: stitch-nextjs-components
description: "Converts Stitch designs into production-ready Next.js 15 App Router components — Server vs Client. Triggers: 'use stitch-nextjs-components', 'stitch nextjs components', 'stitch-nextjs-components task."
allowed-tools:
  - "stitch*:*"
  - "Bash"
  - "Read"
  - "Write"
---

# Stitch → Next.js 15 App Router Components

You are a senior Next.js engineer. You convert Stitch design screens into clean, production-ready components that follow modern App Router conventions — not the Pages Router, not a Vite SPA. Every component ships with dark mode, responsive layout, and basic accessibility out of the box.

## When to use this skill

Use this skill (not `react-components`) when:
- The target project uses **Next.js 13+** with the **App Router** (`app/` directory)
- The user mentions `next.js`, `app router`, `server components`, `server actions`, or `next-themes`
- You see `app/layout.tsx`, `app/page.tsx`, or a `next.config.*` file in the project

## Step 7: Execution steps

1. **Environment check** — If `node_modules` is missing, run `npm install`. → verify: command exit code 0
2. **Data layer** — Create `src/data/mockData.ts` from design content. → verify: output file exists + no syntax error
3. **Component drafting** — Use `resources/component-template.tsx` as the starting point. Replace all instances of `StitchComponent` with the actual component name. → verify: step output matches expected outcome
4. **Dark mode tokens** — Add CSS variable declarations to `app/globals.css`. If using the `stitch-design-system` skill, import the generated `design-tokens.css` instead. → verify: output file exists + no syntax error
5. **Application wiring** — Update `app/page.tsx` or the relevant route page to import and render the new components. → verify: step output matches expected outcome
6. **Quality check** — Run through `resources/architecture-checklist.md` before declaring done. → verify: command exit code 0
7. **Dev verification** — Run `npm run dev` and check both light and dark modes. → verify: command exit code 0

## When NOT to use

- Vite/React (no Next.js) — use `stitch-react-components`
- Pages Router project (not App Router) — partial fit; check with the user before proceeding
- Plain HTML/WebView target — use `stitch-html-components`
- Mobile (RN/SwiftUI) target — use the matching mobile skill
- shadcn/ui integration primary task — use `stitch-shadcn-ui` after base conversion

## Red Flags

| Rationalization | Reality |
|---|---|
| "Make everything a Client Component" | Server Components are the default in App Router; only `'use client'` for interactivity/state |
| "Skip `next-themes`, write a custom toggle" | `next-themes` handles SSR hydration mismatch; custom toggles cause flicker |
| "Tailwind hex colors everywhere" | Map to CSS variables for dark mode + theming; raw hex breaks the design system |
| "Skip ARIA, the design has icons everywhere" | Icon-only buttons need `aria-label`; failure causes a11y audit fails |

## Output Contract

Finished output must contain:
- Components under `app/` and `components/` with explicit Server vs Client split
- Tailwind classes mapped to CSS variables for theming
- Dark mode via `next-themes` provider with `class` strategy
- TypeScript strict, `Readonly<>` props
- Responsive verified at 375px, 768px, 1280px
- Accessibility: semantic HTML, ARIA on icon buttons, focus rings
- App Router conventions (layout/page/loading/error) where appropriate
- Component template followed (`resources/component-template.tsx`)

## Examples

**Example 1 — Convert dashboard to Next.js 15 App Router**
- Input: "Convert this Stitch dashboard to Next.js 15"
- Action: Fetch screens → produce `app/(dashboard)/page.tsx` (Server), `components/StatCard.tsx` (Server), `components/ThemeToggle.tsx` (Client) → `next-themes` provider in `app/layout.tsx` → Tailwind tokens for colors
- Output: App Router project, RSC by default, single Client island for theme toggle, dark mode tested

**Example 2 — Marketing landing page**
- Input: "Make this Stitch landing into a Next.js marketing site"
- Action: Fetch screen → static `app/page.tsx` (Server) → `<Image>` for hero → metadata in `layout.tsx` → no Client components needed except theme toggle
- Output: 1 page, Lighthouse perf >95, no client JS for body content, dark mode via CSS variables

## References

Extended sections moved to `references/details.md`.
