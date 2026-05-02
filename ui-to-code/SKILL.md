---
name: ui-to-code
description: "UI design to working code conversion for turning screenshots, mockups, Figma designs, or image references into functional components. Use this skill any time a UI design or screenshot needs to be converted to working code, a Figma design needs to be implemented, or a visual mockup needs to be turned into a component. Trigger immediately on: \"implement this design\", \"convert to code\", \"design to code\", \"screenshot to code\", \"Figma to code\", \"mockup to code\", \"build this UI\", \"implement this mockup\", \"code this design\", \"turn this into code\", \"replicate this design\", \"UI from image\". If someone shares a screenshot or design image and says \"implement this\" this skill MUST trigger."
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
