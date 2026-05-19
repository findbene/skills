# Extended Details

## Overview
{2-3 sentence summary of what this skill covers}

## Key Concepts

### {Concept 1}
{Explanation with examples}

### {Concept 2}
{Explanation with examples}

## When to Apply

Use this knowledge when:
- {Situation 1}
- {Situation 2}
- {Situation 3}

## Best Practices

1. **{Practice 1}**: {Explanation}
2. **{Practice 2}**: {Explanation}
3. **{Practice 3}**: {Explanation}

## Common Mistakes

- **{Mistake 1}**: {Why it's wrong and how to fix}
- **{Mistake 2}**: {Why it's wrong and how to fix}

## Related Topics

- {Related topic 1}
- {Related topic 2}

## Sources

- {Source from web research}
- {Original video URL}
```

### Step 6: Confirm Creation

After creating the skill, report:
- Skill location
- Skill name
- Brief summary of what was captured
- How to use/reference the skill

---

## Example

User: `/skillify`

Claude: Which summary would you like to turn into a skill?

User: Use the agentic-engineering summary

Claude:
1. [Reads summary]
2. [Identifies key concepts: planning phase, agent-first setup, validation loops]
3. [Searches web for best practices]
4. [Asks where to save]

User: Save to ~/.claude/skills/ as "agentic-engineering"

Claude:
1. [Creates ~/.claude/skills/agentic-engineering/SKILL.md]
2. [Populates with researched content]
3. [Reports success]

---

## Output Example

After running `/skillify`, users get a skill file like:

```markdown
---
name: agentic-engineering
description: Best practices for AI-assisted development. Use when planning projects for Claude Code or other AI coding assistants. Apply whenever the user mentions agentic workflows, AI coding setup, project structure for agents, or asks how to work effectively with Claude Code.
user_invocable: false
---

# Agentic Engineering

## Overview
Agentic engineering is an approach to software development that optimizes for AI coding assistants...

## Key Concepts

### 90% Planning, 10% Execution
Most effort should go into research, discovery, and planning before writing code...

### Agent-First Project Setup
Structure projects so AI agents can easily navigate and understand context...
```

## Notes

- The skill should capture ACTIONABLE knowledge, not just information
- Include concrete examples whenever possible
- Reference the original video as a source
- Make the skill description "pushy" (include explicit trigger phrases) so Claude uses it automatically
- Make the skill useful for future Claude Code sessions globally
