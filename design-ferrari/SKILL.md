---
name: "design-ferrari"
description: "Apply Ferrari design system aesthetic. A luxury-automotive brand whose marketing surfaces read as cinematic editorial. Use when scaffolding UI components, generating styled pages, or matching Ferrari's visual language (colors, typography, spacing, components). Loads bundled DESIGN.md with full token spec, type scale, component patterns, and brand rationale."
---

# Ferrari Design System

Reusable design system skill scaffolded from `DESIGN.md`. Source: VoltAgent/awesome-claude-design + getdesign.md.

## When to use

User asks to build, scaffold, or style UI matching Ferrari's visual language. Also: "make it look like Ferrari", "use Ferrari aesthetic", "Ferrari-style components".

## How to apply

1. Read `DESIGN.md` (bundled in this skill dir) — contains full token spec.
2. Emit CSS variables from `colors:` section verbatim.
3. Build type scale from `typography:` block.
4. Apply component patterns from `components:` section.
5. Respect `dos_donts:` rules — these are guardrails.
6. Use `agent_prompt_guide:` reusable prompts for new screens.

## Files

- `DESIGN.md` — full design system spec (Ferrari)

## Brand summary

A luxury-automotive brand whose marketing surfaces read as cinematic editorial. The base canvas is **near-black** (`#181818`) holding pure white display type; white-canvas bands appear only inside specific editorial contexts (preowned listings, pricing tables). The single brand voltage is **Rosso Corsa** (`#da291c`) — the iconic Ferrari racing red — used scarcely on primary CTAs, the Cavallino mark, and Formula 1 race-position highlights. Type runs **FerrariSans** at modest weights (display 500, body 400) — never bombastic. Spacing follows an explicit 8px token ladder (`xxxs` 4px through `super` 128px); generous editorial pacing throughout. The brand's strongest visual signature is the **full-bleed cinematic hero photograph** that fills the viewport top with car photography, model details, or trackside livery — followed by a tighter editorial body layout below.
