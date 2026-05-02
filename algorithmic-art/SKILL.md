---
name: algorithmic-art
description: "Algorithmic and generative art creation using p5.js with seeded randomness, particle systems, flow fields, and noise-based visuals. Use this skill any time creative coding and generative art needs to be produced, p5.js sketches need to be written, or procedural visual patterns need to be generated. Trigger immediately on: \"generative art\", \"p5.js\", \"creative coding\", \"procedural art\", \"particle system\", \"flow field\", \"noise algorithm\", \"algorithmic design\", \"Perlin noise\", \"random art\", \"code art\", \"sketch\", \"visual algorithm\", \"generative design\". If someone says \"create a generative art piece\" or \"make something with p5.js\" this skill MUST trigger."
---

# Algorithmic Art

Create generative art through code, expressing computational aesthetics with reproducible, interactive results.

## Two-Step Process

1. **Algorithmic Philosophy Creation** (.md file)
2. **Express through p5.js** (.html + .js files)

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

1. **Parameter Controls**: Sliders, color pickers, real-time updates
2. **Seed Navigation**: Previous/Next, Random, Jump to seed
3. **Self-Contained HTML**: Everything inline, p5.js from CDN

## Craftsmanship

- **Balance**: Complexity without visual noise — restraint is what separates generative art from random noise
- **Color Harmony**: Thoughtful palettes, not random RGB values
- **Composition**: Visual hierarchy even in randomness
- **Performance**: Smooth execution at interactive framerates
- **Reproducibility**: Same seed produces identical output every time
