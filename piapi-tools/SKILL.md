---
name: piapi-tools
description: 'Multi-model AI media generation hub for images, videos, music, and 3D models using Midjourney, Flux, Kling, Luma Dream Machine, Suno, Udio, Hu. Triggers: "use piapi-tools", "piapi tools", "piapi task.'
allowed-tools: Glob, Grep, Read
---

# PiAPI Tools

Multi-model AI media generation — images, videos, music, 3D models — via PiAPI's unified API.

## Supported Models

| Category | Models |
|----------|--------|
| **Image** | Midjourney (imagine), Flux |
| **Video** | Kling, Luma Dream Machine, Hunyuan, Wan, Skyreels |
| **Music** | Suno, TTS Zero-Shot |
| **3D** | Trellis (image to 3D model) |
| **Audio** | MMAudio (video to music) |

## Available Tool Categories

### Image Generation
- Flux image generation from text/image prompt
- Midjourney imagine command
- Base image toolkit

### Video Generation
- Kling video and effects generation
- Luma Dream Machine video generation
- Hunyuan video from text/image
- Wan video from text/image
- Skyreels video from image

### Music & Audio
- Suno music generation
- MMAudio music from video
- TTS Zero-Shot voice generation

### 3D
- Trellis 3D model generation from image

## Common Workflows

### Generate a Midjourney Image
Use the imagine tool with a Midjourney-style prompt.

### Generate a Video with Kling
Submit a text or image prompt to Kling for high-quality video generation.

### Generate Music for a Video
1. Generate a video with any video model → verify: output file exists + no syntax error
2. Use MMAudio to generate matching background music → verify: output file exists + no syntax error

### Create a 3D Model
Use Trellis to convert a 2D image into a 3D model.

## Tips

- Video generation may time out in Claude due to long processing times — use async submission patterns when available
- Requires a PiAPI API key from [piapi.ai](https://piapi.ai/workspace/key)
- PiAPI is a unified gateway — one API key gives access to all supported models
- For dedicated Runway or Pika generation, use their specific skills instead
- Flux is best for fast, high-quality image generation
- Kling excels at consistent character motion in video
- Luma Dream Machine is strong for cinematic text-to-video

## When NOT to use

- Task doesn't involve calling PiAPI services for media generation → use the matching domain skill instead
- Simple one-off operation that doesn't need this skill's structure
- Different toolchain required → check `find-skills` skill for alternatives
- User explicitly asks to skip skill discipline → respect the override

## Red Flags

| Thought | Reality |
|---------|---------|
| "I'll skip the verify step, output looks right" | Eyeballing without verification ships broken outputs |
| "Generic answer is good enough" | PiAPI media tooling needs domain-specific decisions, not boilerplate |
| "I can hold all context in head" | Multi-step state loss creates regressions you won't catch |
| "Just one more shortcut" | Shortcuts compound — finish discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced and matches user's stated goal
- All verification steps in process passed
- Edge cases for calling PiAPI services for media generation addressed or explicitly noted
- Output is reproducible (no hidden state)
- Hand-off summary provided so user can validate without re-reading entire flow

## Examples

### Example 1 — golden path
- Input: standard request involving calling PiAPI services for media generation
- Action: follow the documented numbered process, apply verify clauses per step
- Output: deliverable that passes the Output Contract

### Example 2 — edge case
- Input: request with non-standard constraint or partial info
- Action: detect the gap, ask clarifying question OR document assumption, proceed with adapted process
- Output: deliverable + explicit note on assumption/limitation
