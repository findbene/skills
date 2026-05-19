---
name: researcher
description: "Deep structured research specialist for comprehensive information gathering, synthesis, and evidence-based findings on any topic from multi. Triggers: 'use researcher', 'researcher', 'researcher task."
allowed-tools: Glob, Grep, Read
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
1. **Central question**: What exactly needs to be answered? → verify: step output matches expected outcome
2. **Scope boundaries**: What's in/out of scope? → verify: step output matches expected outcome
3. **Success criteria**: What would a complete answer look like? → verify: step output matches expected outcome
4. **Time sensitivity**: Is current (2025+) data critical? → verify: step output matches expected outcome
5. **Depth vs. breadth**: Comprehensive coverage or focused depth? → verify: file content matches expected shape

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

## Triggers

\\\"research\\\", \\\"look into\\\", \\\"find out about\\\", \\\"investigate\\\", \\\"gather information\\\", \\\"deep dive\\\", \\\"what do we know about\\\", \\\"analyze this topic\\\", \\\"survey the landscape\\\", \\\"co...

## When NOT to use

- Quick fact lookup (one search query) — overkill; use WebFetch directly
- Competitive sales intel for a deal — use `competitive-intel` or `deal-strategist`
- Academic literature review with citations — use `autoresearch-agent`
- Code search inside a repository — use Grep/Glob directly
- Market sizing only — use a financial-modeling skill

## Red Flags

| Rationalization | Reality |
|---|---|
| "One source is enough" | Single-source findings are dangerous; cross-reference at least 3 independent sources |
| "Skip the synthesis, paste the raw search results" | The user wants actionable conclusions, not search-engine output; always synthesize |
| "Confidence level is obvious" | Always label findings (high/medium/low confidence) — undocumented confidence breeds overreliance |
| "Recency does not matter for this topic" | Tech/markets shift fast; always note recency of sources |

## Output Contract

Finished output must contain:
- Question or hypothesis stated explicitly at the top
- Search strategy used (terms, sources, recency filter)
- Findings organized by sub-question with citations
- At least 3 independent sources cross-checked
- Confidence label per major finding (high/medium/low) with rationale
- Open questions / known unknowns called out
- Recommendation or next step, not just raw findings

## Examples

**Example 1 — Tech evaluation: Postgres vs ClickHouse for events**
- Input: "Should we use Postgres or ClickHouse for our 100M events/day analytics workload?"
- Action: Search benchmarks, ops considerations, query patterns → identify tradeoffs → cross-check 3 case studies → synthesize
- Output: Decision matrix, ClickHouse recommended for >10M events/day with rationale, 3 caveats, citations + confidence labels

**Example 2 — Market research: AI coding assistants**
- Input: "Survey the state of AI coding assistants in 2026"
- Action: Search market reports, product comparisons, pricing → categorize (IDE plugins, terminal agents, autonomous) → identify trends and gaps
- Output: Landscape map, top 8 products with positioning, 3 emerging trends, 2 open opportunities, confidence labels, recency notes
