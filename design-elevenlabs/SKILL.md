---
name: "design-elevenlabs"
description: "Apply ElevenLabs design system aesthetic. A voice-AI brand whose marketing surfaces read like a quietly editorial print magazine. Use when scaffolding UI components, generating styled pages, or matching ElevenLabs's visual language (colors, typography, spacing, components). Loads bundled DESIGN.md with full token spec, type scale, component patterns, and brand rationale."
---

# ElevenLabs Design System

Reusable design system skill scaffolded from `DESIGN.md`. Source: VoltAgent/awesome-claude-design + getdesign.md.

## When to use

User asks to build, scaffold, or style UI matching ElevenLabs's visual language. Also: "make it look like ElevenLabs", "use ElevenLabs aesthetic", "ElevenLabs-style components".

## How to apply

1. Read `DESIGN.md` (bundled in this skill dir) — contains full token spec.
2. Emit CSS variables from `colors:` section verbatim.
3. Build type scale from `typography:` block.
4. Apply component patterns from `components:` section.
5. Respect `dos_donts:` rules — these are guardrails.
6. Use `agent_prompt_guide:` reusable prompts for new screens.

## Files

- `DESIGN.md` — full design system spec (ElevenLabs)

## Brand summary

A voice-AI brand whose marketing surfaces read like a quietly editorial print magazine. The base canvas is off-white (`#f5f5f5`) holding warm near-black ink (`#292524`); the brand voltage is photographic, not chromatic — soft pastel atmospheric gradient orbs (mint → peach → lavender → sky) drift through the page as the only "color" moments. Display runs Waldenburg Light at weight 300 — the editorial signature. Inter carries body, navigation, captions. CTAs are subtle: a near-black ink pill is the primary, a transparent outline is the secondary. The brand trusts atmospheric photography and modest type weights to do all of the brand work; there is no neon accent, no saturated CTA color, no developer-tools dark canvas.
