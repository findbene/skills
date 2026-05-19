---
name: find-skills
description: 'Skill discovery and installation from the community skills ecosystem for finding capabilities not already installed. Triggers: "use find-skills", "find skills", "find task".'
allowed-tools: Bash, Glob, Grep, Read
---

# Find Skills

Discover and install skills from the open agent skills ecosystem.

## Skills CLI Commands

```bash
# Search for skills by keyword
npx skills find [query]

# Install from GitHub sources
npx skills add <package>

# Check for updates
npx skills check

# Update installed skills
npx skills update
```

## Implementation Steps

1. **Identify user need** — Determine domain and specific task → verify: step output matches expected outcome
2. **Search** — Run `npx skills find [query]` with relevant terms → verify: command exit code 0
3. **Present options** — Share skill name, install command, and skills.sh link → verify: package installed + import succeeds
4. **Install** — Execute `npx skills add <owner/repo@skill> -g -y` if approved → verify: command exit code 0

## Search Examples

```bash
# Find design-related skills
npx skills find "web design"

# Find testing skills
npx skills find "testing automation"

# Find AI/ML skills
npx skills find "ai image generation"
```

## Installation Examples

```bash
# Install a specific skill
npx skills add https://github.com/vercel-labs/agent-skills --skill web-design-guidelines

# Install globally with auto-yes
npx skills add vercel-labs/agent-skills@frontend-design -g -y
```

## Browse Skills

Visit [skills.sh](https://skills.sh) to browse all available skills in the ecosystem.

## When NOT to use

- Building a new skill from scratch — use `skill-creator` or `extract`
- The capability is clearly built-in (Read, Edit, Grep) — no skill needed
- Pure tool installation (npm packages, CLIs) — not a skill problem
- User already has a known skill installed — just invoke it
- Skill request is too vague — clarify first, then search

## Red Flags

| Thought | Reality |
|---------|---------|
| "Install three similar skills, pick later" | Bloats the library; one purpose, one skill |
| "Install without `-y` flag and walk away" | Hangs on prompts; for automation use `-g -y` |
| "Skip checking already-installed skills" | Duplicates create ambiguity at invocation time |
| "Install before user approves" | Always present options + install command, then act on approval |

## Output Contract

Done when:
- User need translated into search query
- `npx skills find <query>` executed
- Top candidates presented with name, install command, skills.sh link
- User explicitly approved before install
- `npx skills add <package> -g -y` run for approved skill
- Post-install: skill appears in available-skills list, ready to invoke

## Examples

### Example 1 — User wants a Notion export skill
- Input: "Find me a skill for exporting Notion pages"
- Action: `npx skills find "notion export"`, list 2-3 candidates with skills.sh URLs, await approval, install winning skill with `-g -y`
- Output: Skill installed; user told the trigger phrase to invoke it

### Example 2 — Looking for an A11y audit capability
- Input: "I need accessibility audits"
- Action: `npx skills find "accessibility audit"`, find `a11y-audit`, confirm install, run global install
- Output: `a11y-audit` skill installed and visible in available-skills
