---
name: stitch-react-components
description: "Converts Stitch designs into modular Vite + React components — TypeScript, theme-mapped Tailwind, da. Triggers: 'use stitch-react-components', 'stitch react components', 'stitch-react-components task."
allowed-tools:
  - "stitch*:*"
  - "Bash"
  - "Read"
  - "Write"
---

# Stitch → Vite / React Components

**Constraint:** Only use this skill when the user explicitly mentions "Stitch" and React (Vite, CRA, or just "React app" without Next.js).

You are a frontend engineer converting Stitch mobile/desktop designs into clean, modular React components using Vite + TypeScript. This skill targets plain React apps — **not** Next.js App Router. For Next.js, use `stitch-nextjs-components` instead.

## When to use this skill vs. Next.js

| Scenario | Use |
|----------|-----|
| User says "React app", "Vite", "CRA" | `stitch-react-components` |
| User says "Next.js", "App Router", "SSR" | `stitch-nextjs-components` |
| User wants shadcn/ui components added after | `stitch-react-components` → then `stitch-shadcn-ui` |
| User wants server-side rendering or file-based routing | `stitch-nextjs-components` |

## Prerequisites

- Stitch MCP Server configured (or use downloaded HTML directly)
- Node.js + npm/pnpm
- Vite + React project initialized: `npm create vite@latest my-app -- --template react-ts`

## Step 1: Retrieve the design

1. Run `list_tools` → find Stitch MCP prefix
2. Call `[prefix]:get_screen` with numeric `projectId` and `screenId` → verify: diff matches intended change
3. Download HTML: `bash scripts/fetch-stitch.sh "[htmlCode.downloadUrl]" "temp/source.html"` → verify: file content matches expected shape
4. Check `screenshot.downloadUrl` — verify layout matches expectations

## Step 2: Project structure

```
src/
├── components/           ← One file per component
│   └── [Name].tsx
├── data/
│   └── mockData.ts       ← Static content (never in components)
├── theme/
│   ├── tokens.ts         ← Design token constants
│   └── useTheme.ts       ← Dark mode hook
├── types/
│   └── index.ts          ← Shared TypeScript types
├── App.tsx               ← Root component
└── main.tsx              ← Entry point
```

## Step 3: Extract design tokens

From the Stitch HTML `<head>`, find the `tailwind.config` or CSS variable definitions.

```ts
// src/theme/tokens.ts — extract hex values from Stitch HTML
export const lightTokens = {
  background: '#FFFFFF',
  surface:    '#F4F4F5',
  primary:    '#6366F1',
  primaryFg:  '#FFFFFF',
  text:       '#09090B',
  textMuted:  '#71717A',
  border:     '#E4E4E7',
} as const

export const darkTokens = {
  background: '#09090B',
  surface:    '#18181B',
  primary:    '#818CF8',
  primaryFg:  '#09090B',
  text:       '#FAFAFA',
  textMuted:  '#A1A1AA',
  border:     '#27272A',
} as const

export type ThemeTokens = typeof lightTokens
```

```ts
// src/theme/useTheme.ts
import { useEffect, useState } from 'react'
import { lightTokens, darkTokens, type ThemeTokens } from './tokens'

/**
 * Returns current theme tokens based on system color scheme.
 * Listens for system-level dark/light mode changes.
 */
export function useTheme(): ThemeTokens {
  const [isDark, setIsDark] = useState(
    () => window.matchMedia('(prefers-color-scheme: dark)').matches
  )

  useEffect(() => {
    const mq = window.matchMedia('(prefers-color-scheme: dark)')
    const handler = (e: MediaQueryListEvent) => setIsDark(e.matches)
    mq.addEventListener('change', handler)
    return () => mq.removeEventListener('change', handler)
  }, [])

  return isDark ? darkTokens : lightTokens
}
```

## Step 4: Component conversion rules

### Layout mapping

| HTML/CSS | → React / Tailwind |
|---|---|
| `display:flex; flex-direction:column` | `<div className="flex flex-col gap-4">` |
| `display:flex; flex-direction:row` | `<div className="flex items-center gap-2">` |
| `justify-content:space-between` | `<div className="flex justify-between">` |
| `display:grid; grid-template-columns:1fr 1fr` | `<div className="grid grid-cols-2 gap-4">` |
| `overflow-y:scroll` | `<div className="overflow-y-auto">` |
| Long list | `items.map(item => <Card key={item.id} {...item} />)` |
| `<img>` | `<img src="..." alt="..." className="object-cover">` |

### Tailwind class mapping

Use the Stitch HTML classes directly in JSX where they don't reference Stitch-specific tokens. Map Stitch tokens to CSS variables:

```tsx
// Stitch HTML: bg-primary → CSS variable → Tailwind arbitrary value
// OR: use inline style with token value

// Option A — Tailwind arbitrary value (if custom tokens in tailwind.config)
<div className="bg-[--color-primary] text-[--color-primaryFg]">

// Option B — inline style with useTheme()
const theme = useTheme()
<div style={{ backgroundColor: theme.primary, color: theme.primaryFg }}>
```

### Component template

```tsx
// src/components/StitchComponent.tsx

/**
 * Props for StitchComponent — all data via props, never fetched inside.
 */
interface StitchComponentProps {
  /** Primary heading text */
  title: string
  /** Supporting description — optional */
  description?: string
  /** Primary action callback */
  onAction?: () => void
}

/**
 * StitchComponent — [describe purpose in one sentence]
 */
export function StitchComponent({
  title,
  description,
  onAction,
}: Readonly<StitchComponentProps>) {
  const theme = useTheme()

  return (
    <div
      className="rounded-xl border p-4 gap-2 flex flex-col"
      style={{
        backgroundColor: theme.surface,
        borderColor: theme.border,
      }}
    >
      <h3 className="text-base font-semibold" style={{ color: theme.text }}>
        {title}
      </h3>

      {description ? (
        <p className="text-sm" style={{ color: theme.textMuted }}>
          {description}
        </p>
      ) : null}

      {onAction ? (
        <button
          onClick={onAction}
          className="rounded-lg px-4 py-2 text-sm font-medium transition-opacity hover:opacity-90"
          style={{ backgroundColor: theme.primary, color: theme.primaryFg }}
          type="button"
        >
          Action
        </button>
      ) : null}
    </div>
  )
}
```

## Step 5: Architectural rules

- **One component per file** — no single-file spaghetti
- **Static data in `src/data/mockData.ts`** — never hardcoded in JSX
- **Shared types in `src/types/index.ts`**
- **Every component has `Readonly<ComponentNameProps>` interface**
- **No hardcoded hex colors** — use `useTheme()` or CSS variables
- **No `any` types**

## Step 6: Integration with shadcn/ui

After converting the Stitch design to base React components, you can layer in shadcn/ui:

```bash
npx shadcn@latest init    # Set up shadcn in your Vite project
npx shadcn@latest add button card input dialog
```

Then use `stitch-shadcn-ui` skill to replace raw HTML elements with shadcn components while preserving the Stitch design tokens.

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Tailwind classes not applying | Check `tailwind.config.js` includes `./src/**/*.{ts,tsx}` in content |
| Dark mode not toggling | Verify `useTheme()` is called at component level, not hoisted |
| Images not showing | Add explicit `width` and `height` or use `className="w-full h-auto"` |
| Type error on props | Ensure `Readonly<>` wrapper and all required props are provided |

## When NOT to use

- Target uses Next.js App Router — use `stitch-nextjs-components`
- Target is SwiftUI / React Native / Svelte / plain HTML — use the matching skill
- No build step desired or WebView target — use `stitch-html-components`
- shadcn/ui integration is the primary task — use `stitch-shadcn-ui`
- Animation-heavy walkthrough video output — use `stitch-remotion`

## Red Flags

| Rationalization | Reality |
|---|---|
| "Inline Stitch HTML, wrap in a component" | Output must be idiomatic React with proper props, not divs with inline styles |
| "Skip TypeScript, JSX is fine" | Skill mandates TS strict — type safety catches the most common Stitch-to-code defects |
| "Dark mode with a manual className toggle" | Use CSS variables + theme provider; the manual toggle pattern is fragile across pages |
| "Forget responsive, design was desktop" | Stitch designs must be made responsive with Tailwind breakpoints from `sm:` upward |

## Output Contract

Finished output must contain:
- TypeScript React components under `src/components/` with `Props` interface and `Readonly<>` wrap
- Tailwind classes mapped to theme tokens (CSS variables), not hard-coded hex
- Dark mode via CSS variables driven by class or data-attribute on `<html>`
- Responsive layout — verified at 375px, 768px, 1280px
- No inline styles except dynamic computed values
- Index/barrel exports for the component module
- Component template followed (`resources/component-template.tsx`)

## Examples

**Example 1 — Convert Stitch dashboard screen to Vite/React**
- Input: "Convert this Stitch dashboard to a React app, no Next.js"
- Action: `list_tools` → fetch screen → `bash scripts/fetch-stitch.sh` → produce `Dashboard.tsx`, `StatCard.tsx`, `Chart.tsx` with TS interfaces → Tailwind via CSS variables → theme provider for dark mode
- Output: 4 TSX files, `tokens.css` with light+dark, `ThemeProvider.tsx`, components render at 375px and 1280px

**Example 2 — Single landing-page component**
- Input: "Just give me the Hero section from this Stitch screen as a React component"
- Action: Fetch only the hero block → produce `Hero.tsx` with props for headline/subhead/cta → Tailwind classes → mobile-first
- Output: `Hero.tsx`, props interface, mobile + desktop verified, dark mode supported

## References

- `resources/component-template.tsx` — Boilerplate component
- `resources/architecture-checklist.md` — Pre-ship checklist
- `references/tailwind-to-react.md` — Token + class mapping guide (Stitch HTML → React/Tailwind)
- `scripts/fetch-stitch.sh` — Reliable GCS HTML downloader
- `stitch-shadcn-ui` — Add shadcn/ui components after base conversion
- `docs/tailwind-reference.md` — Tailwind utility class lookup
