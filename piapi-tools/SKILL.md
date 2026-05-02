---
name: piapi-tools
description: "Multi-model AI media generation hub for images, videos, music, and 3D models using Midjourney, Flux, Kling, Luma Dream Machine, Suno, Udio, Hunyuan, Wan, Skyreels, and Trellis via PiAPI. Use this skill any time AI-generated media needs to be created across multiple model providers, Midjourney images are needed, AI music needs to be generated, or 3D models need to be created. Trigger immediately on: \"Midjourney\", \"Flux\", \"Kling\", \"Luma\", \"Suno\", \"Udio\", \"Trellis\", \"Hunyuan\", \"Wan\", \"Skyreels\", \"PiAPI\", \"AI music\", \"3D model AI\", \"Luma Dream Machine\". If someone says \"generate a Midjourney image\" or \"create AI music\" this skill MUST trigger."
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
1. Generate a video with any video model
2. Use MMAudio to generate matching background music

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
