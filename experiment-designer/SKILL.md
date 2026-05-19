---
name: experiment-designer
description: 'Use when planning product experiments, writing testable hypotheses, estimating sample size, prioritizing tests, or interp. Triggers: "use experiment-designer", "experiment designer", "experiment task.'
allowed-tools: Bash, Glob, Grep, Read
---

# Experiment Designer

Design, prioritize, and evaluate product experiments with clear hypotheses and defensible decisions.

## When To Use

Use this skill for:
- A/B and multivariate experiment planning
- Hypothesis writing and success criteria definition
- Sample size and minimum detectable effect planning
- Experiment prioritization with ICE scoring
- Reading statistical output for product decisions

## Core Workflow

1. Write hypothesis in If/Then/Because format → verify: output file exists + no syntax error
- If we change `[intervention]`
- Then `[metric]` will change by `[expected direction/magnitude]`
- Because `[behavioral mechanism]`

2. Define metrics before running test → verify: command exit code 0
- Primary metric: single decision metric
- Guardrail metrics: quality/risk protection
- Secondary metrics: diagnostics only

3. Estimate sample size → verify: step output matches expected outcome
- Baseline conversion or baseline mean
- Minimum detectable effect (MDE)
- Significance level (alpha) and power

Use:
```bash
python3 scripts/sample_size_calculator.py --baseline-rate 0.12 --mde 0.02 --mde-type absolute
```

4. Prioritize experiments with ICE → verify: step output matches expected outcome
- Impact: potential upside
- Confidence: evidence quality
- Ease: cost/speed/complexity

ICE Score = (Impact * Confidence * Ease) / 10

5. Launch with stopping rules → verify: step output matches expected outcome
- Decide fixed sample size or fixed duration in advance
- Avoid repeated peeking without proper method
- Monitor guardrails continuously

6. Interpret results → verify: step output matches expected outcome
- Statistical significance is not business significance
- Compare point estimate + confidence interval to decision threshold
- Investigate novelty effects and segment heterogeneity

## Hypothesis Quality Checklist

- [ ] Contains explicit intervention and audience
- [ ] Specifies measurable metric change
- [ ] States plausible causal reason
- [ ] Includes expected minimum effect
- [ ] Defines failure condition

## Common Experiment Pitfalls

- Underpowered tests leading to false negatives
- Running too many simultaneous changes without isolation
- Changing targeting or implementation mid-test
- Stopping early on random spikes
- Ignoring sample ratio mismatch and instrumentation drift
- Declaring success from p-value without effect-size context

## Statistical Interpretation Guardrails

- p-value < alpha indicates evidence against null, not guaranteed truth.
- Confidence interval crossing zero/no-effect means uncertain directional claim.
- Wide intervals imply low precision even when significant.
- Use practical significance thresholds tied to business impact.

See:
- `references/experiment-playbook.md`
- `references/statistics-reference.md`

## Tooling

### `scripts/sample_size_calculator.py`

Computes required sample size (per variant and total) from:
- baseline rate
- MDE (absolute or relative)
- significance level (alpha)
- statistical power

Example:
```bash
python3 scripts/sample_size_calculator.py \
  --baseline-rate 0.10 \
  --mde 0.015 \
  --mde-type absolute \
  --alpha 0.05 \
  --power 0.8
```

## When NOT to use

- Task is unrelated to experiment designer — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Experiment Designer needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for experiment designer
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving experiment designer
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
