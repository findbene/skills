---
name: "setup"
description: "Set up a new autoresearch experiment interactively. Collects domain, target file, eval command, metric, direction, and evaluator. Triggers: 'use setup', 'setup', 'setup task'."
allowed-tools: Bash, Glob, Grep, Read
command: /ar:setup
---

# /ar:setup — Create New Experiment

Set up a new autoresearch experiment with all required configuration.

## Usage

```
/ar:setup                                    # Interactive mode
/ar:setup engineering api-speed src/api.py "pytest bench.py" p50_ms lower
/ar:setup --list                             # Show existing experiments
/ar:setup --list-evaluators                  # Show available evaluators
```

## What It Does

### If arguments provided

Pass them directly to the setup script:

```bash
python {skill_path}/scripts/setup_experiment.py \
  --domain {domain} --name {name} \
  --target {target} --eval "{eval_cmd}" \
  --metric {metric} --direction {direction} \
  [--evaluator {evaluator}] [--scope {scope}]
```

### If no arguments (interactive mode)

Collect each parameter one at a time:

1. **Domain** — Ask: "What domain? (engineering, marketing, content, prompts, custom)" → verify: user confirms
2. **Name** — Ask: "Experiment name? (e.g., api-speed, blog-titles)" → verify: user confirms
3. **Target file** — Ask: "Which file to optimize?" Verify it exists.
4. **Eval command** — Ask: "How to measure it? (e.g., pytest bench.py, python evaluate.py)" → verify: all checks pass
5. **Metric** — Ask: "What metric does the eval output? (e.g., p50_ms, ctr_score)" → verify: user confirms
6. **Direction** — Ask: "Is lower or higher better?" → verify: user confirms
7. **Evaluator** (optional) — Show built-in evaluators. Ask: "Use a built-in evaluator, or your own?" → verify: user confirms
8. **Scope** — Ask: "Store in project (.autoresearch/) or user (~/.autoresearch/)?" → verify: user confirms

Then run `setup_experiment.py` with the collected parameters.

### Listing

```bash
# Show existing experiments
python {skill_path}/scripts/setup_experiment.py --list

# Show available evaluators
python {skill_path}/scripts/setup_experiment.py --list-evaluators
```

## Built-in Evaluators

| Name | Metric | Use Case |
|------|--------|----------|
| `benchmark_speed` | `p50_ms` (lower) | Function/API execution time |
| `benchmark_size` | `size_bytes` (lower) | File, bundle, Docker image size |
| `test_pass_rate` | `pass_rate` (higher) | Test suite pass percentage |
| `build_speed` | `build_seconds` (lower) | Build/compile/Docker build time |
| `memory_usage` | `peak_mb` (lower) | Peak memory during execution |
| `llm_judge_content` | `ctr_score` (higher) | Headlines, titles, descriptions |
| `llm_judge_prompt` | `quality_score` (higher) | System prompts, agent instructions |
| `llm_judge_copy` | `engagement_score` (higher) | Social posts, ad copy, emails |

## After Setup

Report to the user:
- Experiment path and branch name
- Whether the eval command worked and the baseline metric
- Suggest: "Run `/ar:run {domain}/{name}` to start iterating, or `/ar:loop {domain}/{name}` for autonomous mode."

## When NOT to use

- Task is unrelated to setup — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Setup needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for setup
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving setup
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
