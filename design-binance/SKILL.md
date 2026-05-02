---
name: "design-binance"
description: "Apply Binance design system aesthetic. A confident financial-platform interface anchored on a deep near-black canvas, where Binance's iconic yellow (#FCD535) carries every primary CTA, brand accent, and value-claim moment. Use when scaffolding UI components, generating styled pages, or matching Binance's visual language (colors, typography, spacing, components). Loads bundled DESIGN.md with full token spec, type scale, component patterns, and brand rationale."
---

# Binance Design System

Reusable design system skill scaffolded from `DESIGN.md`. Source: VoltAgent/awesome-claude-design + getdesign.md.

## When to use

User asks to build, scaffold, or style UI matching Binance's visual language. Also: "make it look like Binance", "use Binance aesthetic", "Binance-style components".

## How to apply

1. Read `DESIGN.md` (bundled in this skill dir) — contains full token spec.
2. Emit CSS variables from `colors:` section verbatim.
3. Build type scale from `typography:` block.
4. Apply component patterns from `components:` section.
5. Respect `dos_donts:` rules — these are guardrails.
6. Use `agent_prompt_guide:` reusable prompts for new screens.

## Files

- `DESIGN.md` — full design system spec (Binance)

## Brand summary

A confident financial-platform interface anchored on a deep near-black canvas, where Binance's iconic yellow (#FCD535) carries every primary CTA, brand accent, and value-claim moment. Type runs Binance's custom BinanceNova / BinancePlex stack at modest weights — the system trusts size and yellow voltage over bold weight. Marketing and product surfaces default to the dark theme; transactional surfaces (buy crypto, deposit, exchange) flip to a light theme that shares the same yellow CTAs and gray-blue hairlines. Trading green (up) and red (down) accents thread through both modes for price-direction signals.
