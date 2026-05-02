---
name: nanobanana-tools
description: "NanoBanana AI image generation via Google Gemini image model for creating photos, mockups, illustrations, and AI art through MCP. Use this skill any time an AI image needs to be generated via NanoBanana MCP, product mockups need to be created, or visual content needs to be generated with Gemini. Trigger immediately on: \"NanoBanana\", \"generate image\", \"create a photo\", \"AI image\", \"make a mockup\", \"Gemini image\", \"image generation\", \"create visual\", \"generate artwork\", \"nanobanana\", \"AI art\", \"product mockup\", \"create illustration\", \"generate picture\". If someone says \"generate an image\" or \"make a photo of X\" this skill MUST trigger."
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
