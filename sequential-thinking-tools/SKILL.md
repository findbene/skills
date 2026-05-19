---
name: sequential-thinking-tools
description: 'Structured step-by-step reasoning via MCP for breaking down complex problems into systematic thinking chains. Triggers: "use sequential-thinking-tools", "sequential thinking tools", "sequential task".'
allowed-tools: Glob, Grep, Read
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
1. Define the decision and constraints → verify: step output matches expected outcome
2. List each option with pros/cons → verify: step output matches expected outcome
3. Weight the criteria by importance → verify: step output matches expected outcome
4. Score each option → verify: step output matches expected outcome
5. Recommend with rationale → verify: step output matches expected outcome

### Root Cause Analysis
1. State the observed symptom → verify: step output matches expected outcome
2. List possible causes → verify: step output matches expected outcome
3. Eliminate causes using available evidence → verify: step output matches expected outcome
4. Identify most likely cause → verify: step output matches expected outcome
5. Propose verification steps → verify: step output matches expected outcome

### System Design Decomposition
1. Define requirements and constraints → verify: step output matches expected outcome
2. Identify major components → verify: step output matches expected outcome
3. Define interfaces between components → verify: step output matches expected outcome
4. Analyze failure modes → verify: step output matches expected outcome
5. Propose implementation order → verify: step output matches expected outcome

## Tips

- Use for problems where you need to show your reasoning, not just the answer
- Each thought should build on previous thoughts explicitly
- Revise `totalThoughts` as the reasoning unfolds — it's an estimate, not a commitment
- The chain creates an auditable reasoning trace the user can review

## When NOT to use

- Task is unrelated to sequential thinking tools — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Sequential Thinking Tools needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for sequential thinking tools
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving sequential thinking tools
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
