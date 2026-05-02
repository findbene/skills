---
name: google-stitch
description: "Google Stitch AI (Gemini) for generating UI/UX designs and frontend code from natural language prompts. Use this skill any time UI designs need to be generated using Google Stitch, Gemini needs to be used for frontend design generation, or design-to-code workflows need to use Google Stitch. Trigger immediately on: \"Google Stitch\", \"Stitch AI\", \"Gemini design\", \"generate UI with Stitch\", \"Stitch frontend\", \"Google Stitch design\", \"Stitch code generation\", \"design with Gemini\", \"Stitch prototype\", \"Stitch UI\". If someone says \"use Google Stitch to design this\" or \"generate UI with Stitch\" this skill MUST trigger."
trigger: When the user wants to generate UI designs, create frontend screens from prompts, extract design context/DNA from existing screens, build sites from Stitch projects, or work with Google Stitch in any capacity.
tools: stitch MCP server (must be running)
---

# Google Stitch - AI UI/UX Design Generation Skill

## What is Google Stitch?

Google Stitch is an experimental tool from Google Labs, powered by **Gemini 2.5 Pro**, that generates complete UI designs and production-ready HTML/CSS/JS code from text prompts. This skill connects Claude Code to Stitch via the MCP proxy server.

## MCP Server

The Stitch MCP server is configured in `~/.claude/settings.json` under `mcpServers.stitch`:

```json
{
  "stitch": {
    "command": "npx",
    "args": ["@_davideast/stitch-mcp", "proxy"]
  }
}
```

## Prerequisites

Before using Stitch tools, the user must authenticate:

```bash
npx @_davideast/stitch-mcp init
```

This runs a guided wizard for:
1. Google Cloud authentication (`gcloud auth application-default login`)
2. Project selection/creation
3. Stitch API enablement
4. MCP client configuration

To verify setup:
```bash
npx @_davideast/stitch-mcp doctor
```

## Available MCP Tools

### Core Design Tools

| Tool | Purpose | When to Use |
|------|---------|-------------|
| `generate_screen_from_text` | Generate a new UI screen from a text prompt + optional design context | User wants to create a new page/screen/component |
| `get_screen_code` | Download the raw HTML/CSS/JS code for a screen | User wants to integrate generated code into their project |
| `get_screen_image` | Download a screenshot of a screen as base64 | User wants to preview or compare designs |
| `extract_design_context` | Scan a screen to extract "Design DNA" (fonts, colors, layouts, spacing) | User wants to maintain visual consistency across screens |

### Project Management Tools

| Tool | Purpose |
|------|---------|
| `create_project` | Create a new Stitch design project |
| `list_projects` | List all accessible Stitch projects |
| `get_project` | Get metadata for a specific project |
| `list_screens` | List all screens in a project |
| `get_screen` | Get metadata for a specific screen |

### Site Building Tools

| Tool | Purpose |
|------|---------|
| `build_site` | Map screens to routes and generate a multi-page site |

## Workflows

### 1. Generate a Single Screen

```
User: "Create a landing page for a SaaS product"

Steps:
1. Call `generate_screen_from_text` with the user's prompt
2. Call `get_screen_image` to preview the result
3. Call `get_screen_code` to retrieve the HTML
4. Write the code to the user's project
```

### 2. Designer Flow (Consistent Multi-Screen Design)

This is the recommended workflow for generating multiple screens that share a cohesive design system:

```
Step 1 - Extract: Call `extract_design_context` on a reference screen
  -> Returns Design DNA: fonts, colors, spacing, layout patterns

Step 2 - Generate: Call `generate_screen_from_text` with:
  - The user's prompt for the new screen
  - The extracted design context as input
  -> New screen matches the existing design system

Step 3 - Repeat Step 2 for each additional screen
```

### 3. Build a Full Site

```
1. Create a project with `create_project`
2. Generate screens using the Designer Flow above
3. Call `build_site` to map screens to routes:
   {
     "projectId": "abc123",
     "routes": [
       { "screenId": "screen1", "route": "/" },
       { "screenId": "screen2", "route": "/about" },
       { "screenId": "screen3", "route": "/pricing" }
     ]
   }
4. Write the generated HTML files to the project directory
```

### 4. Extract and Apply Design System

```
1. Call `extract_design_context` on existing screen
2. Parse the Design DNA output:
   - Typography: font families, sizes, weights
   - Colors: primary, secondary, accent, background, text
   - Spacing: padding, margin, gap values
   - Layout: grid/flex patterns, breakpoints
3. Generate CSS custom properties or Tailwind config from the extracted values
4. Apply to the user's project
```

## CLI Commands (Direct Shell Access)

These can be run via Bash when MCP tools aren't sufficient:

| Command | Usage |
|---------|-------|
| `npx @_davideast/stitch-mcp init` | First-time setup wizard |
| `npx @_davideast/stitch-mcp doctor` | Verify configuration health |
| `npx @_davideast/stitch-mcp serve -p <id>` | Local Vite preview of all screens |
| `npx @_davideast/stitch-mcp screens -p <id>` | Browse screens in terminal |
| `npx @_davideast/stitch-mcp view` | Interactive project/screen browser |
| `npx @_davideast/stitch-mcp site -p <id>` | Generate Astro project from screens |
| `npx @_davideast/stitch-mcp snapshot` | Save current screen state |
| `npx @_davideast/stitch-mcp logout` | Revoke credentials |

## Environment Variables

| Variable | Purpose |
|----------|---------|
| `STITCH_API_KEY` | Direct API key authentication (bypasses OAuth) |
| `STITCH_ACCESS_TOKEN` | Pre-existing access token |
| `STITCH_PROJECT_ID` | Default project ID |
| `GOOGLE_CLOUD_PROJECT` | Google Cloud project ID |
| `STITCH_USE_SYSTEM_GCLOUD` | Use system gcloud instead of isolated |
| `STITCH_HOST` | Custom API endpoint |

## Integration Patterns

### With WordPress (remotetechgear.com pattern)
1. Generate a page design with Stitch
2. Extract the HTML from `get_screen_code`
3. Convert to WordPress template PHP
4. Add WordPress template header comment
5. Enqueue any generated CSS into the theme

### With React/Next.js
1. Generate screens with Stitch
2. Extract code and convert JSX-incompatible HTML (class -> className, etc.)
3. Extract inline styles to CSS modules or Tailwind classes
4. Create React components from the generated markup

### With Static Sites
1. Use `build_site` to generate route-mapped HTML
2. Use `npx @_davideast/stitch-mcp site -p <id>` to generate an Astro project
3. Deploy to Vercel/Netlify/Cloudflare Pages

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Permission denied | Run `npx @_davideast/stitch-mcp doctor --verbose`, check billing/roles |
| Auth URL not appearing | URLs appear with 5s timeout; check `/tmp/stitch-proxy-debug.log` |
| Already authenticated but stuck | `npx @_davideast/stitch-mcp logout --force --clear-config` |
| API connection fails | Re-run `npx @_davideast/stitch-mcp init` |
| WSL/SSH/Docker | Copy OAuth URL from terminal, open manually in browser |

## Best Practices

1. **Always use Designer Flow** for multi-screen projects to maintain consistency
2. **Extract design context first** from any existing reference before generating new screens
3. **Review generated code** before integrating - Stitch outputs are a starting point, not production-ready
4. **Use `build_site`** for multi-page projects instead of generating screens individually
5. **Save snapshots** of screens you like before iterating further
6. **Check `doctor`** if tools stop responding - token refresh may be needed
