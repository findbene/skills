---
name: "design-bugatti"
description: "Apply Bugatti design system aesthetic. An austere luxury-automotive interface that uses near-pure black canvas, white uppercase letterspaced display, and full-bleed automotive photography as the only voltage. Use when scaffolding UI components, generating styled pages, or matching Bugatti's visual language (colors, typography, spacing, components). Loads bundled DESIGN.md with full token spec, type scale, component patterns, and brand rationale."
---

# Bugatti Design System

Reusable design system skill scaffolded from `DESIGN.md`. Source: VoltAgent/awesome-claude-design + getdesign.md.

## When to use

User asks to build, scaffold, or style UI matching Bugatti's visual language. Also: "make it look like Bugatti", "use Bugatti aesthetic", "Bugatti-style components".

## How to apply

1. Read `DESIGN.md` (bundled in this skill dir) — contains full token spec.
2. Emit CSS variables from `colors:` section verbatim.
3. Build type scale from `typography:` block.
4. Apply component patterns from `components:` section.
5. Respect `dos_donts:` rules — these are guardrails.
6. Use `agent_prompt_guide:` reusable prompts for new screens.

## Files

- `DESIGN.md` — full design system spec (Bugatti)

## Brand summary

An austere luxury-automotive interface that uses near-pure black canvas, white uppercase letterspaced display, and full-bleed automotive photography as the only voltage. The system runs three custom Bugatti typefaces — Bugatti Display, Bugatti Text Regular, and Bugatti Monospace — and combines them at modest weights with wide tracking to feel European-engineered, hyper-minimal, and quietly expensive. There is no accent color, no decorative element, no chrome — only photography, typography, and the brand wordmark.
