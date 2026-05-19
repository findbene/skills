---
name: slides
description: "HTML presentation creation with Chart.js, design tokens, and data visualization for strategic pitch decks and technical slide presentations. Triggers: 'use slides', 'slides', 'slides task'."
allowed-tools: Glob, Grep, Read
metadata:
  author: claudekit
  version: "1.0.0"
---

# Slides

Strategic HTML presentation design with data visualization.

<args>$ARGUMENTS</args>

## When to Use

- Marketing presentations and pitch decks
- Data-driven slides with Chart.js
- Strategic slide design with layout patterns
- Copywriting-optimized presentation content

## Subcommands

| Subcommand | Description | Reference |
|------------|-------------|-----------|
| `create` | Create strategic presentation slides | `references/create.md` |

## When NOT to use

- User wants a Google Slides / PowerPoint `.pptx` export — use `pptx` skill instead
- Quick text-only outline with no visuals — overkill, use plain markdown
- Pure interactive web app or product UI — use `stitch-html-components` or `frontend-design`
- Single-slide social graphic — use `banner-design` or image generation skills
- Long-form report with footnotes and citations — use `docx` or `pdf` skills

## Red Flags

| Rationalization | Reality |
|---|---|
| "Charts are optional, skip Chart.js setup" | Data slides without visualization are why decks lose attention; if claiming data, render it |
| "One huge slide of bullet points is fine" | Audiences cannot read >6 bullets at once; split or rewrite as a story arc |
| "Default font and colors will do" | Generic look kills credibility on pitch decks; load `layout-patterns.md` for design tokens |
| "Skip the copy formulas, the content is self-explanatory" | Without a hook/payoff structure (load `copywriting-formulas.md`) slides read as a wall of facts |

## Output Contract

Finished output must contain:
- A single HTML file (or numbered set) with one `<section class="slide">` per slide
- Design tokens (colors, fonts, spacing) defined as CSS custom properties at the top
- At least one Chart.js visualization when the deck contains quantitative claims
- Speaker-notes block per slide (`<aside class="notes">`) — even if short
- Print/export-friendly styles (`@media print` page-break-after: always per slide)
- Title slide + closing CTA/contact slide bracketing the body

## Examples

**Example 1 — Strategic pitch deck**
- Input: "10-slide Series A pitch for a B2B fintech, $5M ARR, asking $20M"
- Action: Load `slide-strategies.md` → pick pitch arc → load `layout-patterns.md` → load `copywriting-formulas.md` for headline hooks → generate HTML with Chart.js ARR growth chart on slide 4
- Output: `pitch.html` with 10 sections, ARR line chart, problem/solution/traction/ask/team slides, monochrome design tokens

**Example 2 — Technical conference talk**
- Input: "30-min talk on event-driven architecture, audience: senior engineers"
- Action: Load `slide-strategies.md` → pick teach arc → minimal Chart.js (one throughput comparison) → code-block slides with monospace tokens
- Output: `talk.html` with 18 sections, syntax-highlighted code samples, one bar chart comparing sync vs async throughput, speaker notes

## References (Knowledge Base)

| Topic | File |
|-------|------|
| Layout Patterns | `references/layout-patterns.md` |
| HTML Template | `references/html-template.md` |
| Copywriting Formulas | `references/copywriting-formulas.md` |
| Slide Strategies | `references/slide-strategies.md` |

## Routing

1. Parse subcommand from `$ARGUMENTS` (first word) → verify: step output matches expected outcome
2. Load corresponding `references/{subcommand}.md` → verify: file content matches expected shape
3. Execute with remaining arguments → verify: command exit code 0

## Triggers

\\\"slides\\\", \\\"create slides\\\", \\\"HTML presentation\\\", \\\"slide deck\\\", \\\"presentation code\\\", \\\"Chart.js slides\\\", \\\"data visualization slides\\\", \\\"slide design\\\", \\\"build a presentation\\\", \\\"create..."
argument-hint: "[topic] [slide-count]
