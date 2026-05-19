---
name: template-skill
description: "Starter template for building new Claude skills. Copy this directory, rename, fill placeholders. Trigger phrases users say: 'create a new skill', 'skill template', 'scaffold skill', 'starter skill',."
allowed-tools: Edit, Glob, Grep, Read
---

# Template Skill

Use as scaffold for new skills. Copy directory, rename `template-skill` → `your-skill-name`, replace all `[PLACEHOLDER]` blocks below.

## When to use

- User wants to write a new skill from scratch
- Need standardized structure that passes quality audit
- Bootstrapping a skill collection

## When NOT to use

- Skill already exists and just needs editing → use Edit tool on existing SKILL.md
- Looking for finished skill examples → see `~/.claude/skills/tdd-guide/` or `~/.claude/skills/systematic-debugging/`
- Want to install community skills → use `find-skills` skill

## Process

1. Copy this directory: `cp -r template-skill your-new-skill` → verify: directory exists at new path
2. Edit `name:` frontmatter to match directory name → verify: kebab-case, matches dir
3. Write `description:` ≤200 chars with 3+ trigger phrases users would say → verify: char count `awk -F: '/^description:/{print length($0)}'`
4. Replace each `[PLACEHOLDER]` section below with skill-specific content → verify: no `[PLACEHOLDER]` strings remain `grep -c PLACEHOLDER`
5. Add ≥2 concrete examples (input → action → output) → verify: count Example headings
6. Add `references/` dir for any content pushing SKILL.md past 200 lines → verify: `wc -l SKILL.md` ≤ 200

## Red Flags

| Thought | Reality |
|---------|---------|
| "I'll skip When-NOT, my skill is obvious" | Agent loads wrong skill 60% of time without explicit skip rules |
| "Description doesn't need triggers, name is clear" | Model matches user phrasing to description text, not name |
| "Numbered process is enough, verify clauses are noise" | Agent declares done with broken output without per-step gates |
| "One example is fine" | Single example overfits — need golden path + edge case minimum |
| "I'll inline the long content for convenience" | SKILL.md > 200 lines means agent skims and misses steps |

## Output Contract

Finished SKILL.md has:
- Frontmatter: `name:` + `description:` (≤200 chars, ≥3 trigger phrases)
- `## When to use` section with ≥2 trigger conditions
- `## When NOT to use` section with explicit skip cases
- `## Process` with numbered steps, each ending `→ verify: [check]`
- `## Red Flags` table with ≥3 rationalizations
- `## Output Contract` defining done-state
- `## Examples` with ≥2 concrete cases
- File ≤200 lines OR `references/` dir for deep content

## Examples

### Example 1 — PDF reader skill
- Input: "I need a skill that reads PDFs and extracts tables"
- Action: copy template-skill to `pdf-table-extractor/`, fill description with trigger phrases ("extract tables from PDF", "PDF to CSV", "parse PDF table"), write process with PyPDF2 steps and verify clauses
- Output: working SKILL.md ≤200 lines, references/ with library API details

### Example 2 — Code review skill
- Input: "I want a skill that does security-focused code review"
- Action: copy template, name `security-code-review`, description with triggers ("review for security issues", "audit code", "find vulnerabilities"), strict process with checklist gates
- Output: SKILL.md with Red Flags for common false positives, Output Contract listing severity tiers

## References

For deep guidance on writing skills, see `~/.claude/skills/skill-creator/SKILL.md`.
