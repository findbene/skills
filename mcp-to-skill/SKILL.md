---
name: mcp-to-skill
description: "MCP-to-skill conversion specialist that transforms always-on MCP servers into lightweight on-demand skill plugins to eliminate context rot and restore agent performance. Use this skill any time an MCP server needs to be converted to a skill, context window is bloated from too many active MCPs, or agent performance is degrading in long sessions. Trigger immediately on: \"context rot\", \"too many MCPs\", \"convert MCP to skill\", \"reduce context\", \"MCP optimization\", \"context window bloat\", \"MCP overhead\", \"on-demand MCP\", \"skill plugin\", \"context degradation\", \"MCP memory\", \"skill conversion\", \"agent performance\", \"MCP as skill\". If someone says \"my context is bloated from MCPs\" this skill MUST trigger."
---

# MCP-to-Skill Conversion

Transform always-on MCP servers into on-demand skill plugins that load only when needed.

## Why This Matters

MCP servers register tool schemas into context at session start. Each server adds tool definitions that consume tokens every turn, whether used or not. With 10+ MCPs, this creates thousands of wasted tokens per interaction — a primary driver of context rot in long sessions.

Skills use progressive disclosure: only ~100 words of metadata are always present. The full skill body loads only when user intent triggers it. By wrapping MCP capabilities in skills, the model only "thinks about" those tools when relevant.

## Conversion Workflow

### Step 1: Audit Current MCPs

Identify all connected MCP servers and their tool counts.

```bash
# Check settings.json for configured MCPs
cat ~/.claude/settings.json | jq '.mcpServers | keys'

# Check plugin-based MCPs
ls ~/.claude/plugins/marketplaces/*/external_plugins/
```

For each MCP, note:
- **Tool count** — how many tools does it register?
- **Usage frequency** — how often do you actually use it?
- **Session relevance** — is it needed in every session or only specific contexts?

### Step 2: Classify MCPs

Sort each MCP into one of three categories:

| Category | Action | Examples |
|----------|--------|---------|
| **Always needed** | Keep as MCP | filesystem, time |
| **Sometimes needed** | Convert to skill | figma, firecrawl, sentry |
| **Rarely needed** | Convert to skill + consider disabling MCP | nanobanana, Google Calendar |

### Step 3: Generate Skill Wrapper

For each MCP being converted, create a skill that:

1. Describes when to activate (pushy trigger guidance)
2. Documents available tools and their purpose
3. Provides usage patterns and examples
4. References the underlying MCP tools by name

#### Skill Template

```markdown
---
name: [mcp-name]-tools
description: "[What the MCP does]. Use this skill any time [trigger conditions].
Trigger especially when [specific phrases/contexts]. Do NOT trigger for [exclusions]."
---

# [MCP Name] Tools

[One-line purpose statement]

## Available Tools

| Tool | Purpose | When to Use |
|------|---------|-------------|
| tool_name_1 | Description | Context |
| tool_name_2 | Description | Context |

## Common Workflows

### [Workflow 1 Name]
1. Call `tool_name_1` with [parameters]
2. Process the result
3. Call `tool_name_2` if needed

### [Workflow 2 Name]
[Steps]

## Usage Patterns

[Code examples showing typical tool invocations]

## Gotchas
- [Known limitations or edge cases]
```

### Step 4: Write the Skill File

```bash
mkdir -p ~/.claude/skills/[mcp-name]-tools
# Write the SKILL.md with the generated content
```

### Step 5: Verify Skill Triggers Correctly

Test with 3-5 representative prompts that should activate the skill. Confirm the skill loads and the underlying MCP tools are still callable.

## Pre-Built MCP Skill Wrappers

See `references/mcp-skill-map.md` for ready-made skill wrappers for common MCPs including:
- Firecrawl (web scraping)
- Brave Search (web search)
- Figma (design extraction)
- Sentry (error monitoring)
- GitHub (repo management)
- Supabase (database/backend)
- Google Calendar (scheduling)
- Gmail (email)
- NanoBanana (AI image generation)
- Sequential Thinking (reasoning chains)

## Context Budget Math

Estimate context savings per MCP conversion:

| MCP Server | Approx Tool Schemas | Tokens Saved/Turn |
|------------|--------------------|--------------------|
| Small (1-3 tools) | ~200-500 tokens | Low priority |
| Medium (4-10 tools) | ~500-1500 tokens | Good candidate |
| Large (10+ tools) | ~1500-4000 tokens | High priority |

**Rule of thumb:** Convert MCPs with 5+ tools that you use less than 30% of sessions. The token savings compound — removing 3 medium MCPs saves ~3000-4500 tokens per turn across the entire session.

## Integration with Existing Skills

This skill complements:
- **mcp-builder** — build new MCPs, then wrap them as skills
- **harness-engineering** — include skill-wrapped MCPs in reusable harnesses
- **agent-memory-system** — track which skills/MCPs are most useful per project
