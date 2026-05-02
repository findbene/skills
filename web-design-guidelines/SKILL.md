---
name: web-design-guidelines
description: "Web interface design compliance review against 10 categories of best practices for accessibility, performance, responsiveness, and standards. Use this skill any time UI code needs to be reviewed against web design standards, accessibility compliance needs to be checked, or web interface best practices need to be enforced. Trigger immediately on: \"web design review\", \"design guidelines\", \"accessibility check\", \"WCAG\", \"design compliance\", \"web standards\", \"design best practices\", \"accessibility audit\", \"UI review\", \"design review\", \"check this UI\", \"does this follow best practices\", \"accessibility\", \"design standards\". If someone says \"review this UI for best practices\" or \"check accessibility\" this skill MUST trigger."
---

# Web Design Guidelines (Vercel)

Comprehensive UI/UX review with 100+ rules across key categories.

## Categories

### Accessibility (a11y)
- Semantic HTML elements
- ARIA labels and roles
- Keyboard navigation
- Focus management
- Screen reader compatibility
- Color contrast ratios (4.5:1 minimum)
- Alt text for images

### Forms
- Label associations
- Error message placement
- Input validation feedback
- Autofocus appropriately
- Required field indicators
- Form submission states

### Animation & Motion
- Respect prefers-reduced-motion
- Use transform/opacity (GPU-accelerated)
- Avoid layout-triggering animations
- Consistent timing functions

### Typography
- Readable font sizes (16px+ body)
- Line height 1.5-1.7 for body text
- Maximum line length (65-75 characters)
- Font loading strategy (font-display)
- Responsive typography scale

### Images
- Responsive images (srcset)
- Lazy loading for below-fold
- Proper aspect ratios
- WebP/AVIF formats
- Placeholder/skeleton loading

### Performance
- Critical CSS inlined
- Non-blocking resources
- Optimized web fonts
- Image optimization
- Code splitting

### Navigation
- Clear visual hierarchy
- Breadcrumbs for deep pages
- Mobile-friendly menus
- Skip to content links

### Dark Mode
- CSS custom properties
- prefers-color-scheme support
- Sufficient contrast in both modes
- Smooth theme transitions

### Touch & Mobile
- Touch target size (44x44px minimum)
- Swipe gesture support
- No hover-only interactions
- Viewport meta tag
- Safe area insets

### Internationalization
- RTL layout support
- Flexible text containers
- Date/number formatting
- Translation-ready strings

## Review Checklist

```markdown
## Accessibility
- [ ] All images have alt text
- [ ] Form inputs have labels
- [ ] Color contrast passes WCAG AA
- [ ] Keyboard navigation works
- [ ] Focus indicators visible

## Performance
- [ ] Images are optimized
- [ ] Fonts load efficiently
- [ ] No layout shift (CLS)

## Mobile
- [ ] Touch targets 44px+
- [ ] Responsive layout
- [ ] No horizontal scroll
- [ ] Works without hover
```
