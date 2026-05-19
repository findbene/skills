---
name: web-design-guidelines
description: 'Web interface design compliance review against 10 categories of best practices for accessibility, performance, responsivenes. Triggers: "use web-design-guidelines", "web design guidelines", "web task.'
allowed-tools: Glob, Grep, Read
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

## When NOT to use

- Strict WCAG 2.2 a11y audit — use `a11y-audit`
- Performance-only audit — use `audit` or `performance-profiler`
- Mobile app native UI review — use `apple-hig-expert` (iOS) / Material design (Android)
- Design taste/aesthetic uplift (not standards compliance) — use `frontend-design` or `redesign-existing-projects`
- Single-component review — overkill; eyeball it

## Red Flags

| Rationalization | Reality |
|---|---|
| "Contrast checker says 3:1, looks fine" | Need 4.5:1 for normal text, 3:1 only for large/non-text — fails WCAG AA at 3:1 |
| "Skip prefers-reduced-motion, who turns that on?" | Vestibular-impaired users must have it; an OS-level setting we are legally expected to honor |
| "Auto-focus a field on page load" | Disorients screen-reader users and traps keyboard; only autofocus modals/dialogs |
| "Touch targets 24x24 work for me" | Mobile minimum is 44x44 (iOS HIG) / 48dp (Android) — anything less fails on real devices |

## Output Contract

Finished output must contain:
- Scored report across all 10 categories (a11y, forms, motion, layout, color, typography, content, navigation, performance, responsiveness)
- Each issue tagged P0/P1/P2/P3
- Specific file:line references where possible
- Recommended fix per issue, not just "improve X"
- Quick-wins ranked at the top
- Re-test checklist after fixes

## Examples

**Example 1 — Review Next.js marketing site**
- Input: "Review our marketing site against best practices"
- Action: Walk 10 categories → flag low-contrast CTA (3.8:1 — P0), missing prefers-reduced-motion (P1), 36px tap targets on mobile (P0), missing focus rings on dark theme (P1) → produce report
- Output: 12-issue report ranked, 4 P0/5 P1/3 P2, file refs, suggested fix per issue

**Example 2 — Pre-launch design QA**
- Input: "We launch tomorrow, do a quick QA pass"
- Action: Focus on blockers — color contrast, keyboard nav, forms, mobile layout → quick scan, skip P3
- Output: 5-blocker report (P0/P1 only), 2-hour fix list, signoff criteria
