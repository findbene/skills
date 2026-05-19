---
name: stitch-mcp-edit-screens
description: "Edits existing Stitch screens with text prompts — the iteration tool. Triggers: 'use stitch-mcp-edit-screens', 'stitch mcp edit screens', 'stitch-mcp-edit-screens task'."
allowed-tools:
  - "stitch*:*"
  - "Bash"
  - "Read"
  - "Write"
---

# Stitch MCP — Edit Screens

Edits one or more existing screens using a text prompt. This is the primary iteration tool — instead of regenerating a screen from scratch (60–180s), you can refine what you already have with targeted changes.

## Critical prerequisite

**Only use this skill when the user explicitly mentions "Stitch".**

You must have both a `projectId` AND at least one `screenId` before calling this. If you don't:
- List screens via `stitch-mcp-list-screens` to find screen IDs
- Parse from context: `projects/123/screens/456` → projectId: `123`, screenId: `456`

## When to use

- User wants to modify an existing Stitch screen (change colors, layout, content, style)
- After generation, user says "make it darker", "change the font", "move the nav to the side"
- The orchestrator's Step 5b offers "Edit this screen"
- Any iterative refinement on a Stitch design

## Call the MCP tool

```json
{
  "name": "edit_screens",
  "arguments": {
    "projectId": "3780309359108792857",
    "selectedScreenIds": ["88805abc123def456"],
    "prompt": "Change the background to dark mode (#09090B). Make the primary color indigo (#818CF8). Increase the font size of the header to 32px bold.",
    "deviceType": "DESKTOP",
    "modelId": "GEMINI_3_1_PRO"
  }
}
```

## Parameter reference

### `projectId` — numeric ID only, no prefix

```
✅ "3780309359108792857"
❌ "projects/3780309359108792857"
```

### `selectedScreenIds` — array of numeric screen IDs

```
✅ ["88805abc123def456"]
❌ ["projects/123/screens/88805abc123def456"]
❌ ["screens/88805abc123def456"]
```

Select one or more screens to edit. All selected screens receive the same edit instruction.

### `prompt` — the edit instruction

Apply the same quality bar as generation prompts:
- Be specific: "Change background to #09090B" not "make it darker"
- Name exact components: "the header navigation" not "the top part"
- Include values: hex colors, px sizes, specific content text
- One focused change per call produces better results than many changes at once

### `deviceType` — optional

Same enum as `generate_screen_from_text`: `MOBILE`, `DESKTOP`, `TABLET`, `AGNOSTIC`

### `modelId` — optional

| Value | Use when |
|-------|---------|
| `GEMINI_3_1_PRO` | **Recommended** — complex layouts, high fidelity |
| `GEMINI_3_FLASH` | Fast iteration, wireframes, simple changes |
| `GEMINI_3_PRO` | **Deprecated.** Still works but will be removed. Use `GEMINI_3_1_PRO` instead. |

## Handling `output_components`

The response may contain `output_components` with suggestions:

```json
{
  "outputComponents": [
    { "text": "I've updated the background color and adjusted contrast ratios." },
    { "suggestion": "Would you also like me to update the sidebar to match the dark theme?" }
  ]
}
```

**When you see suggestions:**
1. Present them to the user → verify: step output matches expected outcome
2. If the user accepts: call `edit_screens` again with the suggestion as the new `prompt` → verify: diff matches intended change
3. This creates a natural refinement loop — keep going until the user is satisfied → verify: output exists + parses without error

## Timing

Same as generation: 60–180 seconds is normal.
- Do NOT retry during this window
- Do NOT assume failure if it takes > 60 seconds
- Each call creates a new edit — retries mean duplicate edits

## After editing

1. Re-fetch the screen: `stitch-mcp-get-screen` with the same projectId and screenId → verify: step output matches expected outcome
2. Show the updated screenshot to the user → verify: step output matches expected outcome
3. Offer:
   - "Continue editing?" → another `edit_screens` call
   - "Generate variants of this version?" → `stitch-mcp-generate-variants`
   - "Convert to code?" → framework conversion workflow

## Anti-patterns

- **Never send vague edit prompts** — "make it better" will produce unpredictable results
- **Never use `projects/ID` format** for projectId or screenId — both must be numeric
- **Never batch unrelated edits** — "change the color AND completely redo the layout" works poorly

## When NOT to use

- Task is unrelated to stitch mcp edit screens — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Stitch Mcp Edit Screens needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for stitch mcp edit screens
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving stitch mcp edit screens
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
