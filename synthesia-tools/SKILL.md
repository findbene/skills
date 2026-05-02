---
name: synthesia-tools
description: "Synthesia AI avatar video creation via MCP for generating corporate videos, training content, and multi-language video dubbing. Use this skill any time Synthesia needs to be used for avatar videos, corporate video production needs AI avatars, or video translation and dubbing with Synthesia is required. Trigger immediately on: \"Synthesia\", \"Synthesia video\", \"AI presenter\", \"corporate video AI\", \"Synthesia avatar\", \"video dubbing Synthesia\", \"Synthesia MCP\", \"generate Synthesia video\", \"Synthesia template\", \"AI training video\", \"talking head Synthesia\". If someone says \"create a video with Synthesia\" or \"use Synthesia for this presentation\" this skill MUST trigger."
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
