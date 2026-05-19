# Extended Details

## Quick Reference

| Command | What it does |
|---------|-------------|
| `/ads photoshoot` | Interactive: ask for product + styles |
| `/ads photoshoot --styles studio floating` | Generate only selected styles |
| `/ads photoshoot --product shoe.jpg` | Start with a product image file |
| `/ads photoshoot --all-platforms` | Generate all 5 sizes per style |

## Environment Setup

Requires banana-claude (v1.4.1+) with nanobanana-mcp configured.
Run `/banana setup` to configure API key and MCP.

## Cost Estimate

Before generating, show:
- Number of styles selected x 2 sizes = total images
- Estimated cost based on banana pricing tiers
- If >$0.50, ask for confirmation

## Platform Recommendations

| Style | Best Platforms | Rationale |
|-------|---------------|-----------|
| Studio | Meta Feed, LinkedIn, Google PMax | Universal, clean, platform-safe |
| Floating | Meta Reels, TikTok, Stories | High visual impact on vertical placements |
| Ingredient | Meta Feed, Pinterest | Works best as square; tells product story |
| In Use | TikTok, Meta Reels, Stories | Authentic, native-feeling content |
| Lifestyle | All platforms | Aspirational, broad audience appeal |
