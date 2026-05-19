# Diagnostic Analysis Framework — Root Cause Analysis for Financial Variances

## Overview

A Python framework for diagnosing *why* a metric changed, not just *that* it changed. When a KPI (revenue, spend, conversion rate) moves unexpectedly, this tool systematically breaks down the variance into its root causes using decomposition, contribution analysis, and statistical methods.

Directly inspired by audit work where identifying the root cause of a financial discrepancy — not just flagging it — is the core deliverable.

## Business Context

A common scenario in data operations: the client's Q3 revenue is ₹12M but was expected to be ₹15M. A simple "revenue dropped by ₹3M" is not useful. The framework answers:
- Which categories drove the drop?
- Was it volume (fewer units), price (lower ASP), or mix (shift toward cheaper products)?
- Is this a one-month anomaly or a trend?
- Which specific SKUs or segments explain the most variance?

## Project Structure

```
project5_diagnostic_analysis/
│
├── README.md
├── diagnostic_framework.py       ← Main diagnostic engine
├── generate_performance_data.py  ← Mock KPI + actual data generator
├── requirements.txt
├── data/
│   ├── actual_performance.csv    ← Actual monthly metrics
│   └── budget_targets.csv        ← Budget/forecast targets
└── output/
    ├── variance_decomposition.csv ← Variance by driver and dimension
    ├── root_cause_summary.txt     ← Narrative root cause report
    └── charts/
        ├── waterfall_variance.png ← Waterfall chart of variance drivers
        ├── trend_vs_budget.png    ← Actual vs budget trend
        └── category_contribution.png
```

## Analyses Performed

| Analysis | Method | Output |
|---|---|---|
| Budget vs Actual variance | Simple delta + % deviation | variance_decomposition.csv |
| Volume vs Price decomposition | Price-Volume-Mix analysis | waterfall_chart |
| Category contribution | Weighted contribution % | category_contribution.png |
| Trend analysis | Rolling 3-month MA + MoM % | trend_vs_budget.png |
| Anomaly periods | Z-score on variance series | flagged in report |
| Root cause narrative | Rule-based thresholds | root_cause_summary.txt |

## Key Skills Demonstrated
- Price-Volume-Mix (PVM) decomposition — standard in management consulting & audit
- Waterfall charts — the standard visualization for variance explanation
- Rolling averages and trend analysis
- Systematic root cause logic (not just "revenue dropped")
- Documented assumptions and methodology

## How to Run

```bash
pip install -r requirements.txt
python generate_performance_data.py
python diagnostic_framework.py
```

## Assumptions & Design Decisions

1. Volume variance = (Actual Volume - Budget Volume) × Budget Price
2. Price variance = (Actual Price - Budget Price) × Actual Volume
3. Mix variance = residual after volume + price (avoids double-counting)
4. "Significant" variance defined as >10% deviation from budget (configurable)
5. Rolling average uses 3-month window — configurable for different smoothing needs
