---
name: nanobanana-tools
description: 'NanoBanana AI image generation via Google Gemini image model for creating photos, mockups, illustrations, and AI art through MC. Triggers: "use nanobanana-tools", "nanobanana tools", "nanobanana task.'
allowed-tools: Glob, Grep, Read
---

# NanoBanana Tools

Generate AI images using Google Gemini's image generation model.

## Available Tools

| Tool | Purpose | When to Use |
|------|---------|-------------|
| `generate_image` | Generate an image from a text prompt | Any AI image generation request |

## Usage

```
generate_image(prompt="A minimalist wedding invitation with gold foil text on ivory paper")
```

## Prompt Best Practices

### Be Specific
- Bad: "a website"
- Good: "a modern SaaS landing page with a dark theme, hero section with gradient background, and a centered CTA button"

### Include Style Direction
- Photography: "professional product photo, studio lighting, white background"
- Illustration: "flat vector illustration, pastel colors, minimal style"
- UI mockup: "high-fidelity mobile app screenshot, iOS design language"

### Composition Cues
- "centered composition", "rule of thirds", "overhead shot"
- "close-up", "wide angle", "macro detail"
- "clean background", "bokeh background", "gradient backdrop"

## Common Use Cases

| Use Case | Prompt Pattern |
|----------|---------------|
| Product photo | "Professional product photo of [item], studio lighting, [background]" |
| UI mockup | "High-fidelity [platform] app screenshot showing [feature]" |
| Hero image | "[Style] hero image for [topic], [mood], [dimensions hint]" |
| Icon/Logo | "Minimal [style] icon representing [concept], solid background" |
| Illustration | "[Art style] illustration of [subject], [color palette]" |

## Tips

- Generated images work well as placeholders during development
- For production assets, review and iterate on prompts for best results
- Combine with ui-to-code skill: generate a mockup, then implement it

## When NOT to use

- Task is unrelated to nanobanana tools — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Nanobanana Tools needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for nanobanana tools
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving nanobanana tools
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
