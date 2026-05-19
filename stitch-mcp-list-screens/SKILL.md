---
name: stitch-mcp-list-screens
description: "Lists all screens in a Stitch project. Use this after generate_screen_from_text to find the screenId. Triggers: 'use stitch-mcp-list-screens', 'stitch mcp list screens', 'stitch-mcp-list-screens task."
allowed-tools:
  - "stitch*:*"
---

# Stitch MCP — List Screens

Lists all screens contained within a specific Stitch project. You typically call this right after `generate_screen_from_text` to find the `screenId` of the screen that was just created.

## Critical prerequisite

**Only use this skill when the user explicitly mentions "Stitch".**

## When to use

- Immediately after `generate_screen_from_text` — to find the new screen's ID
- User wants to browse all screens in a project
- You need a `screenId` to call `get_screen`
- Checking what screens already exist before generating a new one

## Call the MCP tool

**Important: Use the `projects/ID` format — not the numeric ID alone.**

```json
{
  "name": "list_screens",
  "arguments": {
    "projectId": "projects/3780309359108792857"
  }
}
```

```
✅ "projects/3780309359108792857"
❌ "3780309359108792857"
```

## Output schema

```json
{
  "screens": [
    {
      "name": "projects/3780309359108792857/screens/88805abc123def456",
      "title": "Login Screen",
      "screenshot": {
        "downloadUrl": "https://storage.googleapis.com/..."
      },
      "deviceType": "MOBILE"
    }
  ]
}
```

## After listing

1. Identify the target screen (usually the most recently generated — last in the list) → verify: output exists + parses without error
2. Extract the numeric `screenId` from the `name` field: → verify: step output matches expected outcome
   - `"projects/3780309359108792857/screens/88805abc123def456"` → screenId = `"88805abc123def456"`
3. Call `stitch-mcp-get-screen` with the numeric `projectId` and `screenId` → verify: step output matches expected outcome

## ID format reminder

For the next call (`get_screen`), you need the **numeric IDs** for both project and screen:
- `projectId` → `3780309359108792857` (strip `projects/` prefix)
- `screenId` → `88805abc123def456` (strip `projects/.../screens/` prefix)

## When NOT to use

- Task is unrelated to stitch mcp list screens — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Stitch Mcp List Screens needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for stitch mcp list screens
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving stitch mcp list screens
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
