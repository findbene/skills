---
name: "board-deck-builder"
description: "Assembles comprehensive board and investor update decks by pulling perspectives from all C-suite roles. Triggers: 'use board-deck-builder', 'board deck builder', 'board-deck-builder task'."
allowed-tools: Glob, Grep, Read
license: MIT
metadata:
  version: 1.0.0
  author: Alireza Rezvani
  category: c-level
  domain: board-governance
  updated: 2026-03-05
  frameworks: deck-frameworks, board-deck-template
---

# Board Deck Builder

Build board decks that tell a story — not just show data. Every section has an owner, a narrative, and a "so what."

## Keywords
board deck, investor update, board meeting, board pack, investor relations, quarterly review, board presentation, fundraising deck, investor deck, board narrative, QBR, quarterly business review

## Quick Start

```
/board-deck [quarterly|monthly|fundraising] [stage: seed|seriesA|seriesB]
```

Provide available metrics. The builder fills gaps with explicit placeholders — never invents numbers.

## Deck Structure (Standard Order)

Every section follows: **Headline → Data → Narrative → Ask/Next**

### 1. Executive Summary (CEO)
**3 sentences. No more.**
- Sentence 1: State of the business (where we are)
- Sentence 2: Biggest thing that happened this period
- Sentence 3: Where we're going next quarter

*Bad:* "We had a good quarter with lots of progress across all areas."
*Good:* "We closed Q3 at $2.4M ARR (+22% QoQ), signed our largest enterprise contract, and enter Q4 with 14-month runway. The strategic shift to mid-market is working — ACV up 40% and sales cycle down 3 weeks. Q4 priority: close the $3M Series A and hit $2.8M ARR."

### 2. Key Metrics Dashboard (COO)
**6-8 metrics max. Use a table.**

| Metric | This Period | Last Period | Target | Status |
|--------|-------------|-------------|--------|--------|
| ARR | $2.4M | $1.97M | $2.3M | ✅ |
| MoM growth | 8.1% | 7.2% | 7.5% | ✅ |
| Burn multiple | 1.8x | 2.1x | <2x | ✅ |
| NRR | 112% | 108% | >110% | ✅ |
| CAC payback | 11 months | 14 months | <12 months | ✅ |
| Headcount | 24 | 21 | 25 | 🟡 |

Pick metrics the board actually tracks. Swap out anything they've said they don't care about.

### 3. Financial Update (CFO)
- P&L summary: Revenue, COGS, Gross margin, OpEx, Net burn
- Cash position and runway (months)
- Burn multiple trend (3-quarter view)
- Variance to plan (what was different and why)
- Forecast update for next quarter

**One sentence on each variance.** Boards hate "revenue was below target" with no explanation. Say why.

### 4. Revenue & Pipeline (CRO)
- ARR waterfall: starting → new → expansion → churn → ending
- NRR and logo churn rates
- Pipeline by stage (in $, not just count)
- Forecast: next quarter with confidence level
- Top 3 deals: name/amount/close date/risk

**The forecast must have a confidence level.** "We expect $2.8M" is weak. "High confidence $2.6M, upside to $2.9M if two late-stage deals close" is useful.

### 5. Product Update (CPO)
- Shipped this quarter: 3-5 bullets, user impact for each
- Shipping next quarter: 3-5 bullets with target dates
- PMF signal: NPS trend, DAU/MAU ratio, feature adoption
- One key learning from customer research

**No feature lists.** Only features with evidence of user impact.

### 6. Growth & Marketing (CMO)
- CAC by channel (table)
- Pipeline contribution by channel ($)
- Brand/awareness metrics relevant to stage (traffic, share of voice)
- What's working, what's being cut, what's being tested

### 7. Engineering & Technical (CTO)
- Delivery velocity trend (last 4 quarters)
- Tech debt ratio and plan
- Infrastructure: uptime, incidents, cost trend
- Security posture (one line, flag anything pending)

**Keep this short unless there's a material issue.** Boards don't need sprint details.

### 8. Team & People (CHRO)
- Headcount: actual vs plan
- Hiring: offers out, pipeline, time-to-fill trend
- Attrition: regrettable vs non-regrettable
- Engagement: last survey score, trend
- Key hires this quarter, key open roles

### 9. Risk & Security (CISO)
- Security posture: status of critical controls
- Compliance: certifications in progress, deadlines
- Incidents this quarter (if any): impact, resolution, prevention
- Top 3 risks and mitigation status

### 10. Strategic Outlook (CEO)
- Next quarter priorities: 3-5 items, ranked
- Key decisions needed from the board
- Asks: budget, introductions, advice, votes

**The "asks" slide is the most important.** Be specific. "We'd like 3 warm introductions to CFOs at Series B companies" beats "any help would be appreciated."

### 11. Appendix
- Detailed financial model
- Full pipeline data
- Cohort retention charts
- Customer case studies
- Detailed headcount breakdown

---

## Narrative Framework

Boards see 10+ decks per quarter. Yours needs a through-line.

**The 4-Act Structure:**
1. **Where we said we'd be** (last quarter's targets) → verify: step output matches expected outcome
2. **Where we actually are** (honest assessment) → verify: step output matches expected outcome
3. **Why the gap exists** (one cause per variance, not excuses) → verify: step output matches expected outcome
4. **What we're doing about it** (specific, dated actions) → verify: step output matches expected outcome

This works for good news AND bad news. It's credible because it acknowledges reality.

**Opening frame:** Start with the one thing that matters most — the board should know the key message by slide 3, not slide 30.

---

## Delivering Bad News

Never bury it. Boards find out eventually. Finding out late makes it worse.

**Framework:**
1. **State it plainly** — "We missed Q3 ARR target by $300K (12% gap)" → verify: step output matches expected outcome
2. **Own the cause** — "Primary driver was longer-than-expected sales cycle in enterprise segment" → verify: step output matches expected outcome
3. **Show you understand it** — "We analyzed 8 lost/stalled deals; the pattern is X" → verify: step output matches expected outcome
4. **Present the fix** — "We've made 3 changes: [specific, dated changes]" → verify: diff matches intended change
5. **Update the forecast** — "Revised Q4 target is $2.6M; here's the bottom-up build" → verify: step output matches expected outcome

**What NOT to do:**
- Don't lead with good news to soften bad news — boards notice and distrust the framing
- Don't explain without owning — "market conditions" is not a cause, it's a context
- Don't present a fix without data behind it
- Don't show a revised forecast without showing your assumptions

---

## Common Board Deck Mistakes

| Mistake | Fix |
|---------|-----|
| Too many slides (>25) | Cut ruthlessly — if you can't explain it in the room, the slide is wrong |
| Metrics without targets | Every metric needs a target and a status |
| No narrative | Data without story forces boards to draw their own conclusions |
| Burying bad news | Lead with it, own it, fix it |
| Vague asks | Specific, actionable, person-assigned asks only |
| No variance explanation | Every gap from target needs one-sentence cause |
| Stale appendix | Appendix is only useful if it's current |
| Designing for the reader, not the room | Decks are presented — they must work spoken aloud |

---

## Cadence Notes

**Quarterly (standard):** Full deck, all sections, 20-30 slides. Sent 48 hours in advance.
**Monthly (for early-stage):** Condensed — metrics dashboard, financials, pipeline, top risks. 8-12 slides.
**Fundraising:** Opens with market/vision, closes with ask. See `references/deck-frameworks.md` for Sequoia format.

## When NOT to use

- Internal exec team review (different audience, less ceremonial) — use a leaner exec dashboard
- Investor pitch deck for net-new fundraise from cold investors — use `pitch-deck`
- Sales-team or customer-facing presentations — wrong audience and framing
- Weekly leadership huddle — too much overhead, use a one-pager
- Board pre-read memo only (no slides) — write a narrative memo, not a deck

## Red Flags

| Thought | Reality |
|---------|---------|
| "I'll make up the metric, we don't have it yet" | Boards spot invented numbers; use explicit `[TBD]` placeholders |
| "Bury the miss in slide 22" | Boards always find it and lose trust in the framing |
| "30 slides for completeness" | Decks that can't be presented in the room are unread |
| "Vague ask: 'any help appreciated'" | Specific asks get specific help; vague asks get nothing |

## Output Contract

Done when:
- Deck follows standard order: Exec Summary → Metrics → Financials → Revenue → Product → Growth → Engineering → People → Risk → Strategic Outlook → Appendix
- Each section has Headline → Data → Narrative → Ask/Next
- Every metric has a target and status indicator
- Variance explanations are one-sentence, cause-not-excuse
- Bad news is on an early slide, not buried
- "Asks" are specific, actionable, person-assigned
- Missing data is shown as `[TBD]`, never invented
- Cadence-appropriate slide count (20-30 quarterly, 8-12 monthly)

## Examples

### Example 1 — Quarterly deck after a missed target
- Input: "Build Q3 deck — we missed ARR by $300K"
- Action: 3-sentence exec summary leading with the miss, metrics dashboard with status icons, financial variance one-liner per row, ARR waterfall showing where the $300K leaked, bad-news framework on slide 4 (state → own → understand → fix → revised forecast), specific asks slide at close
- Output: 24-slide deck + appendix; bad news framed honestly with dated fixes; asks list with named owners

### Example 2 — Monthly seed-stage update
- Input: "Monthly update for our 6 angels, we're pre-revenue"
- Action: Condensed 10-slide format — top 3 metrics that matter pre-revenue (cash, users, key experiment results), product shipped, hires, top 2 risks, 2 specific asks (intro list + advice on a decision)
- Output: 10 slides + 2-paragraph email summary; honest about runway; asks named with target investor names where applicable

## References
- `references/deck-frameworks.md` — SaaS board pack format, Sequoia structure, investor tailoring
- `templates/board-deck-template.md` — fill-in template for complete board decks
