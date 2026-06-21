# Survival Analysis for Early-Risk Prediction: A Time-to-Event Framework

**Applying Cox Proportional Hazards and Kaplan-Meier estimation to large-scale survey data (NFHS-5, n≈232,000) to identify subpopulations facing elevated risk of an adverse event within a defined time window.**

---

## Problem

Many real-world risk problems share a common structure: an entity is observed over time, an adverse event may or may not occur within that window, and observation periods end before the event happens for a large share of the population (censoring). Credit default, customer churn, insurance claims, and equipment failure all fit this pattern.

This project uses India's National Family Health Survey (NFHS-5) birth-level data to build a time-to-event model identifying which factors are associated with elevated early-life risk, using infant mortality as the event of interest. The same framework — hazard estimation under censoring — applies directly to time-to-default, time-to-churn, or time-to-claim problems.

## Why Survival Analysis (Not Logistic Regression)

A standard classification model (e.g. logistic regression) only answers "did the event happen, yes or no." It ignores *when* it happened and cannot properly handle observations where the event hadn't occurred by the end of the observation window — these get either dropped or miscoded as "no," biasing the result.

Survival analysis solves this directly:
- **Censoring** — Observations that haven't experienced the event by the data cutoff are retained, not discarded, and contribute partial information.
- **Time-to-event** — The model estimates *risk over time*, not just risk at a single endpoint, which is closer to how default, churn, or claims actually unfold.
- **Cox Proportional Hazards** — Estimates how covariates shift risk relative to a baseline hazard, without forcing a fixed functional form on the baseline itself.

## Methodology

- **Data**: NFHS-5 Birth Recode (2019–21), ~232,000 records
- **Framework**: Mosley-Chen framework used to structure variable selection across proximate and distal determinants
- **Descriptive**: Kaplan-Meier estimation to visualize survival curves across subgroups
- **Multivariate**: Cox Proportional Hazards regression to estimate adjusted hazard ratios
- **Tools**: R (survival, survminer), SPSS (data cleaning, recoding, event variable construction), LaTeX (reporting)

Full methodological detail and diagnostics are in the [dissertation paper](Dissertation.pdf).

## Key Result

[One-sentence headline finding — e.g. "Maternal education and birth interval show the strongest adjusted hazard ratios for early infant mortality risk."]

![Kaplan-Meier survival curve](images/km_curve.png)
![Hazard ratio forest plot](images/hazard_ratios.png)

## Business Translation

The censoring-aware, time-to-event approach used here transfers directly to:
- **Credit risk** — time-to-default modeling, where many accounts are "censored" (still active, haven't defaulted yet)
- **Insurance** — time-to-claim estimation
- **Customer analytics** — time-to-churn, where active customers are censored observations

Cox PH is the standard tool in all three domains for the same reason it's used here: it handles partial information without discarding it.

## What I'd Do Differently

- Test the proportional hazards assumption more rigorously (Schoenfeld residuals) rather than relying on a single diagnostic
- Explore competing risks models, since infant mortality has multiple distinct causal pathways that a single-event Cox model collapses together
- Validate hazard ratios against an out-of-sample split rather than the full dataset

---

**Tech stack**: R · SPSS · LaTeX
**Author**: Harikrishna MS
