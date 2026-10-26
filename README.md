# Customer Retention: Survival Analysis of Churn

**[Live report →](https://twinsilver.github.io/customer-churn-survival/)**

Survival analysis of telecom customer churn: Kaplan-Meier curves, a Cox Proportional Hazards model with assumption checks, and a Random Survival Forest as a flexible alternative. The question is which factors speed up churn, and whether the more flexible model earns its complexity.

## Data

IBM Telco Customer Churn sample ([Kaggle](https://www.kaggle.com/datasets/blastchar/telco-customer-churn)): 7,043 customers of a fictional telecom. 11 customers with tenure = 0 were dropped (no time at risk), leaving 7,032. About 27% churned; the rest are right-censored.

## Methods

- **Predictor screening:** `TotalCharges` excluded (encodes follow-up time); `MonthlyCharges` excluded (VIF ≈ 864, 99.9% explained by service columns).
- **Kaplan-Meier** curves by contract type, with log-rank test.
- **Cox PH** with proportional hazards check (Schoenfeld residuals) and a Contract-stratified refit.
- **Random Survival Forest** with permutation variable importance.
- **Comparison** by concordance index (C-index).

## Key findings

- **Contract length dominates:** one-year HR 0.20, two-year HR 0.04 vs. month-to-month.
- **Internet service:** fiber optic HR 1.47 vs. DSL; no internet HR 0.33.
- **Payment method:** electronic and mailed check carry about 1.8x the hazard of automatic bank transfer.
- **Add-ons** (online security, online backup, tech support) are associated with 34-48% lower hazard.
- **Model comparison:** Cox C-index 0.866 (full sample) vs. RSF 0.842 (out-of-bag). The two are computed differently, so the gap is suggestive, not conclusive.
- Contract violates proportional hazards after about month 30; the stratified refit leaves other hazard ratios nearly unchanged.

## Limitations

- The data is a one-month snapshot of a fictional telco, so absolute survival probabilities are illustrative; relative differences are the focus.
- Results are associations, not causal effects.
- The Cox C-index is in-sample; no held-out evaluation was done.

## Repository

```
customer-retention-survival.qmd   # source (code + writeup)
index.html                        # rendered report (GitHub Pages)
data/WA_Fn-UseC_-Telco-Customer-Churn.csv
```

## Run locally

Requires R and [Quarto](https://quarto.org).

```r
install.packages(c("tidyverse", "survival", "survminer",
                   "randomForestSRC", "ggRandomForests", "knitr", "car"))
```

```
quarto render customer-retention-survival.qmd --output index.html
```
