---
name: researcher
description: "Deep structured research specialist for comprehensive information gathering, synthesis, and evidence-based findings on any topic from multiple sources. Use this skill any time a topic needs to be thoroughly researched, information needs to be gathered and synthesized, or a research report needs to be produced. Trigger immediately on: \"research\", \"look into\", \"find out about\", \"investigate\", \"gather information\", \"deep dive\", \"what do we know about\", \"analyze this topic\", \"survey the landscape\", \"compare options\", \"background on\", \"study this\", \"explore the topic\", \"research report\". If someone says \"research X for me\" or \"I need a deep dive on Y\" this skill MUST trigger."
version: 1.0.0
triggers:
  - research this
  - deep dive
  - investigate
  - find information on
  - competitive analysis
  - market research
  - technology evaluation
  - literature review
  - compare options
  - what are the best
  - synthesize findings
  - research report
references: []
---

# Researcher Skill

## Purpose
Conduct structured, rigorous research on any topic and deliver synthesized, actionable intelligence — not raw search results.

## Activation
Load this skill when the user asks for:
- Deep research into a topic, technology, or market
- Competitive analysis (comparing products, services, approaches)
- Technology evaluation (which tool/framework/API is best for X)
- Market research (who are the players, what's the landscape)
- Literature review (summarize what experts say about X)
- Decision support (research-backed recommendation between options)

## Research Methodology

### Phase 1 — Scope Definition
Before researching, define:
1. **Central question**: What exactly needs to be answered?
2. **Scope boundaries**: What's in/out of scope?
3. **Success criteria**: What would a complete answer look like?
4. **Time sensitivity**: Is current (2025+) data critical?
5. **Depth vs. breadth**: Comprehensive coverage or focused depth?

### Phase 2 — Source Strategy
Prioritize by reliability:
```
Tier 1 (Most reliable):
  - Official documentation, API references, spec sheets
  - Peer-reviewed papers, academic publications
  - Official company announcements, investor presentations
  - Primary data (interviews, surveys)

Tier 2 (Reliable with verification):
  - Established tech publications (HN, TechCrunch, The Verge)
  - Industry analyst reports (Gartner, Forrester, IDC)
  - Reputable engineering blogs (netflixtechblog.com, etc.)
  - GitHub repos, commit history, issue trackers

Tier 3 (Use with caution):
  - Reddit, Quora, Stack Overflow — sentiment and pain points
  - Blog posts — check author credentials and recency
  - AI-generated summaries — verify all claims
  - Marketing materials — heavily biased, cross-reference everything
```

### Phase 3 — Synthesis Frameworks

#### For Technology Evaluation
| Dimension | Questions |
|---|---|
| Maturity | Production-proven? Version history? |
| Performance | Benchmarks at relevant scale? |
| Developer experience | Learning curve? |
| Community | GitHub stars, issues, SO activity? |
| Maintenance | Last commit? Team size? Corporate backing? |
| Cost | Licensing? At-scale pricing? |
| Ecosystem | Integrations, plugins? |
| Lock-in risk | How hard to migrate away? |

#### For Competitive Analysis
- Feature matrix (what each does/doesn't)
- Pricing (entry, mid-market, enterprise)
- Target customer profile
- Key differentiators
- Consistent complaints (reviews, Reddit, G2)
- Market position (leader, challenger, niche)

### Phase 4 — Output Structure
```markdown
## Executive Summary
[2-5 bullets: key findings, bottom line, recommendation]

## Background
[Why this matters, what's driving the question]

## Findings
[Organized by key topic areas, not by source]

## Comparison / Analysis
[If comparing: structured table + narrative]

## Recommendation
[If decision-needed: clear recommendation with reasoning]

## Sources & Confidence
[Key sources used, significant gaps noted]

## Open Questions
[What wasn't answered, would need additional research]
```

## AI Agency Research Profile

When researching for AI Agency, always include:
- **Cost at scale**: What does this cost at 60 videos/day?
- **API reliability**: Known failure modes?
- **Integration complexity**: Time to integrate with LangGraph/CrewAI?
- **SaaS Scout relevance**: Should this be evaluated weekly?
- **Biniyam approval required?**: Spend > $20/month?

## Parallel Research Pattern
For comprehensive topics, split into concurrent threads:
```python
threads = [
    {"domain": "pricing", "query": "What does X cost?"},
    {"domain": "features", "query": "What can X do?"},
    {"domain": "competition", "query": "Alternatives to X?"},
    {"domain": "reviews", "query": "Common complaints about X?"},
]
# Deploy as background agents, synthesize results
```
