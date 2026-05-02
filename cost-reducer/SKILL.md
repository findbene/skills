---
name: cost-reducer
description: "Cloud and infrastructure cost optimization specialist for identifying waste, right-sizing resources, and eliminating unnecessary spend across AWS, Azure, and GCP. Use this skill any time cloud costs need to be reduced, infrastructure spend is too high, unused resources need to be identified, or cost optimization strategies need to be applied. Trigger immediately on: \"cost reduction\", \"reduce costs\", \"cloud costs\", \"AWS bill\", \"infrastructure spend\", \"too expensive\", \"cost optimization\", \"right-size\", \"unused resources\", \"waste\", \"cost savings\", \"cut costs\", \"billing\", \"overprovisioned\". If someone says \"our AWS bill is too high\" or \"how do we reduce costs?\" this skill MUST trigger."
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
1. Pull current billing breakdown by service/resource
2. Identify top 5 cost drivers (usually 80% of spend)
3. Classify each driver: waste, right-sizing opportunity, or necessary

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
1. Executive summary: current spend, projected savings, confidence level
2. Quick wins (< 2 hours each, immediate savings)
3. Medium-term optimizations (1–5 days, larger savings)
4. Strategic changes (architectural, requires planning)
5. Items to monitor but not change yet
