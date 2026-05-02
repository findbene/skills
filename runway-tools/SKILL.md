---
name: runway-tools
description: "Runway ML Gen-4 AI video generation, image generation, and media upscaling via MCP for professional AI media creation. Use this skill any time Runway ML needs to be used for video or image generation, Gen-4 video needs to be created, or AI media needs to be upscaled. Trigger immediately on: \"Runway\", \"Runway ML\", \"Gen-4\", \"Runway video\", \"generate video Runway\", \"Runway image\", \"AI video generation\", \"Runway Gen-4\", \"Runway MCP\", \"video generation\", \"upscale video\", \"Runway AI\". If someone says \"generate a video with Runway\" or \"use Runway ML for this\" this skill MUST trigger."
---

# Runway ML Tools

Generate AI videos, images, upscale and edit media using Runway's Gen-4 models.

## Available Tools

| Tool | Purpose |
|------|---------|
| `runway_generateImage` | Generate an image from text prompt + optional reference images |
| `runway_generateVideo` | Generate a video from an image + text prompt (Gen-4 Turbo) |
| `runway_upscaleVideo` | Upscale a video to higher resolution |
| `runway_editVideo` | Edit a video with text prompt + optional references (Gen-4 Aleph) |
| `runway_getTask` | Check status of a generation task |
| `runway_cancelTask` | Cancel or delete a task |
| `runway_getOrg` | Get org info, credit balance, usage |

## Common Workflows

### Text-to-Video (two steps)
1. Generate an image first:
```
runway_generateImage(promptText="elegant wedding venue with golden lighting", ratio="1920:1080")
```
2. Then animate it:
```
runway_generateVideo(promptImage="<image_url>", promptText="slow cinematic camera push forward", ratio="1280:720", duration=5)
```

### Image Ratios (generateImage)
`1920:1080`, `1080:1920`, `1024:1024`, `1360:768`, `1080:1080`, `1168:880`, `1440:1080`, `1080:1440`, `1808:768`, `2112:912`, `1280:720`, `720:1280`, `720:720`, `960:720`, `720:960`, `1680:720`

### Video Ratios (generateVideo / editVideo)
`1280:720`, `720:1280`, `1104:832`, `832:1104`, `960:960`, `1584:672`

### Upscale a Video
```
runway_upscaleVideo(videoUri="https://...")
```

### Edit a Video with References
```
runway_editVideo(
  promptText="IMG_1 dancing in a golden ballroom",
  videoUri="https://...",
  ratio="1280:720",
  referenceImages=[{uri: "https://...", tag: "IMG_1"}]
)
```

## Tips

- Always generate an image first, then use it for video generation
- Duration: 5 or 10 seconds only
- Generated image/video URLs expire after 24 hours — download promptly
- Tasks poll automatically until completion (can take 30-120 seconds)
- Use `runway_getOrg` to check remaining credits before large batches
- Reference images use tags (e.g. "IMG_1") that must be referenced in the prompt text
