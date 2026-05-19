---
name: redesign-existing-projects
description: "Upgrades existing websites and apps to premium quality. Triggers: 'use redesign-existing-projects', 'redesign existing projects', 'redesign-existing-projects task'."
allowed-tools: Glob, Grep, Read
---

# Redesign Skill

## When NOT to use

- Greenfield design from scratch — use `frontend-design`, `impeccable`, or `design-system`
- Accessibility-only audit — use `a11y-audit`
- Performance-only audit — use `audit` or `performance-profiler`
- Logo / brand identity refresh — use `brand` or `brand-guidelines`
- Major framework migration (e.g., Pages Router → App Router) — use `migration-architect`

## Red Flags

| Rationalization | Reality |
|---|---|
| "Rewrite the whole thing in [new stack]" | Skill explicitly mandates targeted upgrades on existing stack — not rewrites |
| "Inter is fine, it is popular" | Skill flags Inter as a generic-default pattern; switch to characterful fonts (Geist, Outfit, Satoshi) |
| "Skip the audit, the user already knows what is wrong" | The audit surfaces patterns the user has stopped seeing; always run it |
| "Apply Tailwind v4 patterns to a v3 project" | Tailwind config schema differs; check version first to avoid breaking the build |

## Output Contract

Finished output must contain:
- Project audit identifying current framework, styling method, and tech version
- List of generic AI patterns found, ranked by visual impact
- Specific targeted fixes (not a full rewrite) with file paths and diffs
- Typography upgrade (font choice, scale, line-height, letter-spacing)
- Color system refinement using design tokens
- Spacing/layout adjustments
- Empty/loading/error states audited and added where missing
- All changes additive and reversible

## Examples

**Example 1 — Audit and uplift a Next.js dashboard**
- Input: "Make this dashboard look more premium"
- Action: Scan codebase → confirm Next.js + Tailwind v3 → audit typography (Inter), colors (generic gray scale), components (default shadcn defaults), motion (none) → produce targeted fixes
- Output: 8 file edits replacing Inter with Geist, adding tabular nums for data, refining gray scale, adding hover/focus transitions, adding empty states; total diff <500 lines

**Example 2 — Vanilla CSS portfolio site**
- Input: "Upgrade my static portfolio site"
- Action: Scan HTML/CSS → no framework → audit (browser default fonts, no design tokens, 1 color, no responsive) → recommend CSS custom properties + font swap + responsive grid
- Output: Updated `style.css` with tokens, font import (Geist + Cabinet Grotesk), responsive grid, hover micro-interactions, no JS added

## References

Extended sections moved to `references/details.md`.
