---
name: resemble-tools
description: 'Resemble AI voice cloning and custom speech synthesis via documentation, API specs, and code generation helpers. Triggers: "use resemble-tools", "resemble tools", "resemble task".'
allowed-tools: Glob, Grep, Read
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

## When NOT to use

- Task is unrelated to resemble tools — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Resemble Tools needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for resemble tools
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving resemble tools
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
