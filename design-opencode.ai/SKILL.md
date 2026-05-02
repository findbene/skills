---
name: "design-opencode.ai"
description: "Apply Opencode.Ai design system aesthetic. Opencode. Use when scaffolding UI components, generating styled pages, or matching Opencode.Ai's visual language (colors, typography, spacing, components). Loads bundled DESIGN.md with full token spec, type scale, component patterns, and brand rationale."
---

# Opencode.Ai Design System

Reusable design system skill scaffolded from `DESIGN.md`. Source: VoltAgent/awesome-claude-design + getdesign.md.

## When to use

User asks to build, scaffold, or style UI matching Opencode.Ai's visual language. Also: "make it look like Opencode.Ai", "use Opencode.Ai aesthetic", "Opencode.Ai-style components".

## How to apply

1. Read `DESIGN.md` (bundled in this skill dir) — contains full token spec.
2. Emit CSS variables from `colors:` section verbatim.
3. Build type scale from `typography:` block.
4. Apply component patterns from `components:` section.
5. Respect `dos_donts:` rules — these are guardrails.
6. Use `agent_prompt_guide:` reusable prompts for new screens.

## Files

- `DESIGN.md` — full design system spec (Opencode.Ai)

## Brand summary

Opencode.Ai brand design system.
