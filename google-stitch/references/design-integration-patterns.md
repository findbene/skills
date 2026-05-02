# Stitch Design Integration Patterns

## Pattern 1: Design-to-Theme Pipeline

Convert Stitch-generated screens into themed components for any framework.

### Steps:
1. Generate reference screen with brand colors/fonts in the prompt
2. Extract design context to get the Design DNA
3. Generate all remaining screens with that context
4. Extract code from each screen
5. Parse into reusable components

### Design DNA Structure (from extract_design_context):
```
Typography:
  - Primary Font: [family, weights]
  - Secondary Font: [family, weights]
  - Scale: [h1-h6 sizes, body, caption]

Colors:
  - Primary: [hex]
  - Secondary: [hex]
  - Accent: [hex]
  - Background: [hex]
  - Surface: [hex]
  - Text: [primary, secondary, muted]
  - Border: [hex]

Spacing:
  - Base unit: [px]
  - Scale: [xs, sm, md, lg, xl, 2xl]

Layout:
  - Max width: [px]
  - Grid: [columns, gap]
  - Breakpoints: [sm, md, lg, xl]

Effects:
  - Border radius: [values]
  - Shadows: [values]
  - Transitions: [values]
```

## Pattern 2: Iterative Refinement

1. Generate initial screen with broad prompt
2. Preview with `get_screen_image`
3. Extract design context
4. Regenerate with refined prompt + extracted context
5. Repeat until satisfied
6. Extract final code

## Pattern 3: Component Extraction

From a full-page Stitch output, extract reusable components:

1. Get full page HTML from `get_screen_code`
2. Identify repeated patterns (cards, nav, footer, forms)
3. Extract each into a standalone component file
4. Replace inline styles with CSS classes / Tailwind utilities
5. Parameterize dynamic content (props/slots)

## Pattern 4: Design System Bootstrap

Use Stitch to kickstart a design system:

1. Generate 3-5 representative screens (landing, dashboard, form, detail, error)
2. Extract design context from each
3. Find common patterns across all extractions
4. Generate CSS custom properties / design tokens
5. Build component library from the common patterns

## Prompt Engineering for Stitch

### Effective Prompts Include:
- **What**: The type of page/screen (landing page, dashboard, settings)
- **Who**: Target audience (developers, enterprise, consumers)
- **Style**: Visual direction (minimal, bold, corporate, playful)
- **Content**: Key sections and content types
- **Constraints**: Max width, mobile-first, dark mode, accessibility

### Example Prompts:
```
"A minimal SaaS pricing page with 3 tiers, annual/monthly toggle,
feature comparison table, FAQ section. Use Inter font, blue primary
color (#2563EB), white background. Mobile responsive."

"An e-commerce product detail page for tech gadgets. Large hero image,
product specs grid, customer reviews section, related products carousel.
Modern, clean design with subtle shadows."

"A dashboard for monitoring IoT devices. Sidebar navigation, top stats
cards, real-time chart area, device list table, alert notifications.
Dark theme, data-dense layout."
```
