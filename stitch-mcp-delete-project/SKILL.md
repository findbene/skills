---
name: stitch-mcp-delete-project
description: "Permanently deletes a Stitch project and all its screens. Triggers: 'use stitch-mcp-delete-project', 'stitch mcp delete project', 'stitch-mcp-delete-project task'."
allowed-tools:
  - "stitch*:*"
---

# Stitch MCP — Delete Project

Permanently deletes a Stitch project and all its screens, designs, and history. This action is irreversible.

## Critical prerequisite

**Only use this skill when the user explicitly mentions "Stitch".**

**This is a destructive action.** You MUST ask the user to confirm before calling `delete_project`. Never auto-delete.

## When to use

- User explicitly asks to delete a Stitch project
- Cleaning up test/scratch projects after confirming with the user
- The orchestrator offers project cleanup and the user accepts

## Step 1: Confirm the project

Show the user what they're about to delete:

1. Call `stitch-mcp-get-project` with `projects/[ID]` to get the project title and screen count → verify: step output matches expected outcome
2. Present: "You're about to permanently delete **[title]** ([N] screens). This cannot be undone. Proceed?" → verify: step output matches expected outcome
3. Only continue if the user explicitly confirms → verify: step output matches expected outcome

## Step 2: Call the MCP tool

```json
{
  "name": "delete_project",
  "arguments": {
    "name": "projects/3780309359108792857"
  }
}
```

### `name` — full path with `projects/` prefix

```
✅ "projects/3780309359108792857"
❌ "3780309359108792857"
```

This follows the same format as `get_project` — both use `projects/ID`.

## After deleting

- Confirm: "Project **[title]** has been deleted."
- Offer: "Want to see your remaining projects?" → `stitch-mcp-list-projects`
- Do NOT attempt to access the deleted project's screens or data

## Anti-patterns

- **Never delete without explicit user confirmation** — even if the orchestrator suggests cleanup
- **Never delete multiple projects in a batch** without confirming each one
- **Never use numeric ID** — `delete_project` requires the full `projects/ID` path

## When NOT to use

- Task is unrelated to stitch mcp delete project — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Stitch Mcp Delete Project needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for stitch mcp delete project
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving stitch mcp delete project
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
