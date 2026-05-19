---
name: "frontend-design-anti-slop"
description: 'Creates distinctive, production-grade frontend interfaces with exceptional design quality. Triggers: "use frontend-design-anti-slop", "frontend design anti slop", "frontend task".'
allowed-tools: Glob, Grep, Read
---


## Frontend Design Quality Guard

You tend to produce "AI slop" by default: Inter or system fonts, purple-on-white gradients, symmetric card grids, flat white backgrounds. Break this pattern deliberately.

### Typography
Choose unexpected, characterful typefaces. Pair a distinctive display font (e.g. Playfair Display, DM Serif Display, Syne, Bebas Neue, Instrument Serif) with a refined body font. Never use Inter, Roboto, Arial, or Space Grotesk — they signal zero creative investment.

### Color & Theme
Commit fully to one coherent aesthetic. Use CSS custom properties for every color token. Dominant accent colors with sharp contrast outperform timid palettes. Avoid purple gradients on white backgrounds — they are the single most recognizable AI default. Try: deep navy + warm amber, charcoal + acid green, cream + burgundy + gold, near-black + electric cyan.

Express colors in `oklch()` where possible — it gives perceptually uniform lightness steps and vivid gamut-P3 hues without the muddy mid-tones of hex-RGB.

### Motion
CSS-only for HTML artifacts. One well-orchestrated page-load with staggered `animation-delay` reveals is better than scattered micro-interactions. Use `@media (prefers-reduced-motion: reduce)` to gate all animations.

### Spatial Composition
Asymmetry. Overlap. Diagonal rhythm. Grid-breaking hero elements. Resist the urge to center-align everything — left-aligned, offset, or deliberately edge-bleeding layouts feel more crafted.

### Backgrounds
Never solid white or solid grey. Use CSS gradients, noise/grain overlay (`background-image: url("data:image/svg+xml,...")`), geometric SVG patterns, or subtle radial glows. One grain overlay (`opacity: 0.04`) on a colored background immediately elevates perceived quality.

### Tone Commitment
Pick **one** tone and execute it with full craft: brutally minimal / maximalist chaos / retro-futuristic / organic warmth / luxury editorial / bold experimental. Do not hedge between styles — half-committed aesthetics look worse than any single extreme.

### Hard Prohibitions
NEVER use: Inter, Roboto, Arial, system-ui, Space Grotesk as the primary typeface; purple gradients on white; symmetric 3-column card grids as the only layout element; drop shadows that look like Bootstrap defaults; placeholder grey rectangles as "images".

## When NOT to use

- Strict brand systems where deviation is forbidden — use `brand-guidelines` or `design-system`
- Pure UI scaffolding for internal admin tools where distinctiveness is wasted
- A11y-first surfaces where contrast/clarity outweigh distinctiveness
- Backend or non-visual code generation
- Charts/data viz only — use `data-viz-recharts`

## Red Flags

| Thought | Reality |
|---------|---------|
| "Inter is professional and safe" | It signals zero creative investment; pick a characterful pair |
| "Add a purple-to-blue gradient hero" | The single most recognizable AI default; banned |
| "Center every section" | Asymmetric/offset layouts read as crafted |
| "Pure white background is clean" | Reads as AI default; use grain, gradient, or tinted neutral |

## Output Contract

Done when:
- Display + body font pair selected; neither is Inter/Roboto/Arial/system-ui/Space Grotesk
- Color palette committed to one aesthetic (no AI purple-on-white)
- Colors expressed in `oklch()` where possible
- Background uses gradient / grain overlay / SVG pattern / radial glow — never solid white
- Asymmetric layout with at least one grid-breaking hero element
- One tone fully committed (minimal / maximalist / retro-futuristic / etc.)
- All animations gated by `prefers-reduced-motion`

## Examples

### Example 1 — Landing page hero
- Input: "Build a SaaS landing hero"
- Action: Pair Instrument Serif + IBM Plex Sans, color = deep navy + warm amber accent in oklch, grain overlay 0.04, offset-left hero copy with image bleeding off the right edge, staggered intro animation gated by prefers-reduced-motion
- Output: Distinctive hero that does not read as AI default; semantic HTML + CSS custom properties

### Example 2 — Pricing page
- Input: "Pricing page for our SaaS"
- Action: Drop symmetric 3-card grid; use 2-column offset layout with one hero tier breaking the grid, charcoal + acid-green palette, subtle radial glow background, distinct display font for tier names
- Output: Pricing layout with one focal tier, asymmetric balance, fully committed retro-futurist tone
