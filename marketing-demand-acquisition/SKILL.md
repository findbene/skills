---
name: "marketing-demand-acquisition"
description: "Creates demand generation campaigns, optimizes paid ad spend across LinkedIn, Google,. Triggers: 'use marketing-demand-acquisition', 'marketing demand acquisition', 'marketing-demand-acquisition task."
allowed-tools: Glob, Grep, Read
triggers:
  - demand gen
  - demand generation
  - paid ads
  - paid media
  - LinkedIn ads
  - Google ads
  - Meta ads
  - CAC
  - customer acquisition cost
  - lead generation
  - MQL
  - SQL
  - pipeline generation
  - acquisition strategy
  - HubSpot campaigns
metadata:
  version: 1.1.0
  author: Alireza Rezvani
  category: marketing
  domain: demand-generation
  updated: 2025-01
---

# Marketing Demand & Acquisition

Acquisition playbook for Series A+ startups scaling internationally (EU/US/Canada) with hybrid PLG/Sales-Led motion.

## Table of Contents

- [Core KPIs](#core-kpis)
- [Demand Generation Framework](#demand-generation-framework)
- [Paid Media Channels](#paid-media-channels)
- [SEO Strategy](#seo-strategy)
- [Partnerships](#partnerships)
- [Attribution](#attribution)
- [Tools](#tools)
- [References](#references)

---

## Core KPIs

**Demand Gen:** MQL/SQL volume, cost per opportunity, marketing-sourced pipeline $, MQL→SQL rate

**Paid Media:** CAC, ROAS, CPL, CPA, channel efficiency ratio

**SEO:** Organic sessions, non-brand traffic %, keyword rankings, technical health score

**Partnerships:** Partner-sourced pipeline $, partner CAC, co-marketing ROI

---

## Demand Generation Framework

### Funnel Stages

| Stage | Tactics | Target |
|-------|---------|--------|
| TOFU | Paid social, display, content syndication, SEO | Brand awareness, traffic |
| MOFU | Paid search, retargeting, gated content, email nurture | MQLs, demo requests |
| BOFU | Brand search, direct outreach, case studies, trials | SQLs, pipeline $ |

### Campaign Planning Workflow

1. Define objective, budget, duration, audience → verify: step output matches expected outcome
2. Select channels based on funnel stage → verify: step output matches expected outcome
3. Create campaign in HubSpot with proper UTM structure → verify: output exists + parses without error
4. Configure lead scoring and assignment rules → verify: step output matches expected outcome
5. Launch with test budget, validate tracking → verify: all checks pass
6. **Validation:** UTM parameters appear in HubSpot contact records → verify: step output matches expected outcome

### UTM Structure

```
utm_source={channel}       // linkedin, google, meta
utm_medium={type}          // cpc, display, email
utm_campaign={campaign-id} // q1-2025-linkedin-enterprise
utm_content={variant}      // ad-a, email-1
utm_term={keyword}         // [paid search only]
```

---

## Paid Media Channels

### Channel Selection Matrix

| Channel | Best For | CAC Range | Series A Priority |
|---------|----------|-----------|-------------------|
| LinkedIn Ads | B2B, Enterprise, ABM | $150-400 | High |
| Google Search | High-intent, BOFU | $80-250 | High |
| Google Display | Retargeting | $50-150 | Medium |
| Meta Ads | SMB, visual products | $60-200 | Medium |

### LinkedIn Ads Setup

1. Create campaign group for initiative → verify: output exists + parses without error
2. Structure: Awareness → Consideration → Conversion campaigns
3. Target: Director+, 50-5000 employees, relevant industries → verify: step output matches expected outcome
4. Start $50/day per campaign → verify: step output matches expected outcome
5. Scale 20% weekly if CAC < target → verify: step output matches expected outcome
6. **Validation:** LinkedIn Insight Tag firing on all pages → verify: step output matches expected outcome

### Google Ads Setup

1. Prioritize: Brand → Competitor → Solution → Category keywords
2. Structure ad groups with 5-10 tightly themed keywords → verify: step output matches expected outcome
3. Create 3 responsive search ads per ad group (15 headlines, 4 descriptions) → verify: output exists + parses without error
4. Maintain negative keyword list (100+) → verify: step output matches expected outcome
5. Start Manual CPC, switch to Target CPA after 50+ conversions → verify: step output matches expected outcome
6. **Validation:** Conversion tracking firing, search terms reviewed weekly → verify: step output matches expected outcome

### Budget Allocation (Series A, $40k/month)

| Channel | Budget | Expected SQLs |
|---------|--------|---------------|
| LinkedIn | $15k | 10 |
| Google Search | $12k | 20 |
| Google Display | $5k | 5 |
| Meta | $5k | 8 |
| Partnerships | $3k | 5 |

See [campaign-templates.md](references/campaign-templates.md) for detailed structures.

---

## SEO Strategy

### Technical Foundation Checklist

- [ ] XML sitemap submitted to Search Console
- [ ] Robots.txt configured correctly
- [ ] HTTPS enabled
- [ ] Page speed >90 mobile
- [ ] Core Web Vitals passing
- [ ] Structured data implemented
- [ ] Canonical tags on all pages
- [ ] Hreflang tags for international
- **Validation:** Run Screaming Frog crawl, zero critical errors

### Keyword Strategy

| Tier | Type | Volume | Priority |
|------|------|--------|----------|
| 1 | High-intent BOFU | 100-1k | First |
| 2 | Solution-aware MOFU | 500-5k | Second |
| 3 | Problem-aware TOFU | 1k-10k | Third |

### On-Page Optimization

1. URL: Include primary keyword, 3-5 words → verify: step output matches expected outcome
2. Title tag: Primary keyword + brand (60 chars) → verify: step output matches expected outcome
3. Meta description: CTA + value prop (155 chars) → verify: step output matches expected outcome
4. H1: Match search intent (one per page) → verify: step output matches expected outcome
5. Content: 2000-3000 words for comprehensive topics → verify: step output matches expected outcome
6. Internal links: 3-5 relevant pages → verify: step output matches expected outcome
7. **Validation:** Google Search Console shows page indexed, no errors → verify: step output matches expected outcome

### Link Building Priorities

1. Digital PR (original research, industry reports) → verify: step output matches expected outcome
2. Guest posting (DA 40+ sites only) → verify: step output matches expected outcome
3. Partner co-marketing (complementary SaaS) → verify: step output matches expected outcome
4. Community engagement (Reddit, Quora) → verify: step output matches expected outcome

---

## Partnerships

### Partnership Tiers

| Tier | Type | Effort | ROI |
|------|------|--------|-----|
| 1 | Strategic integrations | High | Very high |
| 2 | Affiliate partners | Medium | Medium-high |
| 3 | Customer referrals | Low | Medium |
| 4 | Marketplace listings | Medium | Low-medium |

### Partnership Workflow

1. Identify partners with overlapping ICP, no competition → verify: step output matches expected outcome
2. Outreach with specific integration/co-marketing proposal → verify: step output matches expected outcome
3. Define success metrics, revenue model, term → verify: step output matches expected outcome
4. Create co-branded assets and partner tracking → verify: output exists + parses without error
5. Enable partner sales team with demo training → verify: step output matches expected outcome
6. **Validation:** Partner UTM tracking functional, leads routing correctly → verify: step output matches expected outcome

### Affiliate Program Setup

1. Select platform (PartnerStack, Impact, Rewardful) → verify: step output matches expected outcome
2. Configure commission structure (20-30% recurring) → verify: step output matches expected outcome
3. Create affiliate enablement kit (assets, links, content) → verify: output exists + parses without error
4. Recruit through outbound, inbound, events → verify: step output matches expected outcome
5. **Validation:** Test affiliate link tracks through to conversion → verify: all checks pass

See [international-playbooks.md](references/international-playbooks.md) for regional tactics.

---

## Attribution

### Model Selection

| Model | Use Case |
|-------|----------|
| First-Touch | Awareness campaigns |
| Last-Touch | Direct response |
| W-Shaped (40-20-40) | Hybrid PLG/Sales (recommended) |

### HubSpot Attribution Setup

1. Navigate to Marketing → Reports → Attribution
2. Select W-Shaped model for hybrid motion → verify: step output matches expected outcome
3. Define conversion event (deal created) → verify: output exists + parses without error
4. Set 90-day lookback window → verify: step output matches expected outcome
5. **Validation:** Run report for past 90 days, all channels show data → verify: command exit code 0

### Weekly Metrics Dashboard

| Metric | Target |
|--------|--------|
| MQLs | Weekly target |
| SQLs | Weekly target |
| MQL→SQL Rate | >15% |
| Blended CAC | <$300 |
| Pipeline Velocity | <60 days |

See [attribution-guide.md](references/attribution-guide.md) for detailed setup.

---

## Tools

### scripts/

| Script | Purpose | Usage |
|--------|---------|-------|
| `calculate_cac.py` | Calculate blended and channel CAC | `python scripts/calculate_cac.py --spend 40000 --customers 50` |

### HubSpot Integration

- Campaign tracking with UTM parameters
- Lead scoring and MQL/SQL workflows
- Attribution reporting (multi-touch)
- Partner lead routing

See [hubspot-workflows.md](references/hubspot-workflows.md) for workflow templates.

---

## References

| File | Content |
|------|---------|
| [hubspot-workflows.md](references/hubspot-workflows.md) | Lead scoring, nurture, assignment workflows |
| [campaign-templates.md](references/campaign-templates.md) | LinkedIn, Google, Meta campaign structures |
| [international-playbooks.md](references/international-playbooks.md) | EU, US, Canada market tactics |
| [attribution-guide.md](references/attribution-guide.md) | Multi-touch attribution, dashboards, A/B testing |

---

## Channel Benchmarks (B2B SaaS Series A)

| Metric | LinkedIn | Google Search | SEO | Email |
|--------|----------|---------------|-----|-------|
| CTR | 0.4-0.9% | 2-5% | 1-3% | 15-25% |
| CVR | 1-3% | 3-7% | 2-5% | 2-5% |
| CAC | $150-400 | $80-250 | $50-150 | $20-80 |
| MQL→SQL | 10-20% | 15-25% | 12-22% | 8-15% |

---

## MQL→SQL Handoff

### SQL Criteria

```
Required:
✅ Job title: Director+ or budget authority
✅ Company size: 50-5000 employees
✅ Budget: $10k+ annual
✅ Timeline: Buying within 90 days
✅ Engagement: Demo requested or high-intent action
```

### SLA

| Handoff | Target |
|---------|--------|
| SDR responds to MQL | 4 hours |
| AE books demo with SQL | 24 hours |
| First demo scheduled | 3 business days |

**Validation:** Test lead through workflow, verify notifications and routing.

## Proactive Triggers

- **Over-relying on one channel** → Single-channel dependency is a business risk. Diversify.
- **No lead scoring** → Not all leads are equal. Route to revenue-operations for scoring.
- **CAC exceeding LTV** → Demand gen is unprofitable. Optimize or cut channels.
- **No nurture for non-ready leads** → 80% of leads aren't ready to buy. Nurture converts them later.

## Related Skills

- **paid-ads**: For executing paid acquisition campaigns.
- **content-strategy**: For content-driven demand generation.
- **email-sequence**: For nurture sequences in the demand funnel.
- **campaign-analytics**: For measuring demand gen effectiveness.

## When NOT to use

- Task is unrelated to marketing demand acquisition — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Marketing Demand Acquisition needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for marketing demand acquisition
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving marketing demand acquisition
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
