---
name: nano-banana-pro
description: 'AI image generation expert using Google DeepMind Gemini 3 Pro Image model for high-quality images up to 4K with text rendering, referen. Triggers: "use nano-banana-pro", "nano banana pro", "nano task.'
allowed-tools: Bash, Glob, Grep, Read
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

## When NOT to use

- Task doesn't involve generating AI images via Gemini 3 Pro → use the matching domain skill instead
- Simple one-off operation that doesn't need this skill's structure
- Different toolchain required → check `find-skills` skill for alternatives
- User explicitly asks to skip skill discipline → respect the override

## Red Flags

| Thought | Reality |
|---------|---------|
| "I'll skip the verify step, output looks right" | Eyeballing without verification ships broken outputs |
| "Generic answer is good enough" | AI image generation needs domain-specific decisions, not boilerplate |
| "I can hold all context in head" | Multi-step state loss creates regressions you won't catch |
| "Just one more shortcut" | Shortcuts compound — finish discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced and matches user's stated goal
- All verification steps in process passed
- Edge cases for generating AI images via Gemini 3 Pro addressed or explicitly noted
- Output is reproducible (no hidden state)
- Hand-off summary provided so user can validate without re-reading entire flow

## Examples

### Example 1 — golden path
- Input: standard request involving generating AI images via Gemini 3 Pro
- Action: follow the documented numbered process, apply verify clauses per step
- Output: deliverable that passes the Output Contract

### Example 2 — edge case
- Input: request with non-standard constraint or partial info
- Action: detect the gap, ask clarifying question OR document assumption, proceed with adapted process
- Output: deliverable + explicit note on assumption/limitation
