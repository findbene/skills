---
name: codex-ask
description: "Free-form question interface to Codex. Use when you want a second opinion, want to explore a decision, or need Codex to investigate something specific."
---

# /codex:ask — Ask Codex Any Question About the Codebase

Free-form question interface to Codex. Use when you want a second opinion, want to explore a decision, or need Codex to investigate something specific.

## Usage

```
/codex:ask <question>
```

Examples:
- `/codex:ask why does the pipeline sometimes produce empty scripts?`
- `/codex:ask what would break if I removed the quality gate?`
- `/codex:ask is the citadel runner actually wired to anything right now?`
- `/codex:ask what's the difference between core/memory.py and citadel/knowledge/?`

## Execution Steps

### Step 1 — Run Codex

```bash
codex --approval-policy suggest "
Answer the following question about this codebase by reading the relevant files:

QUESTION: [USER'S QUESTION]

Instructions:
- Read the actual code before answering — do not guess
- Cite specific file paths and line numbers to support your answer
- If the answer is 'it depends', explain what it depends on and why
- If something is broken, misconfigured, or missing, say so directly
- Keep the answer focused — do not summarize the whole codebase unless asked
- If the question cannot be answered from the code alone, say what additional information is needed
"
```

### Step 2 — Deliver answer
Present Codex's response directly. Add your own commentary only if Codex missed something or the answer needs clarification.

## Notes
- Read-only. Codex makes no changes.
- For complex multi-part investigations, use `/codex:adversarial-review` instead.
