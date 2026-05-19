---
name: synthesia-tools
description: 'Synthesia AI avatar video creation via MCP for generating corporate videos, training content, and multi-language video dubbing. Triggers: "use synthesia-tools", "synthesia tools", "synthesia task".'
allowed-tools: Glob, Grep, Read
---

# Synthesia Tools

Create AI avatar videos, use templates, dub/translate videos — enterprise-grade AI video production.

## Available Tools

### Video Creation
| Tool | Purpose |
|------|---------|
| `synthesia_create_video` | Create a video with avatar, script, and voice |
| `synthesia_create_from_template` | Create a video from a template with variables |
| `synthesia_wait_for_video` | Poll until video completes (returns download URL) |

### Video Management
| Tool | Purpose |
|------|---------|
| `synthesia_get_video` | Get video status and details |
| `synthesia_list_videos` | List all workspace videos |
| `synthesia_delete_video` | Delete a video |

### Templates
| Tool | Purpose |
|------|---------|
| `synthesia_list_templates` | List available templates and variables |
| `synthesia_get_template` | Get template details |

### Translation & Dubbing
| Tool | Purpose |
|------|---------|
| `synthesia_create_dubbing` | Translate a video into other languages |
| `synthesia_get_dubbing` | Check dubbing status and get translated videos |
| `synthesia_list_languages` | List supported translation languages |

## Common Workflows

### Create an Avatar Video
```
synthesia_create_video(
  scriptText="Welcome to Biniyam and Lidia's wedding celebration!",
  avatarId="anna_costume1_cameraA",
  title="Wedding Welcome",
  aspectRatio="16:9",
  test=true
)
# Then poll for completion:
synthesia_wait_for_video(videoId="...")
```

### Create from Template
```
synthesia_list_templates()
synthesia_create_from_template(
  templateId="...",
  templateData={"name": "John", "event": "Wedding Reception"},
  title="Personalized Invite"
)
```

### Translate a Video
```
synthesia_list_languages()
synthesia_create_dubbing(
  videoId="...",
  targetLanguages=["es", "am", "fr"]
)
synthesia_get_dubbing(dubbingId="...")
```

## Parameters

| Parameter | Options | Default |
|-----------|---------|---------|
| `aspectRatio` | 16:9, 9:16, 1:1, 4:5, 5:4 | 16:9 |
| `visibility` | private, public | private |
| `soundtrack` | corporate, inspirational, modern, urban | (none) |
| `test` | true, false | false |

## Tips

- Use `test=true` during development — creates watermarked videos without consuming quota
- Video generation takes 1-5 minutes; use `synthesia_wait_for_video` to auto-poll
- Stock avatar IDs like `anna_costume1_cameraA` work out of the box
- Dubbing preserves lip sync and avatar motion while changing language
- Requires `SYNTHESIA_API_KEY` env var — get from Synthesia dashboard
- Great for creating multilingual wedding welcome videos

## When NOT to use

- Avatar video generation via HeyGen — use `heygen-tools`
- Live-action shot video — use `video-use` or `pika-tools`
- Programmatic React-based video — use `remotion`
- Image generation for thumbnails — use `nano-banana-pro`
- Audio-only voiceover — use `elevenlabs-tools` or `resemble-tools`

## Red Flags

| Rationalization | Reality |
|---|---|
| "Skip waiting for video completion, return immediately" | Video render is async — without `synthesia_wait_for_video` you get nothing usable |
| "Use a personal avatar without consent" | Custom avatars require recorded consent; do not bypass |
| "Hard-code script with `$variables` and skip templates" | Templates are the maintainable pattern for repeatable content; use them |
| "API key in code" | Always in `SYNTHESIA_API_KEY` env var; never commit |

## Output Contract

Finished output must contain:
- Video creation call with avatar, voice, language, script explicit
- Polling step via `synthesia_wait_for_video` until status = complete
- Download URL captured and stored or sent on
- Optional: template ID + variables instead of raw script (more maintainable)
- Error path: handle quota exceeded, invalid avatar, language not supported
- `.env` reference for `SYNTHESIA_API_KEY` (never inlined)

## Examples

**Example 1 — Generate a single corporate welcome video**
- Input: "Create a welcome video for new hires, avatar Anna, 60s English script"
- Action: `synthesia_create_video` with avatar=Anna, voice=English-US, script → `synthesia_wait_for_video` → return download URL
- Output: MP4 download URL, video ID, runtime ~3-8 min

**Example 2 — Dub an existing English training video to 5 languages**
- Input: "Translate this training video to Spanish, French, German, Japanese, Brazilian Portuguese"
- Action: For each target → call dubbing API with source video + target language → poll completion → collect 5 download URLs
- Output: 5 MP4 URLs, original lip-sync preserved, completion report
