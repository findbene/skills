---
name: nano-banana-pro
description: "AI image generation expert using Google DeepMind Gemini 3 Pro Image model for high-quality images up to 4K with text rendering, reference images, and live internet data. Use this skill any time AI images need to be generated, product photos need to be created, social media graphics need to be made, or visual content needs to be produced. Trigger immediately on: \"generate image\", \"create image\", \"AI image\", \"nano-banana\", \"Gemini image\", \"product photo\", \"hero image\", \"generate a picture\", \"make an image\", \"visual asset\", \"image generation\", \"AI art\", \"create a graphic\", \"Gemini 3 Pro Image\". If someone says \"generate an image of X\" or \"create a product photo\" this skill MUST trigger."
---

# Nano Banana Pro — AI Image Generation

Google DeepMind's image generation model (Gemini 3 Pro Image). Generates high-quality images up to 4K with text rendering, reference images, and live internet data.

## CLI Tool: `nano-banana-2`

Installed at `~/tools/nano-banana-2/`. Linked globally via bun.

### Basic Usage
```bash
# Set PATH first (required in Claude Code sessions)
export PATH="$HOME/.bun/bin:$PATH"

# Simple generation
nano-banana "product photo of Amazon Echo Pop on white desk" -o echo-pop-hero

# High quality with Pro model
nano-banana "product photo" --model pro -o output

# 4K resolution
nano-banana "cinematic workspace setup" -s 4K -o workspace-4k

# Custom aspect ratio (Pinterest pin = 2:3)
nano-banana "tech gadget flatlay" -a 2:3 -o pinterest-pin

# Reference image (style transfer)
nano-banana "same style but with blue lighting" -r reference.png -o styled

# Transparent background
nano-banana "product cutout" -t -o cutout
```

### Options

| Option | Values |
|--------|--------|
| Aspect ratios | 1:1, 16:9, 9:16, 4:3, 3:4, 3:2, 2:3, 4:5, 5:4, 21:9 |
| Resolutions | 512px, 1K (default), 2K, 4K |
| Models | `flash` (default, ~$0.045/img), `pro` (highest quality, ~$0.09/img) |

### Cost Tracking
```bash
nano-banana --costs
```

## MCP Server

Also configured as MCP server `nanobanana` in `~/.claude/settings.json`. API key stored as `GOOGLE_AI_API_KEY`.

## Prompting Tips

- Be specific about lighting, background, angle, and style
- Include "product photography" or "editorial style" for professional look
- Use "on white background" or "on dark desk setup" for brand consistency
- Add "no text" to prevent text in the image
- Use reference images (-r) for consistency across multiple generations

## API Key

Stored at `~/.nano-banana/.env` as `GEMINI_API_KEY`. Also in `~/.claude/settings.json` MCP config as `GOOGLE_AI_API_KEY`.
