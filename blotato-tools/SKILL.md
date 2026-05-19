---
name: blotato-tools
description: "Blotato social media publishing tool for scheduling and managing content across 9 platforms via MCP. Trigger: \\'Blotato\\', \\'schedule post\\', \\'publish to social media\\', \\'social media."
allowed-tools: Glob, Grep, Read
---

# Blotato Tools

Publish, schedule, and repurpose social media content across 9 platforms via the `blotato` MCP server.

## Available Tools

| Tool | Purpose |
|------|---------|
| `list_accounts` | List connected social accounts (get accountId) |
| `get_subaccounts` | Get Facebook pages / LinkedIn company pages (get pageId) |
| `publish_post` | Publish or schedule a post to any platform |
| `get_post_status` | Poll post status until published |
| `extract_content` | Repurpose URLs, videos, PDFs, text into content |
| `get_extraction_status` | Poll extraction job status |
| `create_visual` | Generate AI visuals from templates |
| `get_visual_status` | Poll visual creation status |
| `list_templates` | List available visual templates |
| `upload_media` | Upload media from URL for posts |

## Supported Platforms

Twitter/X, LinkedIn, Facebook, Instagram, TikTok, Pinterest, YouTube, Threads, Bluesky

## Publishing Workflow

### 1. Get Account ID
```
list_accounts → returns accountId for each connected platform
```

### 2. For Facebook/LinkedIn Pages
```
get_subaccounts({ accountId }) → returns pageId
```

### 3. Publish a Post
```json
publish_post({
  "accountId": "98432",
  "platform": "twitter",
  "text": "Hello world!",
  "mediaUrls": []
})
```

### 4. Schedule a Post
```json
publish_post({
  "accountId": "98432",
  "platform": "linkedin",
  "text": "Scheduled post",
  "mediaUrls": [],
  "scheduledTime": "2026-03-10T15:30:00Z"
})
```
Or use `"useNextFreeSlot": true` for the next available calendar slot.

### 5. Create a Thread
```json
publish_post({
  "accountId": "98432",
  "platform": "twitter",
  "text": "First tweet",
  "mediaUrls": [],
  "additionalPosts": [
    { "text": "Second tweet", "mediaUrls": [] },
    { "text": "Third tweet", "mediaUrls": [] }
  ]
})
```
Threading supported on: Twitter, Bluesky, Threads.

## Content Repurposing

Extract content from any source and transform into social posts:
```json
extract_content({
  "sourceType": "youtube",
  "url": "https://youtube.com/watch?v=..."
})
```

Source types: `article`, `youtube`, `twitter`, `tiktok`, `text`, `perplexity-query`, `audio`, `pdf`

For `text` and `perplexity-query`, use `text` field instead of `url`.

Poll with `get_extraction_status` until complete.

## Visual Creation

```json
create_visual({
  "templateId": "uuid-here",
  "prompt": "Create a motivational quote about persistence with dark background"
})
```

Status progression: queueing → generating-script → script-ready → generating-media → media-ready → exporting → done

## Platform-Specific Requirements

| Platform | Required Fields |
|----------|----------------|
| **Facebook** | `pageId` (from get_subaccounts) |
| **LinkedIn (company)** | `pageId` (from get_subaccounts) |
| **TikTok** | `platformOptions.privacyLevel`, `disabledComments`, `disabledDuet`, `disabledStitch`, `isBrandedContent`, `isYourBrand`, `isAiGenerated` |
| **Pinterest** | `platformOptions.boardId` (ask user) |
| **YouTube** | `platformOptions.title`, `privacyStatus`, `shouldNotifySubscribers` |
| **Instagram** | Optional: `platformOptions.mediaType` ("reel"/"story"), `altText`, `collaborators` |

## Rate Limits

- POST endpoints: 30 req/min
- GET status endpoints: 60 req/min
- GET user/accounts: unlimited
- HTTP 429 = rate limited, check response for retry guidance

## Critical Rules

- `platform` and `targetType` must match (handled automatically by MCP server)
- `mediaUrls` required even for text-only posts (pass `[]`)
- Scheduling fields (`scheduledTime`, `useNextFreeSlot`) are root-level, not nested in post
- Facebook always requires `pageId`
- Use bare UUID for template IDs
- All creation operations are async — poll for status

## Setup

1. Get Blotato account at https://www.blotato.com/ (starts $29/mo) → verify: step output matches expected outcome
2. Connect social accounts in Blotato settings → verify: step output matches expected outcome
3. Copy API key from Settings > API Access → verify: step output matches expected outcome
4. Set `BLOTATO_API_KEY` in settings.json → verify: step output matches expected outcome

## Complete Workflow Example

```
1. list_accounts → get accountId for Twitter
2. extract_content({ sourceType: "youtube", url: "..." }) → repurpose video
3. get_extraction_status → wait for completion
4. publish_post({ accountId, platform: "twitter", text: extracted_content }) → publish
5. get_post_status → confirm published, get publicUrl
```

## When NOT to use

- Task is unrelated to blotato tools — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Blotato Tools needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for blotato tools
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving blotato tools
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
