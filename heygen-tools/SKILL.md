---
name: heygen-tools
description: "HeyGen AI avatar video generation via MCP for creating spokesperson videos, digital presenters, and video dubbing/translation. Use this skill any time AI avatar videos need to be created, HeyGen needs to be used for video generation, or video dubbing and translation is needed. Trigger immediately on: \"HeyGen\", \"AI avatar\", \"avatar video\", \"spokesperson video\", \"HeyGen video\", \"AI presenter\", \"talking head video\", \"heygen\", \"video avatar\", \"HeyGen API\", \"digital human\", \"video with avatar\", \"HeyGen MCP\". If someone says \"create an avatar video\" or \"use HeyGen to make a video\" this skill MUST trigger."
---

# HeyGen Tools

Generate AI avatar videos — browse avatars, select voices, and create talking-head videos.

## Available Tools

| Tool | Purpose |
|------|---------|
| `get_remaining_credits` | Check remaining HeyGen credits |
| `get_voices` | List available voices (first 100) |
| `get_avatar_groups` | List avatar groups/categories |
| `get_avatars_in_avatar_group` | List avatars in a specific group |
| `generate_avatar_video` | Create an avatar video with text and voice |
| `get_avatar_video_status` | Check video generation status |

## Common Workflows

### Create an Avatar Video
1. Browse avatars:
```
get_avatar_groups()
get_avatars_in_avatar_group(group_id="...")
```
2. Pick a voice:
```
get_voices()
```
3. Generate:
```
generate_avatar_video(
  avatar_id="...",
  text="Welcome to our wedding! We're so excited to celebrate with you.",
  voice_id="..."
)
```
4. Poll status:
```
get_avatar_video_status(video_id="...")
```

### Check Credits Before Generating
```
get_remaining_credits()
```

## Tips

- Free tier includes 10 credits/month
- Video generation is async — use `get_avatar_video_status` to poll for completion
- Browse avatar groups first, then list avatars within a group
- Requires `HEYGEN_API_KEY` env var in MCP server config
- Great for creating personalized welcome videos or wedding announcement clips
