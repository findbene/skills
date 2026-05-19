# Extended Details

## Quick Reference

| Command | What it does |
|---------|-------------|
| `/ads generate` | Generate all images from campaign-brief.md |
| `/ads generate --platform meta` | Generate Meta assets only |
| `/ads generate --prompt "text" --ratio 9:16` | Standalone generation without brief |

## Environment Setup

**Required before running:**

- Requires banana-claude (v1.4.1+) with nanobanana-mcp configured
- Run `/banana setup` to configure API key and MCP
- Fallback: if banana is not available, use `scripts/generate_image.py` (deprecated)

If banana-claude is not installed, this skill will display setup instructions
and stop. It will never fail silently.

If banana-claude is unavailable, alternatives include: OpenAI gpt-image-1 ($0.040/image), Stability SD 3.5 ($0.065), or Replicate FLUX.1 Pro ($0.055). Configure via ADS_IMAGE_PROVIDER env var.

## Cost Transparency

Before generating, estimate and show the cost:
- Count the number of image briefs in campaign-brief.md
- Show estimated cost based on banana pricing tiers
- If >$1.00, ask for confirmation before proceeding

## Standalone Mode (No campaign-brief.md)

When running without a campaign brief:

```
Platform target → dimensions used:
  meta-feed     → 1080×1350 (4:5)
  meta-reels    → 1080×1920 (9:16)
  tiktok        → 1080×1920 (9:16)
  google-pmax   → 1200×628 (1.91:1)
  linkedin      → 1080×1080 (1:1)
  youtube       → 1280×720 (16:9)
  youtube-short → 1080×1920 (9:16)
```

Use `/banana generate` directly with the specified prompt and aspect ratio.
