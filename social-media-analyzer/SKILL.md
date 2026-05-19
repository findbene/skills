---
name: "social-media-analyzer"
description: "Social media campaign analysis and performance tracking. Triggers: 'use social-media-analyzer', 'social media analyzer', 'social-media-analyzer task'."
allowed-tools: Bash, Glob, Grep, Read
triggers:
  - analyze social media
  - calculate engagement rate
  - social media ROI
  - campaign performance
  - compare platforms
  - benchmark engagement
  - Instagram analytics
  - Facebook metrics
  - TikTok performance
  - LinkedIn engagement
---

# Social Media Analyzer

Campaign performance analysis with engagement metrics, ROI calculations, and platform benchmarks.

---

## Table of Contents

- [Analysis Workflow](#analysis-workflow)
- [Engagement Metrics](#engagement-metrics)
- [ROI Calculation](#roi-calculation)
- [Platform Benchmarks](#platform-benchmarks)
- [Tools](#tools)
- [Examples](#examples)

---

## Analysis Workflow

Analyze social media campaign performance:

1. Validate input data completeness (reach > 0, dates valid) → verify: step output matches expected outcome
2. Calculate engagement metrics per post → verify: step output matches expected outcome
3. Aggregate campaign-level metrics → verify: step output matches expected outcome
4. Calculate ROI if ad spend provided → verify: step output matches expected outcome
5. Compare against platform benchmarks → verify: step output matches expected outcome
6. Identify top and bottom performers → verify: step output matches expected outcome
7. Generate recommendations → verify: output file exists + no syntax error
8. **Validation:** Engagement rate < 100%, ROI matches spend data → verify: step output matches expected outcome

### Input Requirements

| Field | Required | Description |
|-------|----------|-------------|
| platform | Yes | instagram, facebook, twitter, linkedin, tiktok |
| posts[] | Yes | Array of post data |
| posts[].likes | Yes | Like/reaction count |
| posts[].comments | Yes | Comment count |
| posts[].reach | Yes | Unique users reached |
| posts[].impressions | No | Total views |
| posts[].shares | No | Share/retweet count |
| posts[].saves | No | Save/bookmark count |
| posts[].clicks | No | Link clicks |
| total_spend | No | Ad spend (for ROI) |

### Data Validation Checks

Before analysis, verify:

- [ ] Reach > 0 for all posts (avoid division by zero)
- [ ] Engagement counts are non-negative
- [ ] Date range is valid (start < end)
- [ ] Platform is recognized
- [ ] Spend > 0 if ROI requested

---

## Engagement Metrics

### Engagement Rate Calculation

```
Engagement Rate = (Likes + Comments + Shares + Saves) / Reach × 100
```

### Metric Definitions

| Metric | Formula | Interpretation |
|--------|---------|----------------|
| Engagement Rate | Engagements / Reach × 100 | Audience interaction level |
| CTR | Clicks / Impressions × 100 | Content click appeal |
| Reach Rate | Reach / Followers × 100 | Content distribution |
| Virality Rate | Shares / Impressions × 100 | Share-worthiness |
| Save Rate | Saves / Reach × 100 | Content value |

### Performance Categories

| Rating | Engagement Rate | Action |
|--------|-----------------|--------|
| Excellent | > 6% | Scale and replicate |
| Good | 3-6% | Optimize and expand |
| Average | 1-3% | Test improvements |
| Poor | < 1% | Analyze and pivot |

---

## ROI Calculation

Calculate return on ad spend:

1. Sum total engagements across posts → verify: step output matches expected outcome
2. Calculate cost per engagement (CPE) → verify: step output matches expected outcome
3. Calculate cost per click (CPC) if clicks available → verify: step output matches expected outcome
4. Estimate engagement value using benchmark rates → verify: step output matches expected outcome
5. Calculate ROI percentage → verify: step output matches expected outcome
6. **Validation:** ROI = (Value - Spend) / Spend × 100 → verify: step output matches expected outcome

### ROI Formulas

| Metric | Formula |
|--------|---------|
| Cost Per Engagement (CPE) | Total Spend / Total Engagements |
| Cost Per Click (CPC) | Total Spend / Total Clicks |
| Cost Per Thousand (CPM) | (Spend / Impressions) × 1000 |
| Return on Ad Spend (ROAS) | Revenue / Ad Spend |

### Engagement Value Estimates

| Action | Value | Rationale |
|--------|-------|-----------|
| Like | $0.50 | Brand awareness |
| Comment | $2.00 | Active engagement |
| Share | $5.00 | Amplification |
| Save | $3.00 | Intent signal |
| Click | $1.50 | Traffic value |

### ROI Interpretation

| ROI % | Rating | Recommendation |
|-------|--------|----------------|
| > 500% | Excellent | Scale budget significantly |
| 200-500% | Good | Increase budget moderately |
| 100-200% | Acceptable | Optimize before scaling |
| 0-100% | Break-even | Review targeting and creative |
| < 0% | Negative | Pause and restructure |

---

## Platform Benchmarks

### Engagement Rate by Platform

| Platform | Average | Good | Excellent |
|----------|---------|------|-----------|
| Instagram | 1.22% | 3-6% | >6% |
| Facebook | 0.07% | 0.5-1% | >1% |
| Twitter/X | 0.05% | 0.1-0.5% | >0.5% |
| LinkedIn | 2.0% | 3-5% | >5% |
| TikTok | 5.96% | 8-15% | >15% |

### CTR by Platform

| Platform | Average | Good | Excellent |
|----------|---------|------|-----------|
| Instagram | 0.22% | 0.5-1% | >1% |
| Facebook | 0.90% | 1.5-2.5% | >2.5% |
| LinkedIn | 0.44% | 1-2% | >2% |
| TikTok | 0.30% | 0.5-1% | >1% |

### CPC by Platform

| Platform | Average | Good |
|----------|---------|------|
| Facebook | $0.97 | <$0.50 |
| Instagram | $1.20 | <$0.70 |
| LinkedIn | $5.26 | <$3.00 |
| TikTok | $1.00 | <$0.50 |

See `references/platform-benchmarks.md` for complete benchmark data.

---

## Tools

### Calculate Metrics

```bash
python scripts/calculate_metrics.py assets/sample_input.json
```

Calculates engagement rate, CTR, reach rate for each post and campaign totals.

### Analyze Performance

```bash
python scripts/analyze_performance.py assets/sample_input.json
```

Generates full performance analysis with ROI, benchmarks, and recommendations.

**Output includes:**
- Campaign-level metrics
- Post-by-post breakdown
- Benchmark comparisons
- Top performers ranked
- Actionable recommendations

---

## Examples

### Sample Input

See `assets/sample_input.json`:

```json
{
  "platform": "instagram",
  "total_spend": 500,
  "posts": [
    {
      "post_id": "post_001",
      "content_type": "image",
      "likes": 342,
      "comments": 28,
      "shares": 15,
      "saves": 45,
      "reach": 5200,
      "impressions": 8500,
      "clicks": 120
    }
  ]
}
```

### Sample Output

See `assets/expected_output.json`:

```json
{
  "campaign_metrics": {
    "total_engagements": 1521,
    "avg_engagement_rate": 8.36,
    "ctr": 1.55
  },
  "roi_metrics": {
    "total_spend": 500.0,
    "cost_per_engagement": 0.33,
    "roi_percentage": 660.5
  },
  "insights": {
    "overall_health": "excellent",
    "benchmark_comparison": {
      "engagement_status": "excellent",
      "engagement_benchmark": "1.22%",
      "engagement_actual": "8.36%"
    }
  }
}
```

### Interpretation

The sample campaign shows:
- **Engagement rate 8.36%** vs 1.22% benchmark = Excellent (6.8x above average)
- **CTR 1.55%** vs 0.22% benchmark = Excellent (7x above average)
- **ROI 660%** = Outstanding return on $500 spend
- **Recommendation:** Scale budget, replicate successful elements

---

## Reference Documentation

### Platform Benchmarks

`references/platform-benchmarks.md` contains:

- Engagement rate benchmarks by platform and industry
- CTR benchmarks for organic and paid content
- Cost benchmarks (CPC, CPM, CPE)
- Content type performance by platform
- Optimal posting times and frequency
- ROI calculation formulas

## Proactive Triggers

- **Engagement rate below platform average** → Content isn't resonating. Analyze top performers for patterns.
- **Follower growth stalled** → Content distribution or frequency issue. Audit posting patterns.
- **High impressions, low engagement** → Reach without resonance. Content quality issue.
- **Competitor outperforming significantly** → Content gap. Analyze their successful posts.

## Output Artifacts

| When you ask for... | You get... |
|---------------------|------------|
| "Social media audit" | Performance analysis across platforms with benchmarks |
| "What's performing?" | Top content analysis with patterns and recommendations |
| "Competitor social analysis" | Competitive social media comparison with gaps |

## Communication

All output passes quality verification:
- Self-verify: source attribution, assumption audit, confidence scoring
- Output format: Bottom Line → What (with confidence) → Why → How to Act
- Results only. Every finding tagged: 🟢 verified, 🟡 medium, 🔴 assumed.

## Related Skills

- **social-content**: For creating social posts. Use this skill for analyzing performance.
- **campaign-analytics**: For cross-channel analytics including social.
- **content-strategy**: For planning social content themes.
- **marketing-context**: Provides audience context for better analysis.

## When NOT to use

- Task is unrelated to social media analyzer — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Social Media Analyzer needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for social media analyzer
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow
