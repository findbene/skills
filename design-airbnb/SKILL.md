---
name: "design-airbnb"
description: "Apply Airbnb design system aesthetic. A warm, generous consumer marketplace anchored on a clean white canvas and Airbnb Rausch (#f. Triggers: 'use design-airbnb', 'design airbnb', 'design-airbnb task."
allowed-tools: Glob, Grep, Read
---

# Airbnb Design System

Reusable design system skill scaffolded from `DESIGN.md`. Source: VoltAgent/awesome-claude-design + getdesign.md.

## When to use

User asks to build, scaffold, or style UI matching Airbnb's visual language. Also: "make it look like Airbnb", "use Airbnb aesthetic", "Airbnb-style components".

## How to apply

1. Read `DESIGN.md` (bundled in this skill dir) — contains full token spec. → verify: file exists and `colors:` / `typography:` blocks present
2. Emit CSS variables from `colors:` section verbatim. → verify: hex values match DESIGN.md spec
3. Build type scale from `typography:` block. → verify: font-family + weight scale defined
4. Apply component patterns from `components:` section. → verify: each component uses spec radii/shadows
5. Respect `dos_donts:` rules — these are guardrails. → verify: no `dos_donts:` rule violated
6. Use `agent_prompt_guide:` reusable prompts for new screens. → verify: prompts referenced when generating UI

## Files

- `DESIGN.md` — full design system spec (Airbnb)

## When NOT to use

- User wants brand-agnostic design → use generic `frontend-design` skill instead
- Different brand requested → use the matching `design-<brand>` skill
- User needs plain Tailwind/CSS without brand language → skip design-* skills
- Building production replica of Airbnb itself (trademark / IP risk)

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
- Input: "Build a marketing hero in Airbnb style"
- Action: read `DESIGN.md`, apply brand background/typography/CTA radius + brand voltage color, follow whitespace guidance from spec
- Output: hero section using exact brand tokens, ready to drop into Next.js or HTML

### Example 2 — Card grid
- Input: "Card grid that looks like Airbnb"
- Action: pull card radius from `components:`, shadow from `shadows:`, gap from `spacing:`, hover state from `dos_donts:` guidance
- Output: grid with brand-correct card chrome, no default Tailwind leakage

## Brand summary

A warm, generous consumer marketplace anchored on a clean white canvas and Airbnb Rausch (#ff385c), the single brand voltage that carries every primary CTA, search-button orb, and rating dot. Type runs Airbnb Cereal VF at modest weights — display sits at 22–28px in weight 500/600 rather than the heavy 700+ that fintech and enterprise systems use; the brand trusts photography and generous whitespace over typographic muscle. Three product entries (Homes, Experiences, Services) sit in the top nav with hand-illustrated 32-icon glyphs and "NEW" badges, signaling a marketplace expansion rather than a feature dump. Pill-shaped search bars (`{rounded.full}`), softly rounded property cards (`{rounded.lg}` ~14px), and 32px button radii read as friendly and human — there is no hard corner anywhere except the body grid.
