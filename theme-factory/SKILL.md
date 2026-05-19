---
name: theme-factory
description: "Applies professional color and typography themes to any artifact — slides, docs, reports, HTML landing pages, and more. Triggers: 'use theme-factory', 'theme factory', 'theme-factory task'."
allowed-tools: Glob, Grep, Read
license: Complete terms in LICENSE.txt
---


# Theme Factory Skill

This skill provides a curated collection of professional font and color themes themes, each with carefully selected color palettes and font pairings. Once a theme is chosen, it can be applied to any artifact.

## Purpose

To apply consistent, professional styling to presentation slide decks, use this skill. Each theme includes:
- A cohesive color palette with hex codes
- Complementary font pairings for headers and body text
- A distinct visual identity suitable for different contexts and audiences

## Usage Instructions

To apply styling to a slide deck or other artifact:

1. **Show the theme showcase**: Display the `theme-showcase.pdf` file to allow users to see all available themes visually. Do not make any modifications to it; simply show the file for viewing. → verify: step output matches expected outcome
2. **Ask for their choice**: Ask which theme to apply to the deck → verify: diff matches intended change
3. **Wait for selection**: Get explicit confirmation about the chosen theme → verify: step output matches expected outcome
4. **Apply the theme**: Once a theme has been chosen, apply the selected theme's colors and fonts to the deck/artifact → verify: diff matches intended change

## Themes Available

The following 10 themes are available, each showcased in `theme-showcase.pdf`:

1. **Ocean Depths** - Professional and calming maritime theme → verify: step output matches expected outcome
2. **Sunset Boulevard** - Warm and vibrant sunset colors → verify: step output matches expected outcome
3. **Forest Canopy** - Natural and grounded earth tones → verify: step output matches expected outcome
4. **Modern Minimalist** - Clean and contemporary grayscale → verify: step output matches expected outcome
5. **Golden Hour** - Rich and warm autumnal palette → verify: step output matches expected outcome
6. **Arctic Frost** - Cool and crisp winter-inspired theme → verify: step output matches expected outcome
7. **Desert Rose** - Soft and sophisticated dusty tones → verify: step output matches expected outcome
8. **Tech Innovation** - Bold and modern tech aesthetic → verify: step output matches expected outcome
9. **Botanical Garden** - Fresh and organic garden colors → verify: step output matches expected outcome
10. **Midnight Galaxy** - Dramatic and cosmic deep tones → verify: step output matches expected outcome

## Theme Details

Each theme is defined in the `themes/` directory with complete specifications including:
- Cohesive color palette with hex codes
- Complementary font pairings for headers and body text
- Distinct visual identity suitable for different contexts and audiences

## Application Process

After a preferred theme is selected:
1. Read the corresponding theme file from the `themes/` directory → verify: file readable + content matches expected shape
2. Apply the specified colors and fonts consistently throughout the deck → verify: diff matches intended change
3. Ensure proper contrast and readability → verify: file readable + content matches expected shape
4. Maintain the theme's visual identity across all slides → verify: step output matches expected outcome

## Create your Own Theme
To handle cases where none of the existing themes work for an artifact, create a custom theme. Based on provided inputs, generate a new theme similar to the ones above. Give the theme a similar name describing what the font/color combinations represent. Use any basic description provided to choose appropriate colors/fonts. After generating the theme, show it for review and verification. Following that, apply the theme as described above.

## Triggers

the user wants to style an artifact, pick a theme, change colors/fonts, or make a presentation look polished

## When NOT to use

- Task is unrelated to theme factory — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Theme Factory needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for theme factory
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving theme factory
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
