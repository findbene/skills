---
name: "extract"
description: "Turn a proven pattern or debugging solution into a standalone reusable skill with SKILL.md, reference docs, and examples. Triggers: 'use extract', 'extract', 'extract task'."
allowed-tools: Bash, Glob, Grep, Read
---

# /si:extract — Create Skills from Patterns

Transforms a recurring pattern or debugging solution into a standalone, portable skill that can be installed in any project.

## Workflow

### Step 1: Identify the pattern

Read the user's description. Search auto-memory for related entries:

```bash
MEMORY_DIR="$HOME/.claude/projects/$(pwd | sed 's|/|%2F|g; s|%2F|/|; s|^/||')/memory"
grep -rni "<keywords>" "$MEMORY_DIR/"
```

If found in auto-memory, use those entries as source material. If not, use the user's description directly.

### Step 2: Determine skill scope

Ask (max 2 questions):
- "What problem does this solve?" (if not clear)
- "Should this include code examples?" (if applicable)

### Step 3: Generate skill name

Rules for naming:
- Lowercase, hyphens between words
- Descriptive but concise (2-4 words)
- Examples: `docker-m1-fixes`, `api-timeout-patterns`, `pnpm-workspace-setup`

### Step 4: Create the skill files

**Spawn the `skill-extractor` agent** for the actual file generation.

The agent creates:

```
<skill-name>/
├── SKILL.md            # Main skill file with frontmatter
├── README.md           # Human-readable overview
└── reference/          # (optional) Supporting documentation
    └── examples.md     # Concrete examples and edge cases
```

### Step 5: SKILL.md structure

The generated SKILL.md must follow this format:

```markdown
---
name: "skill-name"
description: "<one-line description>. Use when: <trigger conditions>."
---

# <Skill Title>

> One-line summary of what this skill solves.

## Examples

### Extracting a debugging pattern

```
/si:extract "Fix for Docker builds failing on Apple Silicon with platform mismatch"
```

Creates `docker-m1-fixes/SKILL.md` with:
- The platform mismatch error message
- Three solutions (build flag, Dockerfile, docker-compose)
- Trade-offs table
- Performance note about Rosetta 2 emulation

### Extracting a workflow pattern

```
/si:extract "Always regenerate TypeScript API client after modifying OpenAPI spec"
```

Creates `api-client-regen/SKILL.md` with:
- Why manual regen is needed
- The exact command sequence
- CI integration snippet
- Common failure modes

## When NOT to use

- One-off fix you'll never see again — not worth extracting
- Pattern tied to one specific codebase / proprietary stack — won't generalize
- Trivial commands a user already knows — bloats the skill library
- Documentation update — use a doc PR, not a skill
- Wide multi-domain knowledge — split into multiple focused skills

## Red Flags

| Thought | Reality |
|---------|---------|
| "I'll dump the whole conversation as the skill" | Bloated skills don't get re-used; distill to the pattern |
| "Skip the examples to save tokens" | Examples are the most useful part; never cut them |
| "Make one mega-skill covering everything" | One problem per skill; multi-skill split scales better |
| "Skip the description frontmatter" | Without `description:`, Claude Code can't auto-invoke |

## Output Contract

Done when:
- `<skill-name>/SKILL.md` written with valid frontmatter (name + description + trigger conditions)
- README.md (human-readable overview) present
- `reference/examples.md` for non-trivial cases
- Skill installable in any project
- Skill makes sense read standalone (no dangling references to original conversation)
- Naming follows kebab-case, 2-4 words

## References

Extended sections moved to `references/details.md`.
