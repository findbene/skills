---
name: "marketing-strategy-pmm"
description: "Product marketing skill for positioning, GTM strategy, competitive intelligence, and product launches. Triggers: 'use marketing-strategy-pmm', 'marketing strategy pmm', 'marketing-strategy-pmm task'."
allowed-tools: Glob, Grep, Read
triggers:
  - product marketing
  - PMM
  - positioning
  - GTM strategy
  - go-to-market
  - competitive analysis
  - battlecard
  - product launch
  - market entry
  - sales enablement
  - win loss analysis
---

# Marketing Strategy & PMM

Product marketing patterns for positioning, GTM strategy, and competitive intelligence.

---

## Table of Contents

- [ICP Definition Workflow](#icp-definition-workflow)
- [Positioning Development](#positioning-development)
- [Competitive Intelligence](#competitive-intelligence)
- [Product Launch Planning](#product-launch-planning)
- [Sales Enablement](#sales-enablement)
- [International Expansion](#international-expansion)
- [Reference Documentation](#reference-documentation)

---

## ICP Definition Workflow

Define ideal customer profile for targeting:

1. Analyze existing customers (top 20% by LTV) → verify: step output matches expected outcome
2. Identify common firmographics (size, industry, revenue) → verify: step output matches expected outcome
3. Map technographics (tools, maturity, integrations) → verify: step output matches expected outcome
4. Document psychographics (pain level, motivation, risk tolerance) → verify: step output matches expected outcome
5. Define 3-5 buyer personas (economic, technical, user) → verify: step output matches expected outcome
6. Validate against sales cycle and churn data → verify: step output matches expected outcome
7. Score prospects A/B/C/D based on ICP fit → verify: step output matches expected outcome
8. **Validation:** A-fit customers have lowest churn and fastest close → verify: all tests pass

### Firmographics Template

| Dimension | Target Range | Rationale |
|-----------|--------------|-----------|
| Employees | 50-5000 | Series A sweet spot |
| Revenue | $5M-$500M | Budget available |
| Industry | SaaS, Tech, Services | Product fit |
| Geography | US, UK, DACH | Market priority |
| Funding | Seed to Growth | Willing to adopt |

### Buyer Personas

| Persona | Title | Goals | Messaging |
|---------|-------|-------|-----------|
| Economic Buyer | VP, Director, Head of [Department] | ROI, team productivity, cost reduction | Business outcomes, ROI, case studies |
| Technical Buyer | Engineer, Architect, Tech Lead | Technical fit, easy integration | Architecture, security, documentation |
| User/Champion | Manager, Team Lead, Power User | Makes job easier, quick wins | UX, ease of use, time savings |

### ICP Validation Checklist

- [ ] 5+ paying customers match this profile
- [ ] Fastest sales cycles (< median)
- [ ] Highest LTV (> median)
- [ ] Lowest churn (< 5% annual)
- [ ] Strong product engagement
- [ ] Willing to do case studies

---

## Positioning Development

Develop positioning using April Dunford methodology:

1. List competitive alternatives (direct, adjacent, status quo) → verify: step output matches expected outcome
2. Isolate unique attributes (features only you have) → verify: step output matches expected outcome
3. Map attributes to customer value (why it matters) → verify: step output matches expected outcome
4. Define best-fit customers (who cares most) → verify: step output matches expected outcome
5. Choose market category (head-to-head, niche, new category) → verify: step output matches expected outcome
6. Layer on relevant trends (timing justification) → verify: step output matches expected outcome
7. Test with 10+ customer interviews → verify: all checks pass
8. **Validation:** 7+ customers describe value unprompted → verify: step output matches expected outcome

### Positioning Statement Template

```
FOR [target customer]
WHO [statement of need]
THE [product] IS A [category]
THAT [key benefit]
UNLIKE [competitive alternative]
OUR PRODUCT [primary differentiation]
```

### Value Proposition Formula

Template: `[Product] helps [Target Customer] [Achieve Goal] by [Unique Approach]`

Example: "Acme helps mid-market SaaS teams ship 2x faster by automating project workflows with AI"

### Messaging Hierarchy

| Level | Content | Example |
|-------|---------|---------|
| Headline | 5-7 words | "Ship faster with AI automation" |
| Subhead | 1 sentence | "Automate workflows so teams focus on what matters" |
| Benefits | 3-4 bullets | Speed, quality, collaboration, cost |
| Features | Supporting evidence | AI automation → 10 hrs/week saved |
| Proof | Social proof | Customer logos, stats, case studies |

---

## Competitive Intelligence

Build competitive knowledge base:

1. Identify tier 1 (direct), tier 2 (adjacent), tier 3 (status quo) → verify: step output matches expected outcome
2. Sign up for competitor products (hands-on evaluation) → verify: step output matches expected outcome
3. Monitor competitor websites, pricing, messaging → verify: step output matches expected outcome
4. Analyze sales call recordings for competitor mentions → verify: step output matches expected outcome
5. Read G2/Capterra reviews (pros and cons) → verify: file content matches expected shape
6. Track competitor job postings (roadmap signals) → verify: step output matches expected outcome
7. Update battlecards monthly → verify: step output matches expected outcome
8. **Validation:** Sales team uses battlecards in 80%+ competitive deals → verify: step output matches expected outcome

### Competitive Tier Structure

| Tier | Definition | Examples |
|------|------------|----------|
| 1 | Direct competitor, same category | [Competitor A, B] |
| 2 | Adjacent solution, overlapping use case | [Alt Solution C, D] |
| 3 | Status quo (what they do today) | Spreadsheets, manual, in-house |

### Battlecard Template

```
COMPETITOR: [Name]
OVERVIEW: Founded [year], Funding [stage], Size [employees]

POSITIONING:
- They say: "[Their claim]"
- Reality: [Your assessment]

STRENGTHS:
1. [What they do well] → verify: step output matches expected outcome
2. [What they do well] → verify: step output matches expected outcome

WEAKNESSES:
1. [Where they fall short] → verify: step output matches expected outcome
2. [Where they fall short] → verify: step output matches expected outcome

OUR ADVANTAGES:
1. [Your advantage + evidence] → verify: step output matches expected outcome
2. [Your advantage + evidence] → verify: step output matches expected outcome

WHEN WE WIN:
- [Scenario where you win]

WHEN WE LOSE:
- [Scenario where they win]

TALK TRACK:
Objection: "[Common objection]"
Response: "[Your response]"
```

### Win/Loss Analysis

Track monthly:
- Win rate by competitor
- Top win reasons (product fit, ease of use, price)
- Top loss reasons (missing feature, price, relationship)
- Action items for product, sales, marketing

---

## Product Launch Planning

Plan launches by tier:

| Tier | Scope | Prep Time | Budget |
|------|-------|-----------|--------|
| 1 | New product, major feature | 6-8 weeks | $50-100k |
| 2 | Significant feature, integration | 3-4 weeks | $10-25k |
| 3 | Small improvement | 1 week | <$5k |

### Tier 1 Launch Workflow

Execute major product launch:

1. Kickoff meeting with Product, Marketing, Sales, CS → verify: step output matches expected outcome
2. Define goals (pipeline $, MQLs, press coverage) → verify: dependency resolves + import works
3. Develop positioning and messaging → verify: step output matches expected outcome
4. Create sales enablement (deck, demo, battlecard) → verify: output exists + parses without error
5. Build campaign assets (landing page, emails, ads) → verify: step output matches expected outcome
6. Train sales and CS teams → verify: step output matches expected outcome
7. Execute launch day (press, email, ads, outbound) → verify: command exit code 0
8. Monitor and optimize for 30 days → verify: step output matches expected outcome
9. **Validation:** Pipeline on track to goal by week 2 → verify: dependency resolves + import works

### Launch Day Checklist

- [ ] Press release distributed
- [ ] Email announcement sent
- [ ] Social media posts live
- [ ] Paid ads at full budget
- [ ] Sales outbound blitz launched
- [ ] In-app notification active
- [ ] Metrics monitored every 2 hours

### Launch Metrics

| Metric | Leading (Daily) | Lagging (Weekly) |
|--------|-----------------|------------------|
| Traffic | Landing page visitors | - |
| Engagement | Demo requests, signups | Feature adoption % |
| Pipeline | MQLs generated | SQLs, pipeline $ |
| Revenue | - | Deals closed, revenue |

---

## Sales Enablement

Equip sales team with PMM assets:

1. Create sales deck (15-20 slides, visual-first) → verify: output exists + parses without error
2. Build one-pagers (product, competitive, case study) → verify: step output matches expected outcome
3. Develop demo script (30-45 min with discovery) → verify: step output matches expected outcome
4. Write email templates (outreach, follow-up, closing) → verify: output exists + parses without error
5. Create ROI calculator (input costs, output savings) → verify: output exists + parses without error
6. Conduct monthly enablement calls → verify: step output matches expected outcome
7. Deliver quarterly training (positioning, competitive) → verify: step output matches expected outcome
8. **Validation:** Sales uses assets in 80%+ of opportunities → verify: step output matches expected outcome

### Sales Deck Structure

| Slide | Content |
|-------|---------|
| 1-2 | Title, agenda |
| 3-4 | Company intro, problem statement |
| 5-7 | Solution, key benefits, demo |
| 8-10 | Differentiation, case study, pricing |
| 11-12 | Implementation, support, next steps |

### Demo Flow

```
1. Intro (2 min): Who we are, agenda → verify: step output matches expected outcome
2. Discovery (5 min): Their needs, pain points → verify: step output matches expected outcome
3. Demo (20 min): Product focused on their use case → verify: step output matches expected outcome
4. Q&A (10 min): Objection handling → verify: step output matches expected outcome
5. Next steps (3 min): Trial, POC, proposal → verify: step output matches expected outcome
```

### Sales-Marketing Handoff

| Handoff | Frequency | Content |
|---------|-----------|---------|
| Weekly sync | 30 min | Win/loss, competitive, new assets |
| Monthly enablement | 60 min | Product updates, training |
| Quarterly review | Half-day | Results, strategy, planning |

---

## International Expansion

Enter new markets systematically:

1. Validate market demand (inbound leads, TAM analysis) → verify: all checks pass
2. Localize website, pricing, legal → verify: step output matches expected outcome
3. Establish sales coverage (hire or agency) → verify: step output matches expected outcome
4. Adapt messaging for cultural fit → verify: step output matches expected outcome
5. Build local partnerships and references → verify: step output matches expected outcome
6. Launch localized campaigns → verify: step output matches expected outcome
7. Monitor CAC and conversion by market → verify: step output matches expected outcome
8. **Validation:** 3+ paying customers from market in first 90 days → verify: step output matches expected outcome

### Market Priority (Series A)

| Market | Timeline | Budget % | Target ARR |
|--------|----------|----------|------------|
| US | Months 1-6 | 50% | $1M |
| UK | Months 4-9 | 20% | $500k |
| DACH | Months 7-12 | 15% | $300k |
| France | Months 10-15 | 10% | $200k |
| Canada | Months 7-12 | 5% | $100k |

### Localization Checklist

- [ ] Website translation (professional, not machine)
- [ ] Currency and pricing localized
- [ ] Local phone number and address
- [ ] Legal compliance (GDPR, PIPEDA)
- [ ] Local payment methods
- [ ] Sales coverage during local hours
- [ ] Local case studies and references

---

## Reference Documentation

### Positioning Frameworks

`references/positioning-frameworks.md` contains:

- April Dunford 5-step positioning process
- Geoffrey Moore positioning statement template
- Positioning validation interview protocol
- Competitive positioning map construction

### Launch Checklists

`references/launch-checklists.md` contains:

- Tier 1/2/3 launch checklists
- Week-by-week launch timeline
- Launch day runbook
- Post-launch metrics dashboard

### International GTM

`references/international-gtm.md` contains:

- US, UK, DACH, France, Canada playbooks
- Market-specific channel mix and messaging
- Localization requirements per market
- Entry timeline and budget allocation

### Messaging Templates

`references/messaging-templates.md` contains:

- Value proposition formulas
- Persona-specific messaging
- Competitive response scripts
- Objection handling templates
- Channel-specific copy (landing pages, emails, ads)

---

## PMM KPIs

| Metric | Target | Measurement |
|--------|--------|-------------|
| Product adoption | >40% in 90 days | Feature usage after launch |
| Win rate | >30% competitive | Deals won vs. competitors |
| Sales velocity | -20% YoY | Days from SQL to close |
| Deal size | +25% YoY | Average contract value |
| Launch pipeline | 3:1 ROMI | Pipeline $ : marketing spend |

---

## Quick Reference

### PMM Monthly Rhythm

| Week | Focus |
|------|-------|
| 1 | Review metrics, update battlecards |
| 2 | Create assets, publish content |
| 3 | Support launches, optimize campaigns |
| 4 | Monthly report, plan next month |

## Proactive Triggers

- **No documented positioning** → Without clear positioning, all marketing is guesswork.
- **Messaging differs across channels** → Inconsistent story confuses buyers.
- **No ICP defined** → Selling to everyone means selling to no one.
- **Competitor repositioning** → Market shift detected. Review your positioning.

## Output Artifacts

| When you ask for... | You get... |
|---------------------|------------|
| "Position my product" | Positioning framework (April Dunford method) with output |
| "GTM strategy" | Go-to-market plan with channels, messaging, and timeline |
| "Competitive positioning" | Positioning map with competitive gaps and opportunities |

## Communication

All output passes quality verification:
- Self-verify: source attribution, assumption audit, confidence scoring
- Output format: Bottom Line → What (with confidence) → Why → How to Act
- Results only. Every finding tagged: 🟢 verified, 🟡 medium, 🔴 assumed.

## Related Skills

- **marketing-context**: For capturing foundational positioning. PMM builds on this.
- **launch-strategy**: For executing product launches planned by PMM.
- **competitive-intel** (C-Suite): For strategic competitive intelligence.
- **cmo-advisor** (C-Suite): For marketing budget and growth model decisions.

## When NOT to use

- Task is unrelated to marketing strategy pmm — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Marketing Strategy Pmm needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for marketing strategy pmm
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving marketing strategy pmm
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
