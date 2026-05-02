---
name: "design-bmw"
description: "Apply BMW design system aesthetic. BMW's corporate site — distinct from BMW M's motorsport-bombastic variant, this is a measured and settled corporate-automotive interface. Use when scaffolding UI components, generating styled pages, or matching BMW's visual language (colors, typography, spacing, components). Loads bundled DESIGN.md with full token spec, type scale, component patterns, and brand rationale."
---

# BMW Design System

Reusable design system skill scaffolded from `DESIGN.md`. Source: VoltAgent/awesome-claude-design + getdesign.md.

## When to use

User asks to build, scaffold, or style UI matching BMW's visual language. Also: "make it look like BMW", "use BMW aesthetic", "BMW-style components".

## How to apply

1. Read `DESIGN.md` (bundled in this skill dir) — contains full token spec.
2. Emit CSS variables from `colors:` section verbatim.
3. Build type scale from `typography:` block.
4. Apply component patterns from `components:` section.
5. Respect `dos_donts:` rules — these are guardrails.
6. Use `agent_prompt_guide:` reusable prompts for new screens.

## Files

- `DESIGN.md` — full design system spec (BMW)

## Brand summary

BMW's corporate site — distinct from BMW M's motorsport-bombastic variant, this is a measured and settled corporate-automotive interface. On a light (cream-tinted white) canvas, BMW corporate blue (#1c69d4) carries every primary CTA; dark navy hero bands frame model photography. BMW Type Next Latin sets the entire hierarchy on two weights — heavy 700 display and Light 300 body. Configuration and reservation flows ride a card-based 4-up grid, where each card holds a model render, a name, and a "Learn More" link.
