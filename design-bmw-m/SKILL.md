---
name: "design-bmw-m"
description: "Apply BMW M design system aesthetic. A motorsport-engineering interface anchored on a near-black canvas with white BMW Type Next Lati. Triggers: 'use design-bmw-m', 'design bmw m', 'design-bmw-m task."
allowed-tools: Glob, Grep, Read
---

# BMW M Design System

Reusable design system skill scaffolded from `DESIGN.md`. Source: VoltAgent/awesome-claude-design + getdesign.md.

## When to use

User asks to build, scaffold, or style UI matching BMW M's visual language. Also: "make it look like BMW M", "use BMW M aesthetic", "BMW M-style components".

## How to apply

1. Read `DESIGN.md` (bundled in this skill dir) — contains full token spec. → verify: file exists and `colors:` / `typography:` blocks present
2. Emit CSS variables from `colors:` section verbatim. → verify: hex values match DESIGN.md spec
3. Build type scale from `typography:` block. → verify: font-family + weight scale defined
4. Apply component patterns from `components:` section. → verify: each component uses spec radii/shadows
5. Respect `dos_donts:` rules — these are guardrails. → verify: no `dos_donts:` rule violated
6. Use `agent_prompt_guide:` reusable prompts for new screens. → verify: prompts referenced when generating UI

## Files

- `DESIGN.md` — full design system spec (BMW M)

## When NOT to use

- User wants brand-agnostic design → use generic `frontend-design` skill instead
- Different brand requested → use the matching `design-<brand>` skill
- User needs plain Tailwind/CSS without brand language → skip design-* skills
- Building production replica of Bmw M itself (trademark / IP risk)

## Red Flags

| Thought | Reality |
|---------|---------|
| "I'll pick close hex values instead of exact" | Brand voltage must match exact — close hex breaks brand recognition |
| "Tailwind defaults are close enough" | Brand radii/shadows/spacing are non-default; defaults break the look |
| "Skip DESIGN.md, just match vibes" | Tokens encode the rules; vibes drift |
| "Apply brand color everywhere" | Brand uses CTA/accent voltage sparingly — uniform application kills hierarchy |

## Output Contract

Done-state:
- All color values traceable to `DESIGN.md` `colors:` block
- Typography scale uses brand font + brand weight ladder from `typography:`
- Component radii, shadows, spacing match `components:` tokens
- No `dos_donts:` rule violated in generated output
- CSS variables exposed for downstream re-theming

## Examples

### Example 1 — Marketing hero
- Input: "Build a marketing hero in Bmw M style"
- Action: read `DESIGN.md`, apply brand background/typography/CTA radius + brand voltage color, follow whitespace guidance from spec
- Output: hero section using exact brand tokens, ready to drop into Next.js or HTML

### Example 2 — Card grid
- Input: "Card grid that looks like Bmw M"
- Action: pull card radius from `components:`, shadow from `shadows:`, gap from `spacing:`, hover state from `dos_donts:` guidance
- Output: grid with brand-correct card chrome, no default Tailwind leakage

## Brand summary

A motorsport-engineering interface anchored on a near-black canvas with white BMW Type Next Latin display headlines in confident UPPERCASE. The brand carries no decorative voltage — its energy comes from full-bleed automotive photography (cars on tracks, driver-cockpit shots, carbon-fiber detail) and the iconic M tricolor stripe (light blue → dark blue → red) used sparingly as a brand signature on logos, dividers, and motorsport chrome. Type stays light to medium weight to feel European-engineered, never American-bombastic.
