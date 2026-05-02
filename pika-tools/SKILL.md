---
name: pika-tools
description: "Pika Labs AI video generation via fal.ai for creating short video clips, effects, and visual content with AI. Use this skill any time AI videos need to be generated using Pika, short video clips need to be created with AI, or Pika 2.2 video generation capabilities are needed. Trigger immediately on: \"Pika\", \"Pika Labs\", \"Pika video\", \"pika-tools\", \"generate video with Pika\", \"AI video Pika\", \"Pika 2.2\", \"Pika MCP\", \"video generation Pika\", \"fal.ai Pika\", \"Pika clip\". If someone says \"generate a video with Pika\" or \"use Pika to create a clip\" this skill MUST trigger."
---

# Pika Labs Tools

Generate AI videos using Pika 2.2 models via fal.ai — text-to-video, image-to-video, and turbo variants.

## Available Tools

### Generation (synchronous, waits for result)
| Tool | Purpose |
|------|---------|
| `pika_text_to_video` | Generate video from text prompt |
| `pika_image_to_video` | Animate a static image into video |
| `pika_text_to_video_turbo` | Fast text-to-video (lower cost) |
| `pika_image_to_video_turbo` | Fast image-to-video (lower cost) |

### Async Job Management
| Tool | Purpose |
|------|---------|
| `pika_submit` | Submit a job, return immediately with request ID |
| `pika_status` | Check status of a submitted job |
| `pika_result` | Get the result of a completed job |
| `pika_cancel` | Cancel a pending/in-progress job |

## Common Workflows

### Quick Text-to-Video
```
pika_text_to_video(
  prompt="A bride and groom's first dance in a candlelit ballroom, cinematic slow motion",
  aspect_ratio="16:9",
  duration=5,
  resolution="1080p"
)
```

### Animate a Photo
```
pika_image_to_video(
  image_url="https://example.com/photo.jpg",
  prompt="gentle wind blowing through hair, soft camera movement",
  resolution="720p",
  duration=5
)
```

### Fast Turbo Generation
```
pika_text_to_video_turbo(
  prompt="golden confetti falling in slow motion",
  aspect_ratio="16:9",
  resolution="720p"
)
```

### Async for Long Jobs
```
# Submit without waiting
pika_submit(endpoint="text-to-video", params={prompt: "...", aspect_ratio: "16:9"})
# Check later
pika_status(status_url="https://queue.fal.run/...")
# Get result when done
pika_result(response_url="https://queue.fal.run/...")
```

## Parameters

| Parameter | Options | Default |
|-----------|---------|---------|
| `aspect_ratio` | 16:9, 9:16, 1:1, 4:5, 5:4, 3:2, 2:3 | 16:9 |
| `duration` | 5, 10 (turbo: 5 only) | 5 |
| `resolution` | 720p, 1080p | 720p |
| `negative_prompt` | Any text | (empty) |
| `seed` | Integer | (random) |

## Pricing (fal.ai)
- 720p, 5 sec: $0.20
- 1080p, 5 sec: $0.45

## Tips

- Turbo variants are faster and cheaper but only support 5-second duration
- Use `negative_prompt` to avoid unwanted elements
- Set `seed` for reproducible results
- Synchronous tools poll every 5 seconds until completion (30-90 sec typical)
- For batch work, use `pika_submit` + `pika_status` to manage multiple jobs
- Requires a fal.ai API key set as `FAL_KEY` in MCP server config
