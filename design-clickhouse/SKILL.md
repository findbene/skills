---
name: "design-clickhouse"
description: "Apply ClickHouse design system aesthetic. A high-performance database interface anchored on near-pure black canvas with electric yellow as the brand voltage. Use when scaffolding UI components, generating styled pages, or matching ClickHouse's visual language (colors, typography, spacing, components). Loads bundled DESIGN.md with full token spec, type scale, component patterns, and brand rationale."
---

# ClickHouse Design System

Reusable design system skill scaffolded from `DESIGN.md`. Source: VoltAgent/awesome-claude-design + getdesign.md.

## When to use

User asks to build, scaffold, or style UI matching ClickHouse's visual language. Also: "make it look like ClickHouse", "use ClickHouse aesthetic", "ClickHouse-style components".

## How to apply

1. Read `DESIGN.md` (bundled in this skill dir) — contains full token spec.
2. Emit CSS variables from `colors:` section verbatim.
3. Build type scale from `typography:` block.
4. Apply component patterns from `components:` section.
5. Respect `dos_donts:` rules — these are guardrails.
6. Use `agent_prompt_guide:` reusable prompts for new screens.

## Files

- `DESIGN.md` — full design system spec (ClickHouse)

## Brand summary

A high-performance database interface anchored on near-pure black canvas with electric yellow as the brand voltage. White typography in confident sans, yellow CTAs, and yellow-text stat numbers carry the brand voice across every page. Code blocks and product UI fragments embed directly in dark cards. The yellow + black pairing (and yellow used scarcely as accent) is the system's signature — brand identity without atmospheric decoration.
