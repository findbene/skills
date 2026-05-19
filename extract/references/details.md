# Extended Details

## Usage

```
/si:extract <pattern description>                  # Interactive extraction
/si:extract <pattern> --name docker-m1-fixes       # Specify skill name
/si:extract <pattern> --output ./skills/            # Custom output directory
/si:extract <pattern> --dry-run                     # Preview without creating files
```

## When to Extract

A learning qualifies for skill extraction when ANY of these are true:

| Criterion | Signal |
|---|---|
| **Recurring** | Same issue across 2+ projects |
| **Non-obvious** | Required real debugging to discover |
| **Broadly applicable** | Not tied to one specific codebase |
| **Complex solution** | Multi-step fix that's easy to forget |
| **User-flagged** | "Save this as a skill", "I want to reuse this" |

## Quick Reference

| Problem | Solution |
|---------|----------|
| {{problem 1}} | {{solution 1}} |
| {{problem 2}} | {{solution 2}} |

## The Problem

{{2-3 sentences explaining what goes wrong and why it's non-obvious.}}

## Solutions

### Option 1: {{Name}} (Recommended)

{{Step-by-step with code examples.}}

### Option 2: {{Alternative}}

{{For when Option 1 doesn't apply.}}

## Trade-offs

| Approach | Pros | Cons |
|----------|------|------|
| Option 1 | {{pros}} | {{cons}} |
| Option 2 | {{pros}} | {{cons}} |

## Edge Cases

- {{edge case 1 and how to handle it}}
- {{edge case 2 and how to handle it}}
```

### Step 6: Quality gates

Before finalizing, verify:

- [ ] SKILL.md has valid YAML frontmatter with `name` and `description`
- [ ] `name` matches the folder name (lowercase, hyphens)
- [ ] Description includes "Use when:" trigger conditions
- [ ] Solutions are self-contained (no external context needed)
- [ ] Code examples are complete and copy-pasteable
- [ ] No project-specific hardcoded values (paths, URLs, credentials)
- [ ] No unnecessary dependencies

### Step 7: Report

```
✅ Skill extracted: {{skill-name}}

Files created:
  {{path}}/SKILL.md          ({{lines}} lines)
  {{path}}/README.md         ({{lines}} lines)
  {{path}}/reference/examples.md  ({{lines}} lines)

Install: /plugin install (copy to your skills directory)
Publish: clawhub publish {{path}}

Source: MEMORY.md entries at lines {{n, m, ...}} (retained — the skill is portable, the memory is project-specific)
```

## Tips

- Extract patterns that would save time in a *different* project
- Keep skills focused — one problem per skill
- Include the error messages people would search for
- Test the skill by reading it without the original context — does it make sense?
