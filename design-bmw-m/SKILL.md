---
name: "design-bmw-m"
description: "Apply BMW M design system aesthetic. A motorsport-engineering interface anchored on a near-black canvas with white BMW Type Next Latin display headlines in confident UPPERCASE. Use when scaffolding UI components, generating styled pages, or matching BMW M's visual language (colors, typography, spacing, components). Loads bundled DESIGN.md with full token spec, type scale, component patterns, and brand rationale."
---

# BMW M Design System

Reusable design system skill scaffolded from `DESIGN.md`. Source: VoltAgent/awesome-claude-design + getdesign.md.

## When to use

User asks to build, scaffold, or style UI matching BMW M's visual language. Also: "make it look like BMW M", "use BMW M aesthetic", "BMW M-style components".

## How to apply

1. Read `DESIGN.md` (bundled in this skill dir) — contains full token spec.
2. Emit CSS variables from `colors:` section verbatim.
3. Build type scale from `typography:` block.
4. Apply component patterns from `components:` section.
5. Respect `dos_donts:` rules — these are guardrails.
6. Use `agent_prompt_guide:` reusable prompts for new screens.

## Files

- `DESIGN.md` — full design system spec (BMW M)

## Brand summary

A motorsport-engineering interface anchored on a near-black canvas with white BMW Type Next Latin display headlines in confident UPPERCASE. The brand carries no decorative voltage — its energy comes from full-bleed automotive photography (cars on tracks, driver-cockpit shots, carbon-fiber detail) and the iconic M tricolor stripe (light blue → dark blue → red) used sparingly as a brand signature on logos, dividers, and motorsport chrome. Type stays light to medium weight to feel European-engineered, never American-bombastic.
