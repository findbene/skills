---
name: find-skills
description: "Skill discovery and installation from the community skills ecosystem for finding capabilities not already installed. Use this skill any time new skills need to be found or installed, the community skill library needs to be browsed, or a skill for a specific capability needs to be searched for. Trigger immediately on: \"find a skill\", \"install a skill\", \"skill library\", \"browse skills\", \"skills ecosystem\", \"new skill\", \"skill search\", \"community skills\", \"available skills\", \"skill for X\", \"what skills exist\", \"skill marketplace\", \"discover skills\". If someone says \"is there a skill for X?\" or \"find me a skill that does Y\" this skill MUST trigger."
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

1. **Identify user need** — Determine domain and specific task
2. **Search** — Run `npx skills find [query]` with relevant terms
3. **Present options** — Share skill name, install command, and skills.sh link
4. **Install** — Execute `npx skills add <owner/repo@skill> -g -y` if approved

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
