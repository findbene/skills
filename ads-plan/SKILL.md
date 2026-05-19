---
name: ads-plan
description: "Strategic paid advertising planning with industry-specific templates. Covers platform selection, campaign architecture, budget planning, creative. Triggers: 'use ads-plan', 'run ads plan', 'ads plan'."
allowed-tools: Glob, Grep, Read
user-invokable: false
---

# Strategic Paid Advertising Plan

## Process

### 1. Discovery
- Business type, products/services, target audience
- Current advertising status (active platforms, spend, performance)
- Goals: brand awareness, lead generation, e-commerce sales, app installs
- Budget range (monthly/quarterly)
- Timeline and urgency
- In-house team capacity vs agency needs

### 2. Competitive Analysis
- Identify top 3-5 competitors
- Analyze their ad presence across platforms (Google Ads Transparency, Meta Ad Library)
- Estimate competitor spend levels and platform mix
- Identify messaging themes and creative approaches
- Note keyword/audience gaps (opportunities competitors are missing)

### 3. Platform Selection
- Load industry template from `assets/` directory
- Match business type to recommended platform mix
- Read `ads/references/budget-allocation.md` for platform selection matrix
- Read `ads/references/conversion-tracking.md` for tracking setup requirements
- Assess platform fit based on:
  - Target audience demographics per platform
  - Product/service type suitability
  - Budget requirements per platform (minimums)
  - Sales cycle length and attribution needs
  - Creative capabilities and content availability

### 4. Campaign Architecture

#### Naming Convention
```
[Platform]_[Objective]_[Audience]_[Geo]_[Date]
```
Example: `META_CONV_Prospecting_US_2026Q1`

#### Campaign Structure Template
```
Account
├── Brand Campaign (always-on, protect brand terms)
├── Non-Brand Prospecting
│   ├── Campaign 1: [Top Funnel - Awareness]
│   │   ├── Ad Group/Set 1: [Audience A]
│   │   └── Ad Group/Set 2: [Audience B]
│   ├── Campaign 2: [Mid Funnel - Consideration]
│   │   ├── Ad Group/Set 1: [Interest-based]
│   │   └── Ad Group/Set 2: [Lookalike/Similar]
│   └── Campaign 3: [Bottom Funnel - Conversion]
│       ├── Ad Group/Set 1: [High-intent]
│       └── Ad Group/Set 2: [Custom audience]
├── Retargeting
│   ├── Website Visitors (7-30 days)
│   ├── Engaged Users (video views, social engagement)
│   └── Cart Abandoners / Form Starters
└── Testing
    └── New audiences, formats, or messaging
```

### 5. Budget Planning

#### Monthly Budget Distribution
Read `ads/references/budget-allocation.md` for the 70/20/10 framework.

| Tier | Allocation | Purpose |
|------|-----------|---------|
| Proven (70%) | Primary platforms with proven ROI | Revenue engine |
| Scaling (20%) | Platforms showing promise | Growth engine |
| Testing (10%) | New platforms or strategies | Innovation |

#### Budget Pacing
- Month 1-2: heavy testing, expect higher CPA (learning)
- Month 3-4: optimize based on data, tighten targeting
- Month 5-6: scale winners, kill losers, expand
- Ongoing: 70/20/10 maintenance with quarterly reviews

### 6. Creative Strategy

#### Content Pillars
- **Pain Point**: address specific problems your audience faces
- **Social Proof**: testimonials, case studies, reviews
- **Product Demo**: show the product/service in action
- **Offer**: promotions, free trials, lead magnets
- **Education**: teach something valuable related to your product

#### Creative Production Plan
| Priority | Asset Type | Platforms | Quantity |
|----------|-----------|-----------|----------|
| P1 | Product/service videos (15-30s) | Meta, TikTok, YouTube | 5-10 |
| P2 | Static images with copy | Google, Meta, LinkedIn | 10-15 |
| P3 | Carousel/collection | Meta, LinkedIn | 3-5 |
| P4 | UGC/testimonial video | TikTok, Meta | 3-5 |
| P5 | Long-form video (1-3 min) | YouTube | 2-3 |

### 7. Tracking Setup Plan

Before launching any ads, ensure tracking is configured:

| Platform | Client-Side | Server-Side | Priority |
|----------|------------|-------------|----------|
| Google | gtag.js | Enhanced Conversions, GTM SS | P1 |
| Meta | Pixel | CAPI | P1 |
| LinkedIn | Insight Tag | CAPI (2025) | P2 |
| TikTok | Pixel | Events API + ttclid | P2 |
| Microsoft | UET Tag | Enhanced Conversions | P2 |

### 8. Implementation Roadmap

#### Phase 1: Foundation (Weeks 1-2)
- Install all tracking pixels and server-side tracking
- Set up conversion events and goals
- Create campaign structure and naming conventions
- Build initial audiences (custom, lookalike/predictive)
- Produce first batch of creative assets

#### Phase 2: Launch (Weeks 3-4)
- Launch campaigns on primary platform(s) first
- Set conservative budgets and bidding (Maximize Clicks / Lowest Cost)
- Monitor daily for the first 7 days
- Verify conversion tracking is firing correctly

#### Phase 3: Optimize (Weeks 5-8)
- Analyze initial data (minimum 2 weeks of data)
- Adjust bidding strategies based on conversion volume
- Kill underperforming ad groups/creatives (3x Kill Rule)
- Launch secondary platforms
- Begin A/B testing (creative, landing pages, audiences)

#### Phase 4: Scale (Weeks 9-12)
- Scale winning campaigns (20% rule)
- Expand to testing platforms (10% budget)
- Implement advanced strategies (ABM, Shopping feeds, Smart+)
- Monthly performance reviews

## When NOT to use

- Task is unrelated to ads plan — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Ads Plan needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for ads plan
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving ads plan
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken

## References

Extended sections moved to `references/details.md`.
