---
name: youtube-tools
description: "YouTube video search, playlist creation and management, and channel data retrieval via MCP. Trigger: \\'YouTube\\', \\'YouTube video\\', \\'search YouTube\\', \\'YouTube playlist\\', \\'YouT."
allowed-tools: Glob, Grep, Read
---

# YouTube Tools

Interact with YouTube via the YouTube Data API v3 through the `youtube` MCP server.

## Available Tools

| Tool | Purpose | Auth Required |
|------|---------|---------------|
| `search_videos` | Search YouTube for videos by query | API key or OAuth |
| `create_playlist` | Create a new playlist (private/public/unlisted) | OAuth |
| `add_to_playlist` | Add a video to an existing playlist | OAuth |
| `list_playlists` | List the authenticated user's playlists | OAuth |
| `delete_playlist` | Delete a playlist by ID | OAuth |

## Common Workflows

### Search for Videos
Call `search_videos` with a query string and optional `maxResults` (1-50, default 10). Returns video IDs, titles, descriptions, channel names, publish dates, and thumbnail URLs.

**Example:**
```
search_videos({ query: "wedding first dance songs 2026", maxResults: 20 })
```

### Create and Populate a Playlist
1. Call `create_playlist` with title, description, and privacy setting → verify: output file exists + no syntax error
2. Search for videos with `search_videos` → verify: step output matches expected outcome
3. Add each video with `add_to_playlist` using the playlist ID and video ID → verify: package installed + import succeeds

**Example:**
```
create_playlist({ title: "Wedding Ceremony Music", description: "Songs for our ceremony", privacy: "unlisted" })
add_to_playlist({ playlistId: "PLxxxxx", videoId: "dQw4w9WgXcQ" })
```

### List and Manage Playlists
- `list_playlists({ maxResults: 25 })` returns playlist IDs, titles, descriptions, and item counts
- `delete_playlist({ playlistId: "PLxxxxx" })` permanently removes a playlist

## Parameters Reference

### search_videos
- `query` (string, required): Search terms
- `maxResults` (number, optional): 1-50, default 10

### create_playlist
- `title` (string, required): Playlist name
- `description` (string, optional): Playlist description
- `privacy` (string, optional): "private" | "public" | "unlisted", default "private"

### add_to_playlist
- `playlistId` (string, required): Target playlist ID
- `videoId` (string, required): Video ID to add

### list_playlists
- `maxResults` (number, optional): 1-50, default 25

### delete_playlist
- `playlistId` (string, required): Playlist ID to delete

## Gotchas
- API key auth only supports read operations (search). Playlist management requires full OAuth.
- YouTube Data API has daily quota limits (10,000 units/day default). Search costs 100 units per call.
- Video IDs are the 11-character string after `v=` in YouTube URLs, not the full URL.
- Deleting a playlist is permanent and cannot be undone.
- Creating playlists defaults to "private" — change to "unlisted" or "public" to share.

## When NOT to use

- Task is unrelated to youtube tools — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Youtube Tools needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for youtube tools
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving youtube tools
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
