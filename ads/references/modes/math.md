# Mode: math — PPC Financial Calculator & Modeling

PPC financial calculator and modeling tool. CPA, ROAS, CPL calculations, break-even analysis, impression share opportunity sizing, budget forecasting.

## Process

1. Ask the user what calculation they need (or detect from context)
2. Collect required inputs (from pasted data, exports, or verbal description)
3. Perform calculations with clear formulas shown
4. Present results with interpretation and recommendations
5. Flag any concerning metrics or benchmarks

## When NOT to use

- Audit of a specific platform — use `ads-google`, `ads-meta`, `ads-tiktok`, `ads-linkedin`, etc.
- Creative concepting or copy — use `ads-create` or `ads-creative`
- Campaign-level optimization tactics — use `ads-budget` or the relevant platform audit
- Statistical A/B test math (sample size, MDE) — use `statistical-analyst` or `ads-test`
- Non-ad LTV/CAC modeling without ad spend context — use `financial-analyst`

## Red Flags (math-specific)

| Thought | Reality |
|---------|---------|
| "Scale budget 200% next week, the math works" | Algorithm learning resets and CPA blows up; 20% step is the rule |
| "ROAS is 4x, we're fine" | Without break-even ROAS (1/margin), ROAS alone is meaningless |
| "LTV:CAC of 8:1 is great, push more spend" | Often signals under-investment in growth, not health — diagnose, don't celebrate |
| "Use last-click attribution for the whole funnel" | Misses incremental contribution; for >$5K/mo accounts use incrementality testing |

## Output Contract (math-specific)

Done when:
- Calculator(s) selected from CPA, ROAS, Break-Even, IS Opportunity, Forecast, LTV:CAC, MER
- All required inputs gathered (asked if missing — no invented numbers)
- Formulas shown with substituted values
- Result table with metric / value / benchmark / PASS|WARNING|FAIL
- Interpretation in 1-2 sentences referencing benchmarks
- Actionable recommendation tied to the result
- Diminishing-returns or 20% scaling caveat applied where relevant

## Examples

### Example 1 — Break-even ROAS check
- Input: "We're hitting 3x ROAS on Meta, spending $10K/mo, AOV $80, 40% margin"
- Action: Break-even ROAS = 1/0.40 = 2.5x → current 3x ROAS is 20% above break-even; recommend modest scale (+20%) and monitor
- Output: Table with ROAS=3.0x, BE-ROAS=2.5x, status PASS, recommendation: scale +20% week-over-week, watch CPA drift

### Example 2 — Impression share opportunity
- Input: "Google Search account: 60% IS, IS lost to budget = 30%, IS lost to rank = 10%, $5K/mo, 200 conv/mo"
- Action: Revenue opportunity ≈ Current × (1/0.6 − 1) = 67%; majority of loss is budget; recommend +50% budget split tested in 20% increments
- Output: Estimated +133 conversions at full IS, budget required ~$8.3K, priority = budget over bids
