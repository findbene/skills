---
name: elevenlabs-tools
description: "ElevenLabs AI voice generation via MCP for text-to-speech, voice cloning, audio transcription, and sound effects. Use this skill any time AI voiceover needs to be generated, voices need to be cloned, text needs to be converted to speech, or audio transcription is needed using ElevenLabs. Trigger immediately on: \"ElevenLabs\", \"voice generation\", \"text to speech\", \"TTS\", \"voiceover\", \"AI voice\", \"voice clone\", \"generate audio\", \"speech synthesis\", \"voice ID\", \"ElevenLabs API\", \"audio generation\", \"narration\", \"elevenlabs\". If someone says \"generate a voiceover\" or \"convert this to speech\" this skill MUST trigger."
---

# ElevenLabs Tools

Generate speech, clone voices, transcribe audio, create sound effects, compose music, and build AI voice agents.

**Important:** ElevenLabs credits are consumed by most tools. Free tier includes 10k credits/month. Tools marked with $ incur costs.

## Available Tools

### Text-to-Speech & Voice
| Tool | Cost | Purpose |
|------|------|---------|
| `text_to_speech` | $ | Convert text to speech with a chosen voice |
| `speech_to_speech` | $ | Transform audio from one voice to another |
| `text_to_voice` | $ | Generate voice previews from a text description |
| `create_voice_from_preview` | Free | Save a generated voice preview to your library |
| `voice_clone` | $ | Create an instant voice clone from audio files |

### Voice Management
| Tool | Cost | Purpose |
|------|------|---------|
| `search_voices` | Free | Search voices in your personal library |
| `search_voice_library` | Free | Search the entire ElevenLabs voice library |
| `get_voice` | Free | Get details of a specific voice |
| `list_models` | Free | List all available TTS models |

### Audio Processing
| Tool | Cost | Purpose |
|------|------|---------|
| `speech_to_text` | $ | Transcribe speech from an audio file |
| `isolate_audio` | $ | Isolate vocals/audio from background noise |
| `text_to_sound_effects` | $ | Generate sound effects from text description |
| `play_audio` | Free | Play a WAV or MP3 file |

### Music
| Tool | Cost | Purpose |
|------|------|---------|
| `compose_music` | $ | Generate music from a text prompt |
| `create_composition_plan` | Free | Plan a music composition before generating |

### Conversational AI Agents
| Tool | Cost | Purpose |
|------|------|---------|
| `create_agent` | $ | Create a conversational AI voice agent |
| `list_agents` | Free | List all your AI agents |
| `get_agent` | Free | Get details of a specific agent |
| `make_outbound_call` | $ | Make a phone call using an AI agent |
| `list_phone_numbers` | Free | List phone numbers on the account |
| `list_conversations` | Free | List conversations with filtering |
| `get_conversation` | Free | Get conversation details with transcript |
| `add_knowledge_base_to_agent` | Free | Add a knowledge base to an agent |

### Account
| Tool | Cost | Purpose |
|------|------|---------|
| `check_subscription` | Free | Check current plan and credit balance |

## Common Workflows

### Generate Narration
```
text_to_speech(
  text="Welcome to our wedding celebration...",
  voice_name="Rachel",
  output_file="welcome_narration.mp3"
)
```

### Clone a Voice
1. `voice_clone(name="My Voice", files=["sample1.mp3", "sample2.mp3"])` — provide 1+ audio samples
2. `text_to_speech(text="Hello world", voice_id="<cloned_voice_id>")` — use the cloned voice

### Design a New Voice
1. `text_to_voice(text="Sample text to preview", description="warm, friendly, female, American")` — generates previews
2. `create_voice_from_preview(voice_id="<preview_id>", name="Warm Narrator")` — save to library

### Transcribe Audio
```
speech_to_text(file_path="meeting_recording.mp3")
```
Returns text with speaker identification.

### Create Sound Effects
```
text_to_sound_effects(
  text="gentle rain falling on a tin roof with distant thunder",
  output_file="rain_ambience.mp3"
)
```

### Compose Music
1. `create_composition_plan(prompt="upbeat jazz for a wedding reception")` — plan first
2. `compose_music(prompt="upbeat jazz for a wedding reception", output_file="reception_music.mp3")`

### Build a Voice Agent
1. `create_agent(name="Wedding Concierge", first_message="Hello! How can I help with wedding details?", system_prompt="You are a helpful wedding concierge...")`
2. `add_knowledge_base_to_agent(agent_id="...", knowledge_base_url="https://...")` — add context
3. `make_outbound_call(agent_id="...", phone_number="+1234567890")` — call a guest

## Output Configuration

Files save to `~/Desktop` by default. Configure via env vars in `~/.claude/settings.json`:

- `ELEVENLABS_MCP_BASE_PATH` — change output directory
- `ELEVENLABS_MCP_OUTPUT_MODE` — `files` (default), `resources`, or `both`

## Setup

1. Get a free API key at https://elevenlabs.io/app/settings/api-keys
2. Replace `<insert-your-api-key-here>` in `~/.claude/settings.json` under the `elevenlabs` MCP config
3. Restart Claude Code

## Tips

- Use `check_subscription` to monitor credit usage before expensive operations
- `search_voice_library` finds professional voices; `search_voices` searches only your personal library
- For long text, `text_to_speech` automatically handles chunking
- Voice cloning works best with 1-3 minutes of clean audio samples
- `speech_to_speech` preserves intonation while changing the voice
