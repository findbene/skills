---
name: frontend-design-system
description: "Scalable frontend design system creation for consistent UI components, typography, spacing scales, and token-based theming. Use this skill any time a design system for a frontend application needs to be built, component libraries need to be established, or design consistency needs to be enforced through a shared token system. Trigger immediately on: \"frontend design system\", \"component library\", \"design tokens\", \"UI system\", \"theme\", \"Tailwind config\", \"CSS variables\", \"shadcn setup\", \"component primitives\", \"UI consistency\", \"token system\", \"spacing scale\", \"color tokens\", \"typography system\". If someone says \"set up a design system\" or \"create a component library\" this skill MUST trigger."
---

# Frontend Design System

Build cohesive design systems that ensure visual consistency, improve development velocity, and create memorable user experiences.

## Design Philosophy

1. **Intentional Design** - Every choice serves a purpose
2. **Systematic Thinking** - Components fit together logically
3. **Progressive Complexity** - Simple base, powerful composition
4. **Accessibility First** - Inclusive by default
5. **Performance Conscious** - Fast and efficient

### Avoiding Generic AI Aesthetics

**DO:** Choose characterful fonts, commit to bold color palettes, add texture and depth, create asymmetric layouts, use intentional animations.

**DON'T:** Default to Inter/Arial/Roboto, use only grayscale with one accent, create flat sterile interfaces, rely on centered symmetric layouts.

## Design Tokens

### Token Architecture

```typescript
// tokens/colors.ts
export const colors = {
  brand: {
    primary: { 50: '#f0f7ff', 500: '#0a7bff', 900: '#003375' },
    secondary: { /* similar scale */ }
  },
  semantic: { success: '#10b981', warning: '#f59e0b', error: '#ef4444', info: '#3b82f6' },
  neutral: { 0: '#ffffff', 50: '#fafafa', 500: '#737373', 900: '#171717', 1000: '#000000' }
};

// tokens/typography.ts
export const typography = {
  fontFamily: {
    display: '"Clash Display", "Poppins", system-ui, sans-serif',
    body: '"Cabinet Grotesk", "Inter", system-ui, sans-serif',
    mono: '"JetBrains Mono", "Fira Code", monospace',
  },
  fontSize: { xs: '0.75rem', sm: '0.875rem', base: '1rem', lg: '1.125rem', xl: '1.25rem', '2xl': '1.5rem', '3xl': '1.875rem', '4xl': '2.25rem', '5xl': '3rem' },
  fontWeight: { normal: '400', medium: '500', semibold: '600', bold: '700' },
};
```

### CSS Variables

```css
:root {
  --color-primary: #0a7bff;
  --color-background: #ffffff;
  --color-text: #171717;
  --font-display: "Clash Display", system-ui, sans-serif;
  --font-body: "Cabinet Grotesk", system-ui, sans-serif;
  --transition-fast: 150ms ease;
  --transition-base: 200ms ease;
}

[data-theme="dark"] {
  --color-background: #0a0a0a;
  --color-text: #fafafa;
}
```

## Component Architecture

### Base Components with CVA

```tsx
import { cva, type VariantProps } from 'class-variance-authority';

const buttonVariants = cva(
  'inline-flex items-center justify-center font-medium transition-colors focus-visible:outline-none focus-visible:ring-2 disabled:pointer-events-none disabled:opacity-50',
  {
    variants: {
      variant: {
        primary: 'bg-primary text-white hover:bg-primary-600',
        outline: 'border border-input bg-background hover:bg-accent',
        ghost: 'hover:bg-accent hover:text-accent-foreground',
      },
      size: {
        sm: 'h-8 px-3 text-sm rounded-md',
        md: 'h-10 px-4 text-base rounded-lg',
        lg: 'h-12 px-6 text-lg rounded-lg',
      },
    },
    defaultVariants: { variant: 'primary', size: 'md' },
  }
);
```

### Compound Components Pattern

```tsx
function Card({ children, className }: CardProps) {
  return <div className={cn('rounded-xl border bg-card p-6 shadow-sm', className)}>{children}</div>;
}
Card.Header = ({ children, className }) => <div className={cn('flex flex-col space-y-1.5 pb-4', className)}>{children}</div>;
Card.Title = ({ children, className }) => <h3 className={cn('text-2xl font-semibold', className)}>{children}</h3>;
Card.Content = ({ children, className }) => <div className={cn('pt-0', className)}>{children}</div>;
Card.Footer = ({ children, className }) => <div className={cn('flex items-center pt-4', className)}>{children}</div>;
```

## Animation System

```css
@keyframes slideUp {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

.stagger-children > * {
  animation: slideUp 0.4s ease-out forwards;
  opacity: 0;
}
.stagger-children > *:nth-child(1) { animation-delay: 0ms; }
.stagger-children > *:nth-child(2) { animation-delay: 50ms; }
.stagger-children > *:nth-child(3) { animation-delay: 100ms; }
```

## Responsive Design

```typescript
const breakpoints = { sm: '640px', md: '768px', lg: '1024px', xl: '1280px', '2xl': '1536px' };
```

Use container queries for component-level responsive behavior:
```css
.card-container { container-type: inline-size; }
@container (min-width: 400px) { .card-content { display: grid; grid-template-columns: 1fr 2fr; } }
```

## Accessibility

- Focus trap for modals with `role="dialog"` and `aria-modal="true"`
- Color contrast: WCAG AA (4.5:1 for text, 3:1 for large text)
- Keyboard navigation support throughout

## Best Practices

1. Document everything — component API, usage examples
2. Version tokens — track breaking changes
3. Test accessibility — automated and manual
4. Performance audit — bundle size, render time
5. Consistent naming — clear, predictable conventions
6. Composable patterns — flexible building blocks
7. Theme support — light/dark modes at minimum
