---
name: stitch-mcp-create-project
description: "Creates a new Stitch project container (a design workspace). Triggers: 'use stitch-mcp-create-project', 'stitch mcp create project', 'stitch-mcp-create-project task'."
allowed-tools:
  - "stitch*:*"
---

# Stitch MCP — Create Project

Creates a new Stitch project. A project is a container that holds one or more generated screens. You need a projectId before you can call `generate_screen_from_text`.

## Critical prerequisite

**Only use this skill when the user explicitly mentions "Stitch"** in their request. Never trigger Stitch operations silently during regular conversation.

## When to use

- Starting a new design task or app concept
- The user says "create a Stitch project", "start a new project in Stitch", "design X using Stitch"
- You need a `projectId` before calling `generate_screen_from_text`
- Starting work in a new domain (keep related screens in one project)

## Step 1: Extract a meaningful title

Infer a descriptive project title from the user's request:

| User says | → title |
|-----------|---------|
| "Design a pet adoption app" | `"PawPals"` or `"Pet Adoption App"` |
| "Make a login page" | `"Login Page"` |
| "SaaS dashboard for analytics" | `"Analytics Dashboard"` |
| "E-commerce product listing" | `"Product Listing"` |

If no clear name is evident, use a descriptive label: `"Design Project [date]"`.

## Step 2: Call the MCP tool

```json
{
  "name": "create_project",
  "arguments": {
    "title": "[Descriptive project title]"
  }
}
```

## Step 3: Extract and store the project ID — CRITICAL

The tool returns a `name` field in the format `projects/NUMERIC_ID`. You must extract the numeric portion:

**Tool response:**
```json
{
  "name": "projects/3780309359108792857",
  "title": "Analytics Dashboard",
  "createTime": "2026-03-24T14:30:00Z",
  "visibility": "PRIVATE",
  "deviceType": "PHONE",
  "projectType": "TEXT_TO_UI",
  "designTheme": {
    "colorMode": "LIGHT",
    "customColor": "",
    "colorVariant": "TONAL_SPOT",
    "roundness": "ROUND_TWELVE",
    "spacingScale": 1,
    "headlineFont": "ROBOTO",
    "bodyFont": "ROBOTO",
    "labelFont": "ROBOTO",
    "namedColors": {},
    "designMd": ""
  }
}
```

> The `designTheme` populates with full values (including `designMd` and `namedColors`) after the first screen is generated. For a brand-new project, expect mostly empty/default theme values.

**Extract:**
- **Full name:** `projects/3780309359108792857` → use for `list_screens`, `get_project`
- **Numeric ID:** `3780309359108792857` → use for `generate_screen_from_text`, `get_screen`

Store both values. Announce to the user:
> "Created project **'Analytics Dashboard'** (ID: `3780309359108792857`). Ready to generate screens."

## ID format rules (critical — different tools need different formats)

| Tool | ID format required |
|------|-------------------|
| `generate_screen_from_text` | Numeric only: `3780309359108792857` |
| `get_screen` | Numeric only: `3780309359108792857` |
| `list_screens` | Full path: `projects/3780309359108792857` |
| `get_project` | Full path: `projects/3780309359108792857` |

## Integration

After creating a project, the next step is always `stitch-mcp-generate-screen-from-text` to generate the first screen.

## When NOT to use

- Task is unrelated to stitch mcp create project — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Stitch Mcp Create Project needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for stitch mcp create project
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving stitch mcp create project
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
