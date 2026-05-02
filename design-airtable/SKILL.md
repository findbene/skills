---
name: "design-airtable"
description: "Apply Airtable design system aesthetic. A sober, editorial workflow-software interface anchored on white canvas and dark-ink type, where brand voltage comes from full-bleed signature cards in coral, dark green, peach, and dark navy that pun. Use when scaffolding UI components, generating styled pages, or matching Airtable's visual language (colors, typography, spacing, components). Loads bundled DESIGN.md with full token spec, type scale, component patterns, and brand rationale."
---

# Airtable Design System

Reusable design system skill scaffolded from `DESIGN.md`. Source: VoltAgent/awesome-claude-design + getdesign.md.

## When to use

User asks to build, scaffold, or style UI matching Airtable's visual language. Also: "make it look like Airtable", "use Airtable aesthetic", "Airtable-style components".

## How to apply

1. Read `DESIGN.md` (bundled in this skill dir) — contains full token spec.
2. Emit CSS variables from `colors:` section verbatim.
3. Build type scale from `typography:` block.
4. Apply component patterns from `components:` section.
5. Respect `dos_donts:` rules — these are guardrails.
6. Use `agent_prompt_guide:` reusable prompts for new screens.

## Files

- `DESIGN.md` — full design system spec (Airtable)

## Brand summary

A sober, editorial workflow-software interface anchored on white canvas and dark-ink type, where brand voltage comes from full-bleed signature cards in coral, dark green, peach, and dark navy that punctuate long-scroll explainer pages. Primary actions use a near-black pill CTA; secondary actions sit in a white outlined button. Type runs Haas Grotesk in modest weights — never bold for its own sake.
