---
name: youtube-tools
description: "YouTube video search, playlist creation and management, and channel data retrieval via MCP. Use this skill any time YouTube videos need to be searched, playlists need to be created or managed, video data needs to be retrieved, or YouTube API operations are needed. Trigger immediately on: \"YouTube\", \"YouTube video\", \"search YouTube\", \"YouTube playlist\", \"YouTube channel\", \"YouTube API\", \"find YouTube video\", \"YouTube MCP\", \"create playlist\", \"YouTube search\", \"video search\", \"YouTube data\". If someone says \"search YouTube for X\" or \"create a YouTube playlist\" this skill MUST trigger."
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
1. Call `create_playlist` with title, description, and privacy setting
2. Search for videos with `search_videos`
3. Add each video with `add_to_playlist` using the playlist ID and video ID

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
