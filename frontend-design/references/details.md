# Extended Details

## Purpose
Create beautiful, production-grade frontend interfaces that look distinctive — not like generic AI-generated output. Every component should have intentional visual identity, clear hierarchy, and thoughtful interaction design.

## Design Philosophy

### Avoid Generic AI Aesthetics
The hallmarks of generic AI-generated UI:
- Blue gradient hero sections with white text
- Generic sans-serif fonts (Inter everywhere, no variation)
- Gray cards on white backgrounds with generic icons
- Centered layouts with no visual tension
- Default Tailwind color classes with no customization

Instead: Create visual interest through contrast, tension, unexpected color combinations, strong typographic hierarchy, and purposeful whitespace.

### Quality Markers
Every component should have at least 3 of these:
- Strong typographic hierarchy (2+ font sizes, weight variation)
- Intentional color palette (custom, not default)
- Micro-interactions (hover states, transitions, transforms)
- Distinctive spacing (not uniform padding everywhere)
- Visual texture or depth (shadows, borders, gradients, backdrop blur)
- Negative space used deliberately (breathing room)

---

## Stack Decision Guide

| Scenario | Stack | Why |
|---|---|---|
| Quick prototype / static page | HTML + Tailwind CDN | Zero setup, fast iteration |
| React project (no Next.js) | React + Tailwind | Component reuse, no SSR needed |
| Next.js project | Next.js App Router + Tailwind | SSR, image optimization, routing |
| Design system / component library | React + CSS Modules | Encapsulation, no Tailwind conflicts |
| Animation-heavy | React + Framer Motion | Declarative animations |
| 3D / canvas / generative | p5.js, Three.js, or canvas API | Native performance |

---

## Component Patterns

### Hero Section (Non-Generic)
```html
<section style="
  background: #0a0a0a;
  min-height: 100vh;
  display: grid;
  place-items: center;
  padding: 6rem 2rem;
  position: relative;
  overflow: hidden;
">
  <div style="position: absolute; left: 0; top: 0; bottom: 0; width: 4px;
    background: linear-gradient(to bottom, #ff6b35, transparent);"></div>

  <div style="max-width: 900px; width: 100%;">
    <span style="display: inline-block; font-size: 0.75rem; letter-spacing: 0.2em;
      text-transform: uppercase; color: #ff6b35; margin-bottom: 1.5rem;">
      AI AUTOMATION AGENCY
    </span>

    <h1 style="font-size: clamp(3rem, 8vw, 7rem); font-weight: 900;
      line-height: 0.95; color: #ffffff; margin-bottom: 2rem;">
      We build<br>
      <span style="-webkit-text-stroke: 2px #ffffff; color: transparent;">autonomous</span><br>
      empires.
    </h1>

    <p style="font-size: 1.25rem; color: rgba(255,255,255,0.6);
      max-width: 500px; line-height: 1.6;">
      16 AI agents running your media, agency, and affiliate business.
    </p>
  </div>
</section>
```

### Metric Card (React/Tailwind)
```tsx
export function MetricCard({ title, description, metric, accent = '#3b5bdb' }) {
  return (
    <div className="group relative p-6 bg-white border border-gray-100 rounded-2xl
                    hover:border-transparent hover:shadow-xl transition-all duration-300">
      <div className="absolute inset-x-0 top-0 h-0.5 opacity-0 group-hover:opacity-100 transition-opacity"
        style={{ background: `linear-gradient(to right, ${accent}, transparent)` }} />

      {metric && (
        <div className="mb-4">
          <span className="text-4xl font-black" style={{ color: accent }}>{metric}</span>
        </div>
      )}
      <h3 className="font-semibold text-gray-900 mb-2">{title}</h3>
      <p className="text-sm text-gray-500 leading-relaxed">{description}</p>
    </div>
  );
}
```

---

## Typography Scale

```css
.display   { font-size: clamp(3rem, 8vw, 8rem); font-weight: 900; line-height: 0.9; letter-spacing: -0.03em; }
.heading-1 { font-size: clamp(2rem, 5vw, 4rem); font-weight: 800; line-height: 1.1; letter-spacing: -0.02em; }
.heading-2 { font-size: clamp(1.25rem, 3vw, 2rem); font-weight: 700; line-height: 1.2; }
.body      { font-size: 1rem; font-weight: 400; line-height: 1.6; }
.caption   { font-size: 0.75rem; font-weight: 500; letter-spacing: 0.1em; text-transform: uppercase; }
```

---

## Color Palette Approach

```css
:root {
  --color-primary:   #1a1a2e;
  --color-accent:    #e94560;
  --color-highlight: #f5a623;

  --color-bg:             #0f0f1a;
  --color-surface:        #1a1a2e;
  --color-surface-raised: #252540;

  --color-text-primary:   #ffffff;
  --color-text-secondary: rgba(255, 255, 255, 0.6);
  --color-text-muted:     rgba(255, 255, 255, 0.3);
}
```

---

## Animation Principles

```css
/* Hover lift */
.card { transition: transform 0.2s ease, box-shadow 0.2s ease; }
.card:hover { transform: translateY(-4px); box-shadow: 0 20px 40px rgba(0,0,0,0.15); }

/* Stagger */
.list-item:nth-child(1) { animation-delay: 0ms; }
.list-item:nth-child(2) { animation-delay: 100ms; }
.list-item:nth-child(3) { animation-delay: 200ms; }

/* Accessible focus */
*:focus-visible { outline: 2px solid var(--color-accent); outline-offset: 3px; border-radius: 4px; }
```

---

## Responsive Design

```css
/* Fluid typography — no breakpoints needed */
h1 { font-size: clamp(2rem, 5vw + 1rem, 6rem); }
p  { font-size: clamp(1rem, 1.5vw + 0.5rem, 1.25rem); }

/* Auto-fit grid */
.grid-layout {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.5rem;
}
```

---

## Quality Checklist

- [ ] Hover states on all interactive elements
- [ ] Focus states visible (keyboard navigation works)
- [ ] Color contrast ratio >= 4.5:1 for body text
- [ ] No hardcoded pixel values for font sizes (use rem/clamp)
- [ ] Loading states for async operations
- [ ] Mobile layout tested (< 375px viewport)
- [ ] Images have alt text
