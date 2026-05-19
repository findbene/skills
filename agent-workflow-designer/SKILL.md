---
name: "agent-workflow-designer"
description: 'Design multi-agent orchestration workflows - DAG-based pipelines, agent handoff patterns, state m. Triggers: ''use agent-workflow-designer'', ''build agent workflow designer'', ''agent workflow designer.'
allowed-tools: Bash, Glob, Grep, Read
---

## Overview

Design production-grade multi-agent workflows with clear pattern choice, handoff contracts, failure handling, and cost/context controls.

## Core Capabilities

- Workflow pattern selection for multi-step agent systems
- Skeleton config generation for fast workflow bootstrapping
- Context and cost discipline across long-running flows
- Error recovery and retry strategy scaffolding
- Documentation pointers for operational pattern tradeoffs

---

## When to Use

- A single prompt is insufficient for task complexity
- You need specialist agents with explicit boundaries
- You want deterministic workflow structure before implementation
- You need validation loops for quality or safety gates

---

## Quick Start

```bash
# Generate a sequential workflow skeleton
python3 scripts/workflow_scaffolder.py sequential --name content-pipeline

# Generate an orchestrator workflow and save it
python3 scripts/workflow_scaffolder.py orchestrator --name incident-triage --output workflows/incident-triage.json
```

---

## Pattern Map

- `sequential`: strict step-by-step dependency chain
- `parallel`: fan-out/fan-in for independent subtasks
- `router`: dispatch by intent/type with fallback
- `orchestrator`: planner coordinates specialists with dependencies
- `evaluator`: generator + quality gate loop

Detailed templates: `references/workflow-patterns.md`

---

## Recommended Workflow

1. Select pattern based on dependency shape and risk profile. → verify: step output matches expected outcome
2. Scaffold config via `scripts/workflow_scaffolder.py`. → verify: step output matches expected outcome
3. Define handoff contract fields for every edge. → verify: step output matches expected outcome
4. Add retry/timeouts and output validation gates. → verify: package installed + import succeeds
5. Dry-run with small context budgets before scaling. → verify: command exit code 0

---

## Common Pitfalls

- Over-orchestrating tasks solvable by one well-structured prompt
- Missing timeout/retry policies for external-model calls
- Passing full upstream context instead of targeted artifacts
- Ignoring per-step cost accumulation

## Best Practices

1. Start with the smallest pattern that can satisfy requirements. → verify: step output matches expected outcome
2. Keep handoff payloads explicit and bounded. → verify: file content matches expected shape
3. Validate intermediate outputs before fan-in synthesis. → verify: all checks pass
4. Enforce budget and timeout limits in every step. → verify: step output matches expected outcome

## When NOT to use

- Task is unrelated to agent workflow designer — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Agent Workflow Designer needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for agent workflow designer
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving agent workflow designer
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
