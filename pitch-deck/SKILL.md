---
name: "pitch-deck"
description: 'Designs polished pitch deck slides and presentation layouts. Use when the user asks for a slide deck, investor pitch, presentation, or multi-sli. Triggers: "use pitch-deck", "pitch deck", "pitch task.'
allowed-tools: Glob, Grep, Read
---


## Pitch Deck Design Principles

### One Claim Per Slide
Every slide communicates exactly one idea. If you find yourself writing "and also..." you need two slides. The claim should be expressible in a single sentence of ≤ 12 words placed at the top of the slide.

### Typographic Hierarchy
Use a 3-tier scale: headline (56–72px), supporting statement (24–32px), body/caption (14–16px). Never mix more than two font families. Display font for headlines; neutral font for body. Tight letter-spacing (−0.02em to −0.04em) on large headlines reads as confidence.

### Whitespace as Signal
Leave at least 20% of each slide empty. Crowded slides signal a presenter who does not trust the audience. Generous margins (8–12% of slide width) and vertical padding between elements are non-negotiable.

### Slide Ratio and Dimensions
Default to 16:9 (1920×1080px or 1280×720px). Avoid 4:3 — it looks dated. For mobile-first contexts (scrollable deck on phone) use 9:16.

### Data Slides
Every chart needs: a headline that states the insight (not just the metric), axis labels in the same font as the slide body, and a clear color key. Never use 3-D charts or pie charts with more than 4 segments. Bar charts and area charts read fastest. Use a single accent color for the data series you want the audience to notice; grey out the rest.

### Color Discipline
Limit the palette to 3 colors: background, text, and one accent. If you need a second accent, make it a tint of the first. Every slide in the deck shares the same palette — do not introduce new colors mid-deck.

### Opening and Closing Slides
The opening slide must make the problem viscerally clear in under 5 seconds of reading. The closing slide must state the single desired action (invest, partner, join) with contact information — not a generic "Thank you" or "Q&A".

### Narrative Arc
Structure: Problem → Insight → Solution → Evidence → Ask. Each section should be 1–3 slides. Do not spend more than 25% of the deck on product features.

## When NOT to use

- Quarterly board update — use `board-deck-builder`
- Sales presentation to a buyer — use `sales-enablement` framing
- Internal product roadmap deck — use `slides` with a different structure
- A single one-pager or memo — write prose
- Conference keynote with heavy visuals — different design constraints

## Red Flags

| Thought | Reality |
|---------|---------|
| "Two claims per slide for efficiency" | One claim per slide; if you say 'and also', it's two slides |
| "End with 'Thank You / Q&A'" | Closing slide = single desired action with contact info |
| "4:3 ratio looks classic" | Reads as dated; use 16:9 (or 9:16 for mobile-first) |
| "Use pie chart for everything" | Bar/area read fastest; never 3D, never pie with >4 segments |

## Output Contract

Done when:
- One claim per slide, ≤12 words headline at top
- 3-tier typographic scale, ≤2 font families
- ≥20% empty space per slide
- 16:9 (or 9:16 mobile) ratio
- 3-color palette throughout (background + text + accent), shared across all slides
- Opening slide makes the problem viscerally clear in <5s
- Closing slide names one specific action + contact
- Narrative follows Problem → Insight → Solution → Evidence → Ask
- ≤25% of deck on product features

## Examples

### Example 1 — Seed-stage investor deck
- Input: "10-slide seed pitch for our AI dental front-desk product"
- Action: Slide 1 problem ("dentists lose 80 calls/week"), 2 insight, 3-4 solution, 5-7 evidence (pilot metrics, testimonial, projection), 8 ask ($1.5M for 12 months), 9 team, 10 contact + specific action
- Output: 10-slide PDF, 16:9, 3-color palette, one claim per slide, contact slide names the next step

### Example 2 — Mobile pitch for X Twitter
- Input: "Pitch as scrollable carousel for sharing"
- Action: Switch to 9:16, reduce slide count to 7, headlines tightened to 8 words, big touch-friendly type
- Output: 9:16 PDF or PNG sequence, mobile-readable, narrative arc preserved
