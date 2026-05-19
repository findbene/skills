---
name: cost-reducer
description: "Cloud and infrastructure cost optimization specialist for identifying waste, right-sizing resources, and eliminating unnecessary spen. Triggers: 'use cost-reducer', 'cost reducer', 'cost-reducer task."
allowed-tools: Glob, Grep, Read
version: 1.0.0
triggers:
  - cost audit
  - reduce cloud spend
  - optimize AWS
  - optimize Azure
  - optimize GCP
  - too expensive
  - cut costs
  - billing spike
  - FinOps
  - cost optimization
references:
  - references/cloud-and-infra.md
  - references/code-level-savings.md
  - references/services-and-finops.md
---

# Cost Reducer

## Purpose
Systematically identify and eliminate unnecessary spend across three layers: cloud infrastructure, code-level efficiency, and SaaS/service licensing.

## Activation
Load this skill when the user asks about:
- Cloud billing, cost spikes, or budget overruns
- Optimizing AWS, Azure, or GCP spend
- Reducing API call costs or compute costs
- FinOps practices, tagging, or cost allocation
- Cutting SaaS subscriptions or licensing costs

## Methodology

### Step 1 — Audit First
Never recommend changes without understanding current spend:
1. Pull current billing breakdown by service/resource → verify: step output matches expected outcome
2. Identify top 5 cost drivers (usually 80% of spend) → verify: step output matches expected outcome
3. Classify each driver: waste, right-sizing opportunity, or necessary → verify: step output matches expected outcome

### Step 2 — Layer-by-Layer Analysis
Run through all three reference layers:
- **Cloud & Infra** — idle resources, oversized instances, data transfer, storage tiers
- **Code-Level** — algorithmic inefficiency, N+1 queries, cache misses, unnecessary API calls
- **Services & FinOps** — unused SaaS seats, duplicate tools, negotiation leverage points

### Step 3 — Prioritize by ROI
Rank optimizations by: (monthly_savings × 12) / implementation_effort_hours

### Step 4 — Implement Safely
- Right-size before deleting — downsize first, monitor, then remove
- Use Reserved Instances / Savings Plans only after stable baseline (3+ months)
- Test caching changes under load before removing original infrastructure

## Key Principles
- **Measure before optimizing** — instrument first, then cut
- **Idle ≠ safe to delete** — check for scheduled jobs, backup purposes, DR roles
- **Free tiers expire** — always check if "free" resources have usage limits
- **Data transfer is invisible** — cross-region and egress costs hide in plain sight
- **Reserved capacity requires commitment** — never commit without 3 months of stable usage data

## Output Format
When producing a cost audit report:
1. Executive summary: current spend, projected savings, confidence level → verify: step output matches expected outcome
2. Quick wins (< 2 hours each, immediate savings) → verify: step output matches expected outcome
3. Medium-term optimizations (1–5 days, larger savings) → verify: step output matches expected outcome
4. Strategic changes (architectural, requires planning) → verify: step output matches expected outcome
5. Items to monitor but not change yet → verify: step output matches expected outcome

## Triggers

\\\"cost reduction\\\", \\\"reduce costs\\\", \\\"cloud costs\\\", \\\"AWS bill\\\", \\\"infrastructure spend\\\", \\\"too expensive\\\", \\\"cost optimization\\\", \\\"right-size\\\", \\\"unused resources\\\", ...

## When NOT to use

- Task is unrelated to cost reducer — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Cost Reducer needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for cost reducer
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving cost reducer
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
