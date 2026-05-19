# Mode: cartoon — Cartoon & Animation Style

Generate cartoon and animation video prompts across 15 art styles with animation principles, color palettes, and character animation.

## Process

1. Collect context: art style target (Disney, Looney Tunes, Pixar, Studio Ghibli, Flat Design, Retro Saturday morning, etc.), subject, duration, platform ratio
2. Read `~/.claude/skills/seedance-cartoon/references/details.md` for the 15-style encyclopedia, animation principles (12), color palette hex codes, and transition/effect library
3. Select art style from the 15-style encyclopedia — use the style's specific vocabulary and visual rules
4. Apply 2-second hook matching the art style's energy (slapstick entrance, magical appearance, dramatic zoom, comedic freeze)
5. Specify line work: line weight, stroke style, outline presence, fill style
6. Define color palette: select from palette library or specify custom with hex if needed
7. Add animation principles: squash-and-stretch, anticipation, follow-through as appropriate to style
8. Describe background style, character proportions, facial expression conventions
9. Build complete prompt with style name, color palette, line work, animation principle, hook in sequence
10. Verify: art style vocabulary used consistently, no mixing incompatible style languages

## Notes
- Style purity matters — mixing Disney vocabulary with anime terms confuses output
- Extended guide: `~/.claude/skills/seedance-cartoon/references/details.md`
