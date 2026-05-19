---
name: prompt-engineering
description: "Prompt design, optimization, and debugging for LLMs covering chain-of-thought, few-shot, role prompting, and sy. Triggers: 'use prompt-engineering', 'engineer prompt engineering', 'prompt engineering."
allowed-tools: Glob, Grep, Read
version: 1.0.0
triggers:
  - prompt engineering
  - design a prompt
  - system prompt
  - improve this prompt
  - prompt optimization
  - few-shot examples
  - chain of thought
  - prompt for Claude
  - LLM prompting
  - prompt template
  - prompt debugging
  - write a system prompt
  - prompt evaluation
references:
  - references/examples.md
  - references/reference.md
---

# Prompt Engineering Skill

## Purpose
Design, optimize, and debug prompts that reliably produce high-quality outputs from large language models across diverse tasks: generation, classification, reasoning, extraction, and tool use.

## Activation
Load this skill when the user asks about:
- Writing or improving system prompts
- Designing few-shot examples for a task
- Implementing chain-of-thought reasoning
- Optimizing prompts for Claude, GPT-4o, or Gemini
- Prompt evaluation and iteration methodology
- RAG prompt design (context injection)
- Structured output prompts (JSON schema enforcement)
- Tool use / function calling prompt patterns

## Prompt Engineering Methodology

### Step 1 — Define the Task Precisely
Before writing a prompt:
1. What is the exact input? (type, format, constraints) → verify: step output matches expected outcome
2. What is the exact output? (type, format, length, tone) → verify: step output matches expected outcome
3. What are the failure modes? (hallucination, wrong format, off-topic) → verify: step output matches expected outcome
4. What model will run this? (different models need different approaches) → verify: command exit code 0

### Step 2 — Start Simple, Add Complexity
```
Version 1: Plain instruction (measure baseline)
Version 2: Add output format specification
Version 3: Add 1-3 few-shot examples
Version 4: Add chain-of-thought reasoning
Version 5: Add constraints and edge case handling
```
Always measure quality at each version. Don't add complexity that doesn't improve quality.

### Step 3 — Evaluate Systematically
- Create a test set of 20–50 representative inputs
- Define binary pass/fail criteria for each output
- Score each version: pass_rate = passing / total
- Use automated evaluation where possible (LLM-as-judge for subjective tasks)

## Key Principles

### Be Specific, Not Vague
```
VAGUE: "Write a good script about AI."
SPECIFIC: "Write a 45-second YouTube Shorts script about one specific AI tool.
Structure: Hook (3s) → Problem (10s) → Tool demo (20s) → Result (12s).
Tone: Conversational, enthusiastic but not salesy. Target: developers aged 25-35.
Output: Script only, no stage directions."
```

### Format Drives Compliance
Explicitly specify format when you need structured output:
- "Respond only with valid JSON"
- "Use exactly this structure: [structure]"
- "Do not include any text before or after the JSON"

### Constraints Are Positive
Frame constraints positively when possible:
- Instead of "Don't write marketing copy" → "Write in a factual, neutral tone"
- Instead of "Don't be too long" → "Keep response under 200 words"

### Model-Specific Notes
- **Claude**: Excels at instruction following, nuanced reasoning, XML tags for structure
- **GPT-4o**: Strong at code, JSON output, function calling
- **Gemini**: Strong at long context, multi-modal, structured data extraction
- **Claude Haiku**: Use for high-volume simple tasks (classification, formatting, extraction)

## Temperature Guide
| Task | Temperature | Why |
|---|---|---|
| Data extraction, classification | 0.0–0.2 | Deterministic, consistent |
| Q&A, summarization | 0.3–0.5 | Accurate but not robotic |
| Script writing, copywriting | 0.7–0.9 | Creative variation |
| Brainstorming, ideation | 0.9–1.2 | Maximum diversity |
| Code generation | 0.0–0.3 | Correctness over creativity |

## Triggers

\\\"prompt engineering\\\", \\\"improve this prompt\\\", \\\"prompt design\\\", \\\"system prompt\\\", \\\"few-shot\\\", \\\"chain of thought\\\", \\\"prompt optimization\\\", \\\"prompt debugging\\\", \\\"LLM prompt\\\", \\\"better prompt\\...

## When NOT to use

- Prompt optimization for the Claude chat app specifically — use `47`
- Building MCP tool prompts / function-calling design — use `mcp-builder`
- Agent design (multi-agent systems) — use `agent-designer` / `senior-prompt-engineer`
- Pure copywriting (marketing copy) — use `copywriting`
- General LLM cost optimization — use `llm-cost-optimizer`

## Red Flags

| Rationalization | Reality |
|---|---|
| "Add 10 examples to be safe" | Diminishing returns past 3-5; too many examples bias the model toward verbatim patterns |
| "Use chain-of-thought for everything" | CoT slows responses and bloats tokens; use only on multi-step reasoning tasks |
| "Skip eval, eyeballing is enough" | LLMs vary; without a written eval set, regressions are silent |
| "One prompt for all use cases" | Task-specialized prompts outperform monolithic prompts; modularize |

## Output Contract

Finished output must contain:
- Goal/task statement explicitly written at the top of the prompt
- Constraints and output format spec (preferred: JSON schema or example)
- Few-shot examples (1-5) when task is non-trivial
- Failure-mode mitigations (refuse, fall back, ask for clarification rules)
- Eval set of at least 5 input/output pairs for regression testing
- Notes on model variant and temperature recommended

## Examples

**Example 1 — Design a classification prompt**
- Input: "Classify support tickets into Billing, Tech, Sales, Other"
- Action: Write task + label definitions → 3 few-shot per label → constraint to output single label JSON → write 10 eval cases covering edge cases (ambiguous, no-fit)
- Output: System prompt + JSON output schema + `evals.jsonl` with 10 cases + recommendation Claude Haiku 4.5 @ temp 0

**Example 2 — Debug a flaky extraction prompt**
- Input: "My address extractor sometimes returns null even when address is present"
- Action: Inspect prompt → identify ambiguity (no canonical format example) → add 3 normalized format examples → add fallback ("if unsure, return the longest candidate") → re-run on eval set
- Output: Revised prompt, eval pass rate 88% → 96%, diff annotated with rationale
