---
name: codex-explain
description: "Asks Codex CLI to deeply explain how a module, function, or system works. Useful for understanding complex agent pipelines, LangGraph state machines, or inherited code."
---

# /codex:explain — Explain Code via Codex

Asks Codex CLI to deeply explain how a module, function, or system works. Useful for understanding complex agent pipelines, LangGraph state machines, or inherited code.

## Usage

```
/codex:explain <target>
```

Examples:
- `/codex:explain projects/media_pipeline/pipeline.py`
- `/codex:explain how the citadel runner dispatches agents`
- `/codex:explain the LangGraph state machine in lead_gen`

## Execution Steps

### Step 1 — Read the target
Read the file(s) mentioned so you have context before Codex runs.

### Step 2 — Run Codex

```bash
codex --approval-policy suggest "
Explain [TARGET] in this codebase in depth.

Produce a structured explanation covering:

1. PURPOSE — What does this code do? What problem does it solve?
2. ARCHITECTURE — How is it structured? What are the main components?
3. DATA FLOW — How does data enter, transform, and exit?
4. KEY DECISIONS — What design choices were made and why?
5. DEPENDENCIES — What does it depend on? What depends on it?
6. STATE MANAGEMENT — How is state tracked and mutated? (especially for LangGraph nodes)
7. ERROR PATHS — How does it handle failures?
8. GOTCHAS — What is non-obvious or surprising about this code?
9. DIAGRAM — ASCII flow diagram if the logic is complex

Be specific. Reference actual function names, class names, and line ranges.
Do not produce generic descriptions — explain THIS specific code.
"
```

### Step 3 — Summarize
After Codex responds, provide a concise 3–5 bullet TL;DR at the top for quick reference.

## Notes
- Read-only. Codex makes no changes in this skill.
- For large files, specify the function or class: `/codex:explain call_llm() in agents/llm_client.py`
