---
name: "design-replicate"
description: "Apply Replicate design system aesthetic. Replicate brand design system. Use when scaffolding UI components, generating styled pages, or matching Replicate's visual language (colors, typography, spacing, components). Loads bundled DESIGN.md with full token spec, type scale, component patterns, and brand rationale."
---

# Replicate Design System

Reusable design system skill scaffolded from `DESIGN.md`. Source: VoltAgent/awesome-claude-design + getdesign.md.

## When to use

User asks to build, scaffold, or style UI matching Replicate's visual language. Also: "make it look like Replicate", "use Replicate aesthetic", "Replicate-style components".

## How to apply

1. Read `DESIGN.md` (bundled in this skill dir) — contains full token spec.
2. Emit CSS variables from `colors:` section verbatim.
3. Build type scale from `typography:` block.
4. Apply component patterns from `components:` section.
5. Respect `dos_donts:` rules — these are guardrails.
6. Use `agent_prompt_guide:` reusable prompts for new screens.

## Files

- `DESIGN.md` — full design system spec (Replicate)

## Brand summary

Replicate brand design system.
