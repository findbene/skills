---
name: "design-cursor"
description: "Apply Cursor design system aesthetic. An AI-first code editor whose marketing site reads like a quietly-confident developer-tools brand with a warm-cream editorial canvas (`#f7f7f4`) instead of the typical dark IDE atmosphere. Use when scaffolding UI components, generating styled pages, or matching Cursor's visual language (colors, typography, spacing, components). Loads bundled DESIGN.md with full token spec, type scale, component patterns, and brand rationale."
---

# Cursor Design System

Reusable design system skill scaffolded from `DESIGN.md`. Source: VoltAgent/awesome-claude-design + getdesign.md.

## When to use

User asks to build, scaffold, or style UI matching Cursor's visual language. Also: "make it look like Cursor", "use Cursor aesthetic", "Cursor-style components".

## How to apply

1. Read `DESIGN.md` (bundled in this skill dir) — contains full token spec.
2. Emit CSS variables from `colors:` section verbatim.
3. Build type scale from `typography:` block.
4. Apply component patterns from `components:` section.
5. Respect `dos_donts:` rules — these are guardrails.
6. Use `agent_prompt_guide:` reusable prompts for new screens.

## Files

- `DESIGN.md` — full design system spec (Cursor)

## Brand summary

An AI-first code editor whose marketing site reads like a quietly-confident developer-tools brand with a warm-cream editorial canvas (`#f7f7f4`) instead of the typical dark IDE atmosphere. Near-black warm ink (`#26251e`) carries body and display alike — display sits at weight 400 with negative letter-spacing for a magazine feel rather than a bold tech voice. The single brand voltage is **Cursor Orange** (`#f54e00`) reserved for primary CTAs and the wordmark. A signature pastel timeline palette (peach, mint, blue, lavender, gold) marks AI-action stages (Thinking / Reading / Editing / Grepping / Done) — only inside in-product timeline visualizations. Cards use minimal hairlines, no shadows, generous 80px section rhythm. CursorGothic for display/body, JetBrains Mono on every code surface (which is roughly half the page).
