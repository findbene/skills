---
name: sequential-thinking-tools
description: "Structured step-by-step reasoning via MCP for breaking down complex problems into systematic thinking chains before acting. Use this skill any time a complex problem needs to be reasoned through carefully, multi-step analysis needs to be structured, or systematic thinking needs to be applied before making a decision. Trigger immediately on: \"think step by step\", \"sequential thinking\", \"break this down logically\", \"reason through this\", \"systematic analysis\", \"step-by-step reasoning\", \"structured thinking\", \"chain of thought\", \"think this through\", \"work through this\", \"reasoning chain\", \"analyze systematically\". If someone says \"think through this carefully\" or \"reason step by step\" this skill MUST trigger."
---

# Sequential Thinking Tools

Structured step-by-step reasoning for complex multi-variable problems.

## Available Tools

| Tool | Purpose | When to Use |
|------|---------|-------------|
| `sequentialthinking` | Create a step-by-step reasoning chain | Complex problems needing structured decomposition |

## Usage

```
sequentialthinking(
  thought="Break down the authentication flow...",
  nextThoughtNeeded=true,
  thoughtNumber=1,
  totalThoughts=5
)
```

Each call represents one step in the chain. Set `nextThoughtNeeded=false` on the final step.

## When to Use

### Good Candidates
- Architecture decisions with multiple tradeoffs
- Debugging complex multi-system interactions
- Mathematical or logical proofs
- Decision trees with branching outcomes
- Root cause analysis with multiple possible causes
- Planning multi-phase migrations

### Skip This Tool For
- Simple factual questions
- Straightforward code generation
- Single-step operations

## Reasoning Patterns

### Tradeoff Analysis
1. Define the decision and constraints
2. List each option with pros/cons
3. Weight the criteria by importance
4. Score each option
5. Recommend with rationale

### Root Cause Analysis
1. State the observed symptom
2. List possible causes
3. Eliminate causes using available evidence
4. Identify most likely cause
5. Propose verification steps

### System Design Decomposition
1. Define requirements and constraints
2. Identify major components
3. Define interfaces between components
4. Analyze failure modes
5. Propose implementation order

## Tips

- Use for problems where you need to show your reasoning, not just the answer
- Each thought should build on previous thoughts explicitly
- Revise `totalThoughts` as the reasoning unfolds — it's an estimate, not a commitment
- The chain creates an auditable reasoning trace the user can review
