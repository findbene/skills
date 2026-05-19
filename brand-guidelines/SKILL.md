---
name: brand-guidelines
description: "Applies Anthropic's official brand colors and typography to any artifact that needs Anthropic's visual identity. Triggers: 'use brand-guidelines', 'brand guidelines', 'brand-guidelines task'."
allowed-tools: Glob, Grep, Read
license: Complete terms in LICENSE.txt
---

# Anthropic Brand Styling

## Overview

To access Anthropic's official brand identity and style resources, use this skill.

**Keywords**: branding, corporate identity, visual identity, post-processing, styling, brand colors, typography, Anthropic brand, visual formatting, visual design

## Brand Guidelines

### Colors

**Main Colors:**

- Dark: `#141413` - Primary text and dark backgrounds
- Light: `#faf9f5` - Light backgrounds and text on dark
- Mid Gray: `#b0aea5` - Secondary elements
- Light Gray: `#e8e6dc` - Subtle backgrounds

**Accent Colors:**

- Orange: `#d97757` - Primary accent
- Blue: `#6a9bcc` - Secondary accent
- Green: `#788c5d` - Tertiary accent

### Typography

- **Headings**: Poppins (with Arial fallback)
- **Body Text**: Lora (with Georgia fallback)
- **Note**: Fonts should be pre-installed in your environment for best results

## Features

### Smart Font Application

- Applies Poppins font to headings (24pt and larger)
- Applies Lora font to body text
- Automatically falls back to Arial/Georgia if custom fonts unavailable
- Preserves readability across all systems

### Text Styling

- Headings (24pt+): Poppins font
- Body text: Lora font
- Smart color selection based on background
- Preserves text hierarchy and formatting

### Shape and Accent Colors

- Non-text shapes use accent colors
- Cycles through orange, blue, and green accents
- Maintains visual interest while staying on-brand

## Technical Details

### Font Management

- Uses system-installed Poppins and Lora fonts when available
- Provides automatic fallback to Arial (headings) and Georgia (body)
- No font installation required - works with existing system fonts
- For best results, pre-install Poppins and Lora fonts in your environment

### Color Application

- Uses RGB color values for precise brand matching
- Applied via python-pptx's RGBColor class
- Maintains color fidelity across different systems

## Triggers

Anthropic style\", \"brand guidelines\", or wants visual formatting that reflects company design standards

## When NOT to use

- Non-Anthropic brand work (apply that brand's tokens, not these)
- Logo design or new brand identity creation — outside this skill's scope
- Web app theming — use `design-system` or `theme-factory` with brand tokens
- Marketing collateral that requires brand approval workflow — escalate to brand team
- Pure typesetting without color/identity application — use `typeset`

## Red Flags

| Thought | Reality |
|---------|---------|
| "Use a similar orange, close enough" | Hex must be exact `#d97757`; brand consistency is the point |
| "Swap Lora for Times, it's close" | Fallback chain is defined for a reason — Lora → Georgia, not arbitrary serifs |
| "Apply accent color everywhere" | Accents are accents — cycle them, don't saturate |
| "Heading at 18pt gets Poppins" | Threshold is 24pt+; below that, body font (Lora) applies |

## Output Contract

Done when:
- Brand colors applied as exact hex values: `#141413`, `#faf9f5`, `#b0aea5`, `#e8e6dc` + accents `#d97757`, `#6a9bcc`, `#788c5d`
- Headings (24pt+) use Poppins with Arial fallback
- Body text uses Lora with Georgia fallback
- Non-text shapes cycle through accent colors (orange, blue, green)
- Background-aware color selection preserves contrast
- Text hierarchy preserved across all systems with or without custom fonts

## Examples

### Example 1 — Style an existing PPTX deck to Anthropic brand
- Input: User pastes a draft pptx and asks to apply Anthropic styling
- Action: Walk slides, set heading fonts to Poppins (>=24pt), body to Lora, swap palette colors to brand hex, cycle accent shapes through orange/blue/green
- Output: Restyled .pptx with consistent brand application across every slide

### Example 2 — Apply brand to a generated chart
- Input: Matplotlib chart needs Anthropic styling
- Action: Set background `#faf9f5`, axes/text `#141413`, primary series `#d97757`, secondary `#6a9bcc`, tertiary `#788c5d`, title font Poppins, body font Lora
- Output: Chart PNG with exact brand palette and typography
