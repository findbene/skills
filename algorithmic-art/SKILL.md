---
name: algorithmic-art
description: 'Algorithmic and generative art creation using p5.js with seeded randomness, particle systems, flow fields, and noise-based visua. Triggers: "use algorithmic-art", "algorithmic art", "algorithmic task.'
allowed-tools: Glob, Grep, Read
---

# Algorithmic Art

Create generative art through code, expressing computational aesthetics with reproducible, interactive results.

## Two-Step Process

1. **Algorithmic Philosophy Creation** (.md file) → verify: step output matches expected outcome
2. **Express through p5.js** (.html + .js files) → verify: step output matches expected outcome

## Creating an Algorithmic Philosophy

### Name the Movement (1-2 words)
Examples: "Organic Turbulence", "Quantum Harmonics", "Emergent Stillness"

### Articulate the Philosophy (4-6 paragraphs)
Express how this philosophy manifests through:
- Computational processes and mathematical relationships
- Noise functions and randomness patterns
- Particle behaviors and field dynamics
- Temporal evolution and system states
- Parametric variation and emergent complexity

### Philosophy Examples

**"Organic Turbulence"**: Flow fields driven by layered Perlin noise. Particles following vector forces, trails accumulating into organic density maps.

**"Quantum Harmonics"**: Particles with phase values exhibiting wave-like interference. Constructive interference creates bright nodes, destructive creates voids.

**"Field Dynamics"**: Vector fields from mathematical functions. Particles flow along field lines, showing ghost-like evidence of invisible forces.

## p5.js Implementation

### Seeded Randomness

Seeded randomness is essential — it makes art reproducible. The same seed produces the identical output, which lets users explore and share specific compositions.

```javascript
let seed = 12345;
randomSeed(seed);
noiseSeed(seed);
```

### Parameter Structure
```javascript
let params = {
  seed: 12345,
  // Quantities, Scales, Probabilities, Ratios, Angles, Thresholds
};
```

### Required Features

1. **Parameter Controls**: Sliders, color pickers, real-time updates → verify: step output matches expected outcome
2. **Seed Navigation**: Previous/Next, Random, Jump to seed → verify: step output matches expected outcome
3. **Self-Contained HTML**: Everything inline, p5.js from CDN → verify: step output matches expected outcome

## Craftsmanship

- **Balance**: Complexity without visual noise — restraint is what separates generative art from random noise
- **Color Harmony**: Thoughtful palettes, not random RGB values
- **Composition**: Visual hierarchy even in randomness
- **Performance**: Smooth execution at interactive framerates
- **Reproducibility**: Same seed produces identical output every time

## When NOT to use

- User wants a static logo, icon, or marketing asset — use `canvas-design` or `nano-banana-pro`
- Photo-realistic or representational imagery — generative AI image tools fit better
- UI design or product mockups — use `frontend-design` or `ui-to-code`
- Data visualization with a real dataset — use `data-viz-recharts` or `algorithmic-art` is wrong tool
- Print/PDF poster output where p5.js canvas resolution is a hard limit

## Red Flags

| Thought | Reality |
|---------|---------|
| "I'll use unseeded `random()` calls" | Output becomes irreproducible; users can't share or revisit a composition |
| "Random RGB looks fine" | It looks like AI slop; thoughtful palettes are what separate art from noise |
| "Add 10 layered effects for richness" | Visual noise; restraint is craftsmanship |
| "Skip the philosophy .md, just write code" | The philosophy is what makes output feel intentional vs. random |

## Output Contract

Done when:
- Philosophy `.md` file written: named movement + 4-6 paragraph manifesto
- Self-contained `index.html` with p5.js loaded from CDN and inline JS
- `randomSeed()` and `noiseSeed()` both seeded from a parameter
- Seed navigation UI (Previous / Next / Random / Jump-to-seed) wired
- Parameter controls (sliders, color pickers) update output live
- Output runs smoothly at interactive framerates

## Examples

### Example 1 — "Organic Turbulence" flow field
- Input: "Make a generative piece inspired by wind currents"
- Action: Write philosophy `organic-turbulence.md` (layered Perlin noise, particle trails, restraint), build `index.html` with 2000 particles following `noise(x*scale, y*scale, t)` vector field, seeded RNG, sliders for noise scale + particle count + trail decay, seed navigator
- Output: `.md` + `.html` + `.js`; same seed reproduces the same flow exactly; user can tweak scale slider live

### Example 2 — Iteration on a published composition
- Input: "I love seed 42 from yesterday but want denser trails"
- Action: Load seed 42, expose trail-density slider with default doubled, keep all other params identical, save new export with new seed for sharing
- Output: HTML loads seed 42 by default with the new trail-density baseline; previous compositions still reproducible by re-entering their seeds
