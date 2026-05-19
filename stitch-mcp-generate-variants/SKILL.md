---
name: stitch-mcp-generate-variants
description: "Generates design variants of existing Stitch screens using the native variant API. Triggers: 'use stitch-mcp-generate-variants', 'stitch mcp generate variants', 'stitch-mcp-generate-variants task'."
allowed-tools:
  - "stitch*:*"
---

# Stitch MCP — Generate Variants

Generates alternative versions of existing screens using Stitch's native variant generation API. This is more efficient than the text-prompt approach (1 API call vs. 3) and offers fine-grained control over what aspects to vary.

## Critical prerequisite

**Only use this skill when the user explicitly mentions "Stitch".**

You must have both a `projectId` AND at least one `screenId` of an existing screen. Variants are always based on an existing design — you can't generate variants from scratch.

## When to use

- User wants to explore alternative versions of an existing screen
- After generation, user says "show me some variations" or "try different styles"
- The orchestrator's Step 5b offers "Generate variants"
- A/B testing — creating multiple options for stakeholder review

## Call the MCP tool

```json
{
  "name": "generate_variants",
  "arguments": {
    "projectId": "3780309359108792857",
    "selectedScreenIds": ["88805abc123def456"],
    "prompt": "Explore different color schemes while keeping the layout structure",
    "variantOptions": {
      "variantCount": 3,
      "creativeRange": "EXPLORE",
      "aspects": ["COLOR_SCHEME", "IMAGES"]
    },
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
```

The source screen(s) to generate variants from.

### `prompt` — optional guidance for variant direction

Provide context about what kind of variations the user wants. The `variantOptions` do the heavy lifting, but the prompt adds nuance.

### `variantOptions` — controls what and how much to vary

| Field | Type | Values | Description |
|-------|------|--------|-------------|
| `variantCount` | int | 1–5 | Number of variants to generate |
| `creativeRange` | enum | `REFINE`, `EXPLORE`, `REIMAGINE` | How much to deviate from the original |
| `aspects` | array | See below | Which design aspects to vary |

#### `creativeRange` mapping from user language

| User says | → creativeRange | What it does |
|-----------|----------------|-------------|
| "subtle changes", "minor tweaks", "polish" | `REFINE` | Small refinements, stays close to original |
| "alternatives", "different options", "explore" | `EXPLORE` | Meaningful differences while keeping the concept |
| "radical", "completely different", "reimagine" | `REIMAGINE` | Major departures from the original design |

#### `aspects` — what to vary

| Value | Varies | Keeps stable |
|-------|--------|-------------|
| `LAYOUT` | Structure, spacing, component arrangement | Colors, fonts, content |
| `COLOR_SCHEME` | Colors, gradients, contrast | Layout, fonts, content |
| `IMAGES` | Photography, illustrations, icons | Layout, colors, text |
| `TEXT_FONT` | Typography, font choices, sizes | Layout, colors, content |
| `TEXT_CONTENT` | Copy, labels, placeholder text | Layout, colors, fonts |

You can combine aspects: `["LAYOUT", "COLOR_SCHEME"]` varies both simultaneously.

### `deviceType` — optional

Same enum: `MOBILE`, `DESKTOP`, `TABLET`, `AGNOSTIC`

### `modelId` — optional

| Value | Use when |
|-------|---------|
| `GEMINI_3_1_PRO` | **Recommended** — complex layouts, high fidelity |
| `GEMINI_3_FLASH` | Fast iteration, wireframes, simple changes |
| `GEMINI_3_PRO` | **Deprecated.** Still works but will be removed. Use `GEMINI_3_1_PRO` instead. |

## Output

Returns new screens added to the project. Each variant appears as a separate screen in `list_screens`.

## After generating variants

1. Call `stitch-mcp-list-screens` to find all new variant screens → verify: step output matches expected outcome
2. Call `stitch-mcp-get-screen` for each to get screenshots → verify: step output matches expected outcome
3. Present side by side: "Here are your 3 variants: [screenshots]. Which one do you prefer?" → verify: step output matches expected outcome
4. Once the user picks a winner, offer: → verify: step output matches expected outcome
   - "Edit the chosen variant further?" → `stitch-mcp-edit-screens`
   - "Convert to code?" → framework conversion workflow
   - "Generate more variants from the winner?" → another `generate_variants` call

## Anti-patterns

- **Never generate variants without an existing screen** — you need a source design
- **Never use `projects/ID` format** for projectId or screenId — both must be numeric
- **Never set `variantCount` above 5** — the API caps at 5

## When NOT to use

- Task is unrelated to stitch mcp generate variants — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Stitch Mcp Generate Variants needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for stitch mcp generate variants
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving stitch mcp generate variants
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
