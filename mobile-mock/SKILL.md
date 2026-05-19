---
name: "mobile-mock"
description: 'Designs mobile UI mocks and prototypes with correct proportions and touch interactions. Triggers: "use mobile-mock", "mobile mock", "mobile task".'
allowed-tools: Glob, Grep, Read
---


## Mobile Mock Design Standards

### Viewport and Frame
Output **only the screen contents** as if filling a 375×812 viewport (or 390×844 for iPhone 14+). The hosting environment provides the device frame, status bar, and home indicator — DO NOT render them yourself.

Specifically forbidden:
- Phone-shaped wrapper divs (rounded outer container, "bezel", "notch", "dynamic island")
- iOS status bar (9:41 time, signal/wifi/battery icons) — the host adds this
- Home indicator bar at the bottom — the host adds this
- Any explicit phone chrome rendering

Use `body { max-width: 390px; margin: 0 auto; min-height: 100vh; }` to constrain content width. Touch targets and spacing remain mobile-appropriate (44pt min target, 16-20px side padding).

If the user explicitly asks for a "phone mockup with frame" or "show the iPhone bezel", THEN you may render device chrome — otherwise default to bare screen contents.

### Touch Targets
Every interactive element must have a minimum tap target of **44×44px**. This is Apple HIG and Android Material specification. Apply it to buttons, list items, toggle switches, icon buttons, and navigation tabs. Use padding to extend small visual elements to the minimum tap size — do not enlarge the visual.

```css
.touch-target {
  min-height: 44px;
  min-width: 44px;
  display: flex;
  align-items: center;
  justify-content: center;
}
```

### Status Bar
Reserve **44–50px** at the top for the status bar (notch phones: 44px safe area + 20px bar = 59px). Use `padding-top: env(safe-area-inset-top, 44px)` in real implementations. In static mocks, show a simple status bar row with time (e.g. "9:41") on the left and battery/signal icons on the right.

### Safe Area Insets
Bottom: reserve **34px** for the home indicator on notched iPhones. Use `padding-bottom: env(safe-area-inset-bottom, 34px)` on fixed bottom bars. Fixed bottom navigation must never overlap the home indicator.

### Typography Scale
Mobile type scale (base 16px):
- Display: 28–34px, weight 700, tight tracking (−0.02em)
- Headline: 20–24px, weight 600
- Body: 15–16px, weight 400, line-height 1.5
- Caption: 12–13px, weight 400, color muted

Never use type smaller than 12px on mobile — it fails WCAG SC 1.4.4 at normal viewing distance.

### Interaction Patterns
- No hover-only states. Mobile has no hover. Use `:active` for press feedback.
- Swipe gestures should have visual affordances (drag handles, chevrons).
- Bottom sheets and modals should be dismissible by swipe-down or background tap.
- Avoid right-click / long-press as the only way to access features.

### Spacing System
Use an 8px base grid. Standard padding: 16px (screen edges), 12px (card insets), 8px (between related elements), 4px (tight pairs). List item height: 48–56px for single-line, 64–72px for two-line items.

### Navigation
Bottom tab bar: 49–56px tall, 3–5 items max. Top navigation bar: 44–56px tall. Avoid hamburger menus — they hide navigation and reduce discoverability on mobile.

## When NOT to use

- Task is unrelated to mobile mock — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Mobile Mock needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for mobile mock
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving mobile mock
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
