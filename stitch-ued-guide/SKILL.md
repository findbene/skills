---
name: stitch-ued-guide
description: "Visual vocabulary, design terminology, and prompt engineering strategy for Stitch. Triggers: 'use stitch-ued-guide', 'stitch ued guide', 'stitch-ued-guide task'."
allowed-tools: []
---

# Stitch UED Guide

This is a reference guide, not an action skill. Use it when:
- You need specific design terminology for a Stitch prompt (masonry, glassmorphism, split-screen)
- You're unsure how to describe a layout pattern or visual style
- You want to apply color + theme structure consistently
- You need to know device-specific constraints for prompt writing

## When NOT to use

- Generating actual Stitch screens — use `stitch-mcp-generate-screen-from-text` (this is a reference guide only)
- Generating real working code — use `stitch-nextjs-components`, `stitch-html-components`, etc.
- Brand design system creation — use `stitch-mcp-create-design-system`
- Non-Stitch design vocabulary needs — use `design-system` or `ui-design-system` skills
- Final prompt construction — use `stitch-ui-prompt-architect`

## Red Flags

| Rationalization | Reality |
|---|---|
| "I will write the Stitch prompt without checking vocabulary" | Stitch is sensitive to specific terms (masonry, glassmorphism, split-screen) — vague prompts produce generic outputs |
| "Pick any colorVariant, they look similar" | Variant choice changes the entire palette derivation — match it to brand vibe (MONOCHROME for luxury, EXPRESSIVE for creative) |
| "Skip the 4-part prompt structure, just write a paragraph" | The structure (Context + Layout + Components + Content) reliably outperforms freeform prompts in Stitch model evals |
| "Use GEMINI_3_PRO, it is in the list" | GEMINI_3_PRO is deprecated; default to GEMINI_3_1_PRO |

## Output Contract

Finished output must contain:
- The specific vocabulary term(s) recommended for the user's intent
- Reasoning tying choice to one of the documented archetypes (layout, color, spacing)
- A 4-part prompt block ready to paste into Stitch
- Model selection recommendation (`GEMINI_3_1_PRO` default, `GEMINI_3_FLASH` for speed)
- colorVariant + spacingScale recommendation aligned with brand vibe
- Notes on device-specific constraints if the design targets mobile

## Examples

**Example 1 — Translate vague brief to Stitch prompt**
- Input: "I want a luxurious dashboard for high-end watch buyers"
- Action: Look up vocabulary → luxury → MONOCHROME or NEUTRAL colorVariant + serif headers + generous spacing → produce 4-part prompt
- Output: "Context: Luxury watch retailer dashboard. MONOCHROME palette derived from deep espresso `#2A1810`. Editorial serif (Playfair Display) headers, sans-serif body. Generous whitespace. // Layout: Sticky top bar, sidebar nav, main canvas with hero watch carousel. // Components: Glass cards, gold hairline borders, lowercase italic subheaders. // Content: 'Patek Philippe Nautilus 5711', '$135,000', 'Last bid 12 minutes ago'."

**Example 2 — Pick spacingScale + colorVariant for a kids app**
- Input: "Designing a kids' learning app, ages 6-10"
- Action: Recommend RAINBOW colorVariant + LOOSE spacing scale + rounded heavy sans-serif typography
- Output: Recommendation + prompt skeleton for mobile (MOBILE deviceType) with playful tone

## References

Extended sections moved to `references/details.md`.
