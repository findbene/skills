---
name: "design-clay"
description: "Apply Clay design system aesthetic. A vibrant claymation-meets-data interface for Clay. Use when scaffolding UI components, generating styled pages, or matching Clay's visual language (colors, typography, spacing, components). Loads bundled DESIGN.md with full token spec, type scale, component patterns, and brand rationale."
---

# Clay Design System

Reusable design system skill scaffolded from `DESIGN.md`. Source: VoltAgent/awesome-claude-design + getdesign.md.

## When to use

User asks to build, scaffold, or style UI matching Clay's visual language. Also: "make it look like Clay", "use Clay aesthetic", "Clay-style components".

## How to apply

1. Read `DESIGN.md` (bundled in this skill dir) — contains full token spec.
2. Emit CSS variables from `colors:` section verbatim.
3. Build type scale from `typography:` block.
4. Apply component patterns from `components:` section.
5. Respect `dos_donts:` rules — these are guardrails.
6. Use `agent_prompt_guide:` reusable prompts for new screens.

## Files

- `DESIGN.md` — full design system spec (Clay)

## Brand summary

A vibrant claymation-meets-data interface for Clay.com (GTM data-orchestration platform). Anchors on white canvas with dark-navy primary CTAs, custom rounded display type, and saturated single-color feature cards — hot pink, deep teal, lavender, peach, ochre — that punctuate long-scroll explainer pages. Brand voltage comes from 3D-rendered claymation illustrations (mountains, characters, mascots) used as full-bleed hero artifacts and the bright multi-color card surfaces showing product UI fragments.
