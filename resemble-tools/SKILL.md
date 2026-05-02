---
name: resemble-tools
description: "Resemble AI voice cloning and custom speech synthesis via documentation, API specs, and code generation helpers. Use this skill any time Resemble AI needs to be used for voice cloning, custom voice synthesis with Resemble is needed, or Resemble API integration needs to be built. Trigger immediately on: \"Resemble AI\", \"Resemble\", \"voice clone Resemble\", \"Resemble voice\", \"custom voice\", \"Resemble API\", \"voice synthesis Resemble\", \"Resemble TTS\", \"clone my voice Resemble\", \"Resemble integration\". If someone says \"use Resemble to clone this voice\" or \"generate audio with Resemble\" this skill MUST trigger."
---

# Resemble AI Tools

Access Resemble AI documentation, API schemas, and code generation helpers for voice AI.

## Available Tools

| Tool | Purpose |
|------|---------|
| `resemble_docs_lookup` | Get aggregated docs for a topic |
| `resemble_search` | Full-text search across all docs |
| `resemble_get_page` | Retrieve a specific doc page by path |
| `resemble_list_topics` | List all available topic IDs |
| `resemble_api_endpoint` | Get OpenAPI spec for a specific endpoint |
| `resemble_api_search` | Search API endpoints by query |

## Topics

`voice-cloning`, `text-to-speech`, `speech-to-speech`, `speech-to-text`, `getting-started`, `voice-design`, `streaming`, `detect`, `agents`, `sdks`, `projects-clips`, `audio-tools`, `ssml`

## Common Workflows

### Build a TTS Feature
```
resemble_docs_lookup(topic="text-to-speech")
resemble_api_endpoint(method="POST", path="/synthesize")
```

### Voice Cloning Integration
```
resemble_docs_lookup(topic="voice-cloning")
resemble_api_search(query="voice")
```

### Streaming Audio
```
resemble_docs_lookup(topic="streaming")
resemble_search(query="websocket streaming", limit=5)
```

### Get Exact API Schema
```
resemble_api_endpoint(method="POST", path="/synthesize")
```
Returns full OpenAPI spec with request/response schemas for accurate code generation.

## Tips

- This is a **documentation server** — it provides API docs and schemas, not direct API calls
- Use `resemble_api_endpoint` to get exact request/response schemas for code generation
- Use `resemble_docs_lookup` with topic IDs for guaranteed comprehensive results
- The server includes the full OpenAPI 3.0 spec for all Resemble AI endpoints
- Combine with code generation to build complete voice AI features
