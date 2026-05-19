---
name: stitch-mcp-list-projects
description: "Lists all Stitch projects accessible to the user. Use this to find an existing project ID before. Triggers: 'use stitch-mcp-list-projects', 'stitch mcp list projects', 'stitch-mcp-list-projects task'."
allowed-tools:
  - "stitch*:*"
---

# Stitch MCP — List Projects

Lists Stitch projects available to the user. Use this when you need to find an existing projectId rather than creating a new project.

## Critical prerequisite

**Only use this skill when the user explicitly mentions "Stitch".**

## When to use

- User says "continue working on my Stitch project" without providing an ID
- User says "list my Stitch projects" or "what projects do I have?"
- You need to find an existing projectId before calling `generate_screen_from_text`
- Checking whether a project already exists before creating a new one

## Call the MCP tool

**To list owned projects (default):**
```json
{
  "name": "list_projects",
  "arguments": {
    "filter": "view=owned"
  }
}
```

**To list projects shared with the user:**
```json
{
  "name": "list_projects",
  "arguments": {
    "filter": "view=shared"
  }
}
```

**To list all accessible projects (owned + shared):**
```json
{
  "name": "list_projects",
  "arguments": {}
}
```

## Output schema

```json
{
  "projects": [
    {
      "name": "projects/3780309359108792857",
      "title": "Analytics Dashboard",
      "updateTime": "2024-11-15T10:30:00Z"
    }
  ]
}
```

## After listing

1. Present the project list to the user with titles and last-updated timestamps → verify: step output matches expected outcome
2. Ask which project to work in (if multiple) → verify: user confirms
3. Extract the numeric ID from the chosen `name` field: `projects/ID` → `ID`
4. Store both the full name and numeric ID for subsequent calls → verify: step output matches expected outcome

## ID format reminder

- `list_screens`, `get_project` → use `projects/NUMERIC_ID`
- `generate_screen_from_text`, `get_screen` → use `NUMERIC_ID` only

## When NOT to use

- Task is unrelated to stitch mcp list projects — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Stitch Mcp List Projects needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for stitch mcp list projects
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving stitch mcp list projects
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
