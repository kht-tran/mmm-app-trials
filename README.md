# Marketing Mix Analysis of App Trials

## Overview
This project analyzes daily marketing and app-trial data to answer a core business question: **which media channels actually drive app trials, and how should budget be reallocated to maximize them?** 

Two complementary modeling approaches - a classical OLS marketing mix model and a Bayesian MMM (Google Meridian) - are used to estimate channel contribution, quantify diminishing returns, and produce an optimized budget allocation.

*[Completed as part of a team assignment. I led the analysis and modeling work reflected in this repository.]*

## Approach/Methods
- **OLS Marketing Mix Model**: daily model with adstock transformations, saturation curves, autoregressive lags, and an explicit TV × search interaction term to capture the "second-screen" effect (TV driving search demand).
- **Separation analysis**: two-stage modeling to disentangle overlapping/correlated channels (e.g. always-on vs. broad search).
- **Bayesian MMM extension (Google Meridian)**: re-estimates the same 10 media channels using MCMC (NUTS sampler, 2 chains, 500 adaptation + 500 burn-in + 1,000 retained samples), providing:
  - Automatic adstock decay and saturation estimation (no manual parameter tuning)
  - Full posterior uncertainty on ROI, marginal ROI, and channel contribution
  - Built-in budget optimization under a fixed-budget, spend-constrained scenario (±30% per channel)
- **Convergence diagnostics**: R-hat statistics confirm chain convergence across all model parameters.
- **Model fit**: predictive accuracy evaluated via R², MAPE, and wMAPE against observed trials.

## Key Results
- **Search Always-On** is the dominant driver of app trials in both frameworks - highest posterior ROI by a wide margin (~1 additional trial per €4 spent), with a tight credible interval reflecting strong identification.
- **CPC/CPM** is a consistent second contributor but absorbs a disproportionate share of spend (~70%+ of budget) relative to its contribution (~20%), indicating over-investment.
- **CPD** is the weakest online channel in both models: excluded from the OLS specification for coefficient instability, and independently estimated as lowest-ROI by Meridian. This cross-method convergence strengthens the case for reallocating its budget.
- **Search Broad** underperforms relative to always-on despite being activated during high-demand periods, consistent with a "second-screen" / demand-capture effect rather than incremental demand creation.
- **TV and Radio** are significant in the OLS model (via adstock + interaction terms), but their Meridian ROI estimates are not directly comparable to online channels since spend is measured in GRPs rather than currency - a data availability constraint.
- **Budget reallocation**: shifting spend from CPD toward Search Always-On and CPC/CPM (holding total budget fixed) is projected to increase incremental trials by **+14.7%** (from ~26,400 to ~30,300 trials).

## Data Note
> Data provided by the professor/university for academic use only and is not included in this repository. Code is shared to demonstrate methodology; sample/aggregated output charts and tables are included where possible in place of raw data.

## Tools Used
Python (pandas, numpy, statsmodels, matplotlib), Google Meridian (Bayesian MMM, TensorFlow Probability backend)

## Repository Contents
- `01_ols_mmm.ipynb`: data prep, adstock/saturation transforms, OLS model, separation analysis
- `02_meridian_bmm.ipynb`: InputData construction, Bayesian model fit, diagnostics, ROI/response curves, adstock decay, budget optimization
- `data_dictionary.md`: describes all variables used in the analysis 
  
  This analysis uses real experimental data provided for academic use only, so the raw dataset is not included in this repository. The notebook is shared to demonstrate methodology
