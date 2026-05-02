---
name: prompt-engineering
description: "Prompt design, optimization, and debugging for LLMs covering chain-of-thought, few-shot, role prompting, and systematic prompt testing. Use this skill any time prompts need to be designed for LLMs, prompts need to be optimized for better outputs, prompt engineering techniques need to be applied, or existing prompts are producing poor results. Trigger immediately on: \"prompt engineering\", \"improve this prompt\", \"prompt design\", \"system prompt\", \"few-shot\", \"chain of thought\", \"prompt optimization\", \"prompt debugging\", \"LLM prompt\", \"better prompt\", \"prompt template\", \"prompt structure\", \"prompt testing\", \"refine prompt\". If someone says \"my prompt is not working well\" or \"help me write a better prompt\" this skill MUST trigger."
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
1. What is the exact input? (type, format, constraints)
2. What is the exact output? (type, format, length, tone)
3. What are the failure modes? (hallucination, wrong format, off-topic)
4. What model will run this? (different models need different approaches)

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
