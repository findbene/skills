---
name: "financial-analyst"
description: "Performs financial ratio analysis, DCF valuation, budget variance analysis, and rolling forecast construction for stra. Triggers: 'use financial-analyst', 'financial analyst', 'financial-analyst task."
allowed-tools: Bash, Glob, Grep, Read
---

# Financial Analyst Skill

## Overview

Production-ready financial analysis toolkit providing ratio analysis, DCF valuation, budget variance analysis, and rolling forecast construction. Designed for financial modeling, forecasting & budgeting, management reporting, business performance analysis, and investment analysis.

## 5-Phase Workflow

### Phase 1: Scoping
- Define analysis objectives and stakeholder requirements
- Identify data sources and time periods
- Establish materiality thresholds and accuracy targets
- Select appropriate analytical frameworks

### Phase 2: Data Analysis & Modeling
- Collect and validate financial data (income statement, balance sheet, cash flow)
- **Validate input data completeness** before running ratio calculations (check for missing fields, nulls, or implausible values)
- Calculate financial ratios across 5 categories (profitability, liquidity, leverage, efficiency, valuation)
- Build DCF models with WACC and terminal value calculations; **cross-check DCF outputs against sanity bounds** (e.g., implied multiples vs. comparables)
- Construct budget variance analyses with favorable/unfavorable classification
- Develop driver-based forecasts with scenario modeling

### Phase 3: Insight Generation
- Interpret ratio trends and benchmark against industry standards
- Identify material variances and root causes
- Assess valuation ranges through sensitivity analysis
- Evaluate forecast scenarios (base/bull/bear) for decision support

### Phase 4: Reporting
- Generate executive summaries with key findings
- Produce detailed variance reports by department and category
- Deliver DCF valuation reports with sensitivity tables
- Present rolling forecasts with trend analysis

### Phase 5: Follow-up
- Track forecast accuracy (target: +/-5% revenue, +/-3% expenses)
- Monitor report delivery timeliness (target: 100% on time)
- Update models with actuals as they become available
- Refine assumptions based on variance analysis

## Tools

### 1. Ratio Calculator (`scripts/ratio_calculator.py`)

Calculate and interpret financial ratios from financial statement data.

**Ratio Categories:**
- **Profitability:** ROE, ROA, Gross Margin, Operating Margin, Net Margin
- **Liquidity:** Current Ratio, Quick Ratio, Cash Ratio
- **Leverage:** Debt-to-Equity, Interest Coverage, DSCR
- **Efficiency:** Asset Turnover, Inventory Turnover, Receivables Turnover, DSO
- **Valuation:** P/E, P/B, P/S, EV/EBITDA, PEG Ratio

```bash
python scripts/ratio_calculator.py sample_financial_data.json
python scripts/ratio_calculator.py sample_financial_data.json --format json
python scripts/ratio_calculator.py sample_financial_data.json --category profitability
```

### 2. DCF Valuation (`scripts/dcf_valuation.py`)

Discounted Cash Flow enterprise and equity valuation with sensitivity analysis.

**Features:**
- WACC calculation via CAPM
- Revenue and free cash flow projections (5-year default)
- Terminal value via perpetuity growth and exit multiple methods
- Enterprise value and equity value derivation
- Two-way sensitivity analysis (discount rate vs growth rate)

```bash
python scripts/dcf_valuation.py valuation_data.json
python scripts/dcf_valuation.py valuation_data.json --format json
python scripts/dcf_valuation.py valuation_data.json --projection-years 7
```

### 3. Budget Variance Analyzer (`scripts/budget_variance_analyzer.py`)

Analyze actual vs budget vs prior year performance with materiality filtering.

**Features:**
- Dollar and percentage variance calculation
- Materiality threshold filtering (default: 10% or $50K)
- Favorable/unfavorable classification with revenue/expense logic
- Department and category breakdown
- Executive summary generation

```bash
python scripts/budget_variance_analyzer.py budget_data.json
python scripts/budget_variance_analyzer.py budget_data.json --format json
python scripts/budget_variance_analyzer.py budget_data.json --threshold-pct 5 --threshold-amt 25000
```

### 4. Forecast Builder (`scripts/forecast_builder.py`)

Driver-based revenue forecasting with rolling cash flow projection and scenario modeling.

**Features:**
- Driver-based revenue forecast model
- 13-week rolling cash flow projection
- Scenario modeling (base/bull/bear cases)
- Trend analysis using simple linear regression (standard library)

```bash
python scripts/forecast_builder.py forecast_data.json
python scripts/forecast_builder.py forecast_data.json --format json
python scripts/forecast_builder.py forecast_data.json --scenarios base,bull,bear
```

## Knowledge Bases

| Reference | Purpose |
|-----------|---------|
| `references/financial-ratios-guide.md` | Ratio formulas, interpretation, industry benchmarks |
| `references/valuation-methodology.md` | DCF methodology, WACC, terminal value, comps |
| `references/forecasting-best-practices.md` | Driver-based forecasting, rolling forecasts, accuracy |
| `references/industry-adaptations.md` | Sector-specific metrics and considerations (SaaS, Retail, Manufacturing, Financial Services, Healthcare) |

## Templates

| Template | Purpose |
|----------|---------|
| `assets/variance_report_template.md` | Budget variance report template |
| `assets/dcf_analysis_template.md` | DCF valuation analysis template |
| `assets/forecast_report_template.md` | Revenue forecast report template |

## Key Metrics & Targets

| Metric | Target |
|--------|--------|
| Forecast accuracy (revenue) | +/-5% |
| Forecast accuracy (expenses) | +/-3% |
| Report delivery | 100% on time |
| Model documentation | Complete for all assumptions |
| Variance explanation | 100% of material variances |

## Input Data Format

All scripts accept JSON input files. See `assets/sample_financial_data.json` for the complete input schema covering all four tools.

## Dependencies

**None** - All scripts use Python standard library only (`math`, `statistics`, `json`, `argparse`, `datetime`). No numpy, pandas, or scipy required.

## When NOT to use

- Ad-spend math (CPA / ROAS / break-even ROAS) — use `ads-math`
- Pricing-model design — use `pricing-strategy`
- Pure accounting / bookkeeping operations — out of scope
- Board narrative or investor storytelling — use `board-deck-builder` or `pitch-deck`
- SaaS-specific NRR / churn analysis — use `saas-metrics-coach` or `revenue-operations`

## Red Flags

| Thought | Reality |
|---------|---------|
| "Use last quarter's revenue ratio as base case" | Without validating inputs (nulls, implausible values), garbage downstream |
| "Pick a single WACC" | Run sensitivity — single-point DCF is misleading |
| "Show DCF result without comparables check" | Sanity-bound against implied multiples; lone DCF often unrealistic |
| "Variances under 10% don't need explanation" | All material variances need a root cause; 'material' depends on context, not 10% |

## Output Contract

Done when:
- Input data validated (no nulls, plausibility checks per statement)
- Ratios computed across 5 categories (profitability, liquidity, leverage, efficiency, valuation)
- DCF (if used) includes WACC, terminal value, and sensitivity table; cross-checked vs comparables
- Variance analysis classifies favorable/unfavorable with root causes for material items
- Rolling forecast includes base/bull/bear scenarios
- Outputs saved with all assumptions documented
- Forecast accuracy tracked vs ±5% revenue / ±3% expense targets

## Examples

### Example 1 — Investment screen via ratio analysis
- Input: "Analyze Company X's last 4 quarters for an investment decision"
- Action: Validate IS/BS/CF inputs, compute the 5 ratio categories, benchmark vs industry, flag liquidity or leverage outliers, summarize verdict
- Output: Ratio dashboard JSON + 1-page brief with PASS/WARN/FAIL per category and named risks

### Example 2 — Annual budget vs actual variance
- Input: "We're $1.2M over plan, need a variance report"
- Action: Variance by department and category, classify favorable/unfavorable, root-cause for items >5% deviation, recommend reforecast items
- Output: Variance report + reforecast deltas + watch-list for next quarter
