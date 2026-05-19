---
name: heygen-tools
description: 'HeyGen AI avatar video generation via MCP for creating spokesperson videos, digital presenters, and video dubbing/translation. Triggers: "use heygen-tools", "heygen tools", "heygen task".'
allowed-tools: Glob, Grep, Read
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
1. Browse avatars: → verify: step output matches expected outcome
```
get_avatar_groups()
get_avatars_in_avatar_group(group_id="...")
```
2. Pick a voice: → verify: step output matches expected outcome
```
get_voices()
```
3. Generate: → verify: output file exists + no syntax error
```
generate_avatar_video(
  avatar_id="...",
  text="Welcome to our wedding! We're so excited to celebrate with you.",
  voice_id="..."
)
```
4. Poll status: → verify: step output matches expected outcome
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

## When NOT to use

- Task is unrelated to heygen tools — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Heygen Tools needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for heygen tools
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving heygen tools
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
