---
name: ui-to-code
description: 'UI design to working code conversion for turning screenshots, mockups, Figma designs, or image references into functional components. Triggers: "use ui-to-code", "build UI to code", "ui to code".'
allowed-tools: Glob, Grep, Read
---

# UI to Code

Convert visual designs into production-ready code.

## Process

### 1. Analyze the Design
- Identify layout structure (grid, flexbox)
- Extract color palette
- Note typography (fonts, sizes, weights)
- Map interactive elements
- Identify responsive breakpoints

### 2. Structure the HTML
- Semantic HTML elements
- Logical component hierarchy
- Accessibility attributes (ARIA)
- Data attributes for interactivity

### 3. Implement Styling
- CSS custom properties for design tokens
- Mobile-first responsive design
- Consistent spacing scale
- Typography system

### 4. Add Interactivity
- Hover/focus states
- Animations and transitions
- JavaScript functionality

## Implementation Guidelines

### Layout Analysis
```css
/* Grid for complex layouts */
.container {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: 1.5rem;
}

/* Flexbox for alignment */
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
```

### Design Tokens
```css
:root {
  --color-primary: #3b82f6;
  --color-text: #1e293b;
  --font-sans: 'Inter', sans-serif;
  --space-4: 1rem;
}
```

### Component Structure (React)
```jsx
const Card = ({ image, title, description, actions }) => (
  <article className="card">
    <img src={image} alt="" className="card-image" />
    <div className="card-content">
      <h3 className="card-title">{title}</h3>
      <p className="card-description">{description}</p>
      <div className="card-actions">{actions}</div>
    </div>
  </article>
);
```

## Quality Checklist

- Matches design visually (spacing, colors, typography)
- Responsive across breakpoints
- Accessible (keyboard nav, screen readers)
- Semantic HTML structure
- Optimized images
- Smooth animations (60fps)
- Cross-browser compatible

## When NOT to use

- Stitch-specific design → code conversion — use `stitch-*-components` skills
- Figma file conversion specifically — use `figma-tools` first to extract, then this
- Mobile-native code (SwiftUI/RN) — use `stitch-swiftui-components` / `stitch-react-native-components`
- Pure design review without code output — use `web-design-guidelines` or `audit`
- Greenfield UI generation without an image input — use `frontend-design` or `impeccable`

## Red Flags

| Rationalization | Reality |
|---|---|
| "Eyeball the colors, near enough" | Use a color picker / dropper; off-by-a-shade kills design fidelity |
| "Skip responsive, just match the desktop mockup" | Mobile-first is the default; without breakpoints, the build looks broken on phones |
| "Tabs and indices instead of semantic HTML" | Use semantic tags — header/main/section/article/aside — for SEO and a11y |
| "Pixel-perfect or nothing" | Reproduce intent and proportions, not exact pixels; tokens > pixels |

## Output Contract

Finished output must contain:
- Semantic HTML structure with appropriate landmarks
- CSS custom properties for colors, spacing, typography
- Responsive design with at least mobile (375px) + desktop (1280px) layouts
- All interactive elements have hover/focus/active states
- Accessibility: alt text on images, ARIA on icon buttons, label-input associations
- One-line note for each visual decision the design left ambiguous
- Performance: optimized image formats, no layout-thrashing animations

## Examples

**Example 1 — Convert screenshot to landing page**
- Input: User uploads `landing.png`, wants HTML/CSS
- Action: Identify grid (3-col hero) → extract palette (4 colors) → identify typography (display serif + body sans) → write semantic HTML + tokens + responsive CSS → add hover states
- Output: `index.html` + `style.css` with custom properties, mobile + desktop layouts, hover states, semantic markup

**Example 2 — Figma frame to React component**
- Input: Figma export of a `ProductCard`
- Action: Map layout to flexbox → extract spacing/colors as Tailwind theme tokens → produce `ProductCard.tsx` with props for image/title/price/cta
- Output: TS React component, typed props, responsive grid behavior verified, hover/focus states
