---
name: "ma-playbook"
description: "M&A strategy for acquiring companies or being acquired. Due diligence, valuation, integration, and deal structure. Triggers: 'use ma-playbook', 'ma playbook', 'ma-playbook task'."
allowed-tools: Glob, Grep, Read
license: MIT
metadata:
  version: 1.0.0
  author: Alireza Rezvani
  category: c-level
  domain: ma-strategy
  updated: 2026-03-05
---

# M&A Playbook

Frameworks for both sides of M&A: acquiring companies and being acquired.

## Keywords
M&A, mergers and acquisitions, due diligence, acquisition, acqui-hire, integration, deal structure, valuation, LOI, term sheet, earnout

## Quick Start

**Acquiring:** Start with strategic rationale → target screening → due diligence → valuation → negotiation → integration.

**Being Acquired:** Start with readiness assessment → data room prep → advisor selection → negotiation → transition.

## When You're Acquiring

### Strategic Rationale (answer before anything else)
- **Buy vs Build:** Can you build this faster/cheaper? If yes, don't acquire.
- **Acqui-hire vs Product vs Market:** What are you really buying? Talent? Technology? Customers?
- **Integration complexity:** How hard is it to merge this into your company?

### Due Diligence Checklist
| Domain | Key Questions | Red Flags |
|--------|--------------|-----------|
| Financial | Revenue quality, customer concentration, burn rate | >30% revenue from 1 customer |
| Technical | Code quality, tech debt, architecture fit | Monolith with no tests |
| Legal | IP ownership, pending litigation, contracts | Key IP owned by individuals |
| People | Key person risk, culture fit, retention risk | Founders have no lockup/earnout |
| Market | Market position, competitive threats | Declining market share |
| Customers | Churn rate, NPS, contract terms | High churn, short contracts |

### Valuation Approaches
- **Revenue multiple:** Industry-dependent (2-15x ARR for SaaS)
- **Comparable transactions:** What similar companies sold for
- **DCF:** For profitable companies only (most startups: use multiples)
- **Acqui-hire:** $1-3M per engineer in hot markets

### Integration Frameworks
See `references/integration-playbook.md` for the 100-day integration plan.

## When You're Being Acquired

### Readiness Signals
- Inbound interest from strategic buyers
- Market consolidation happening around you
- Fundraising becomes harder than operating
- Founder ready for a transition

### Preparation (6-12 months before)
1. Clean up financials (audited if possible) → verify: findings count > 0 OR clean signal returned
2. Document all IP and contracts → verify: step output matches expected outcome
3. Reduce customer concentration → verify: step output matches expected outcome
4. Lock up key employees → verify: step output matches expected outcome
5. Build the data room → verify: step output matches expected outcome
6. Engage an M&A advisor → verify: step output matches expected outcome

### Negotiation Points
| Term | What to Watch | Your Leverage |
|------|--------------|---------------|
| Valuation | Earnout traps (unreachable targets) | Multiple competing offers |
| Earnout | Milestone definitions, measurement period | Cash-heavy vs earnout-heavy split |
| Lockup | Duration, conditions | Your replaceability |
| Rep & warranties | Scope of liability | Escrow vs indemnification cap |
| Employee retention | Who gets offers, at what terms | Key person dependencies |

## Red Flags (Both Sides)

- No clear strategic rationale beyond "it's a good deal"
- Culture clash visible during due diligence and ignored
- Key people not locked in before close
- Integration plan doesn't exist or is "we'll figure it out"
- Valuation based on projections, not actuals

## Integration with C-Suite Roles

| Role | Contribution to M&A |
|------|-------------------|
| CEO | Strategic rationale, negotiation lead |
| CFO | Valuation, deal structure, financing |
| CTO | Technical due diligence, integration architecture |
| CHRO | People due diligence, retention planning |
| COO | Integration execution, process merge |
| CPO | Product roadmap impact, customer overlap |

## Resources
- `references/integration-playbook.md` — 100-day post-acquisition integration plan
- `references/due-diligence-checklist.md` — comprehensive DD checklist by domain

## When NOT to use

- Pure fundraising (Series A/B) — use `pitch-deck` or `launch-strategy`
- Vendor procurement / commercial contract review — use `proposal-strategist`
- Operational integration without acquisition context — use `migration-architect`
- Pure valuation modeling — use `financial-analyst`
- Internal reorg without external entity — use `change-management`

## Output Contract

Done when:
- Side identified (acquiring vs being acquired) and matched workflow followed
- Strategic rationale answered explicitly (Buy-vs-Build, what's being bought, integration complexity)
- Due-diligence checklist run across all 6 domains (financial, technical, legal, people, market, customers)
- Valuation triangulated with at least 2 approaches (multiple, comparable, DCF, or acqui-hire)
- Deal-term watchlist filled: valuation, earnout, lockup, R&W, retention
- 100-day integration outline drafted before close
- All C-suite roles' contributions captured per the integration table

## Examples

### Example 1 — Evaluating an acquisition target
- Input: "We want to acquire a smaller competitor for their customer base"
- Action: Strategic rationale (buying customers, not tech); 6-domain DD with priority on customer concentration + churn; revenue-multiple valuation; integration architecture sketched; 100-day plan referenced
- Output: DD memo with red flags called out, valuation range, deal-structure recommendation (cash + earnout split), integration plan starter

### Example 2 — Being approached by a strategic buyer
- Input: "A bigger company wants to acquire us, where do we start?"
- Action: Readiness assessment, data-room prep list, advisor recommendation, negotiation watchlist (earnout traps, lockup, R&W), competing-offer leverage plan
- Output: Readiness scorecard, 6-12 month prep timeline, term-sheet watchlist, advisor short-list
