---
name: "design-cal"
description: "Apply Cal.com design system aesthetic. A clean, calendar-software-first interface anchored on white canvas with black primary CTAs and custom Cal Sans display typography. Use when scaffolding UI components, generating styled pages, or matching Cal.com's visual language (colors, typography, spacing, components). Loads bundled DESIGN.md with full token spec, type scale, component patterns, and brand rationale."
---

# Cal.com Design System

Reusable design system skill scaffolded from `DESIGN.md`. Source: VoltAgent/awesome-claude-design + getdesign.md.

## When to use

User asks to build, scaffold, or style UI matching Cal.com's visual language. Also: "make it look like Cal.com", "use Cal.com aesthetic", "Cal.com-style components".

## How to apply

1. Read `DESIGN.md` (bundled in this skill dir) — contains full token spec.
2. Emit CSS variables from `colors:` section verbatim.
3. Build type scale from `typography:` block.
4. Apply component patterns from `components:` section.
5. Respect `dos_donts:` rules — these are guardrails.
6. Use `agent_prompt_guide:` reusable prompts for new screens.

## Files

- `DESIGN.md` — full design system spec (Cal.com)

## Brand summary

A clean, calendar-software-first interface anchored on white canvas with black primary CTAs and custom Cal Sans display typography. The system reads as friendly modern SaaS — generous whitespace, soft-rounded cards (~12px), product UI fragments shown directly inside cards, and a dark navy footer that visually closes long-scroll pages. Brand voltage comes from the Cal Sans display headline (a custom geometric face) and from product UI artifacts shown in-card rather than from accent colors.
