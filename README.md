# Advance Categorical Data Analysis – GDT600

![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)
![R ≥ 4.0](https://img.shields.io/badge/R-≥%204.0-blue.svg)
![Quarto ≥ 1.4](https://img.shields.io/badge/Quarto-≥%201.4-orange.svg)
![IDE: Positron](https://img.shields.io/badge/IDE-Positron-blueviolet.svg)
![GitHub stars](https://img.shields.io/github/stars/DrKangMunir/advance-categorical-analysis?style=social)
![GitHub forks](https://img.shields.io/github/forks/DrKangMunir/advance-categorical-analysis?style=social)

Reproducible **multinomial** and **ordinal logistic regression** analyses for the Advanced Categorical Data Analysis course (DrPH, Universiti Sains Malaysia).

---

<details>
<summary>📑 Table of Contents</summary>

- [Overview](#overview)
- [Contributor](#contributor)
- [Folder Structure](#folder-structure)
- [Installation](#installation)
- [Usage](#usage)
- [Data](#data)
- [List of Analyses](#list-of-analyses)
- [Results](#results)
- [Disclaimer](#disclaimer)
- [License](#license)
- [Version History](#version-history)

</details>

---

## Overview

This repository contains two individual assignment submissions for GDT600 Advanced Categorical Data Analysis:

| # | Analysis | Dataset | Method |
|---|----------|---------|--------|
| 1 | Contraceptive method choice | 1987 National Indonesia Contraceptive Prevalence Survey (n = 1,473) | Multinomial logistic regression (`VGAM::vglm`) |
| 2 | Diabetes severity | CDC BRFSS 2015 — stratified sample (n = 2,200) | Ordinal logistic regression / proportional odds model (`ordinal::clm`) |

Both analyses use **R**, **Quarto**, and **tidyverse-based workflows** covering data preparation, exploratory data analysis, model building, diagnostics, model fit evaluation, and prediction.

---

## Contributor

| Name                             | Student ID | Role                     |
|----------------------------------|------------|--------------------------|
| Dr. Nazirul Munir Bin Abu Hassan | 23202537   | Analysis & Documentation |

---

## Folder Structure

```
├── Multinomial-Logistic-Regression/
│   ├── cmc_multinomial_analysis.qmd
│   ├── cmc_multinomial_analysis.html
│   └── cmc.csv
├── Ordinal-Logistic-Regression/
│   ├── ordinal_logistic_regression_diabetes.qmd
│   ├── ordinal_logistic_regression_diabetes.html
│   └── diabetes_sample_2200.csv
└── README.md
```

- **`Multinomial-Logistic-Regression/`** — multinomial logistic regression on contraceptive method choice
  - `cmc_multinomial_analysis.qmd` — Quarto source (fully reproducible)
  - `cmc_multinomial_analysis.html` — rendered HTML report
  - `cmc.csv` — 1987 Indonesia Contraceptive Prevalence Survey data (n = 1,473)

- **`Ordinal-Logistic-Regression/`** — proportional odds model on diabetes severity
  - `ordinal_logistic_regression_diabetes.qmd` — Quarto source (fully reproducible)
  - `ordinal_logistic_regression_diabetes.html` — rendered HTML report
  - `diabetes_sample_2200.csv` — stratified sample from CDC BRFSS 2015 (n = 2,200)

- `README.md` — this overview file

---

## Installation

### Prerequisites

- R (≥ 4.0.0) and [Positron](https://positron.posit.co/) (recommended IDE)
- Quarto (≥ 1.4)

### Required R Packages

**Multinomial Logistic Regression:**

```r
install.packages(c(
  "tidyverse", "gtsummary", "VGAM", "nnet",
  "knitr", "kableExtra", "here"
))
```

**Ordinal Logistic Regression:**

```r
install.packages(c(
  "tidyverse", "ordinal", "gtsummary", "broom",
  "knitr", "kableExtra", "car"
))
```

### Setup

```bash
git clone https://github.com/DrKangMunir/advance-categorical-analysis.git
cd advance-categorical-analysis
```

---

## Usage

### 1. Multinomial Logistic Regression

```bash
cd Multinomial-Logistic-Regression
quarto render cmc_multinomial_analysis.qmd
```

### 2. Ordinal Logistic Regression

```bash
cd Ordinal-Logistic-Regression
quarto render ordinal_logistic_regression_diabetes.qmd
```

Alternatively, open either `.qmd` file in **Positron** and click **Render**.

---

## Data

### 1. 1987 National Indonesia Contraceptive Prevalence Survey

- **File:** `Multinomial-Logistic-Regression/cmc.csv`
- **Source:** UCI Machine Learning Repository (DOI: [10.24432/C5C59T](https://archive.ics.uci.edu/ml/datasets/Contraceptive+Method+Choice))
- **Sample size:** n = 1,473 married non-pregnant women

| Variable | Type | Description |
|----------|------|-------------|
| `contraceptive_method` | Nominal (1/2/3) | Outcome: No use / Long-term / Short-term |
| `wife_age` | Continuous | Wife's age in years |
| `wife_edu` | Ordinal (1–4) | Wife's education level |
| `husb_edu` | Ordinal (1–4) | Husband's education level |
| `num_children` | Continuous | Number of children ever born |
| `wife_religion` | Binary | Religion: 0 = Non-Islam, 1 = Islam |
| `wife_working` | Binary | Working status: 0 = Yes, 1 = No |
| `husb_occupation` | Nominal (1–4) | Husband's occupation |
| `sol_index` | Ordinal (1–4) | Standard of living index |
| `media_exposure` | Binary | Media exposure: 0 = Good, 1 = Not good |

### 2. CDC BRFSS 2015 — Stratified Sample

- **File:** `Ordinal-Logistic-Regression/diabetes_sample_2200.csv`
- **Source:** CDC BRFSS 2015 / UCI ML Repository (DOI: [10.24432/C53919](https://doi.org/10.24432/C53919))
- **Sample size:** n = 2,200 (stratified; Pre-Diabetes oversampled)

| Category | n | % in sample |
|----------|---|-------------|
| No Diabetes | 1,600 | 72.7% |
| Pre-Diabetes | 150 | 6.8% (oversampled) |
| Diabetes | 450 | 20.5% |

Key predictors: `HighBP`, `HighChol`, `BMI`, `Smoker`, `PhysActivity`, `GenHlth`, `Sex`, `Age`

---

## List of Analyses

### Analysis 1 — Multinomial Logistic Regression: Contraceptive Method Choice

- **Data preparation** — variable recoding, factor labelling, dichotomisation of education and standard of living (low = levels 1–2, high = levels 3–4), reference category assignment for `VGAM` (`no_use` as last level) and `nnet` (`no_use` as first level)
- **Exploratory data analysis** — descriptive statistics by contraceptive method (Table 2); group-level distributions for all predictors
- **Unadjusted models**
  - Model 1: wife's religion — RRR, 95% CI, log-odds (Table 3)
  - Model 2: wife's education (four-level) — dose-response pattern, RRR, 95% CI (Table 4)
- **Adjusted models**
  - Model 3 (full): all seven predictors with four-level education and SoL — coefficient pattern inspection
  - Model 4 (final): dichotomised education and SoL — parsimonious model for final inference
- **Model diagnostics** — multicollinearity via GVIF (`car::vif()` on proxy OLS model)
- **Interaction analysis** — a priori Religion × Education interaction; LRT comparison of main-effects vs interaction model; interaction RRRs and 95% CIs
- **Model fit** — global LRT vs null, McFadden R²
- **Model comparison** — AIC and BIC across null, full (4-level), and final (dichotomised) models
- **Model results** — adjusted RRRs, log-odds, and 95% CIs for both equations: long-term vs no-use and short-term vs no-use (Table 5); predictor-by-predictor interpretation
- **Prediction**
  - Predicted log-odds and probabilities from Model 1 (`predict.vgam`, `type = "link"` and `"response"`)
  - Manual derivation using the multinomial probability formula for non-Islamic and Islamic profiles, verified against `predict.vgam()`
  - Predicted probabilities from the final adjusted model for four covariate profiles varying education, religion, and standard of living
- **Cross-validation** — coefficient comparison between `VGAM::vglm` and `nnet::multinom`; p-values via z-test from `nnet`; 95% CIs from three-dimensional `confint` array
- **Discussion & limitations** — interpretation of education, parity, religion, age, SoL, and media effects; Religion × Education interaction conclusion; cross-sectional and generalisability caveats

### Analysis 2 — Ordinal Logistic Regression: Diabetes Severity

- **Data preparation** — variable recoding: binary factors, ordered outcome (`diabetes_cat`), collapsed age bands (13 groups → 6 clinical bands), GenHlth and Income factor levels; ordering verified: No Diabetes < Pre-Diabetes < Diabetes
- **Exploratory data analysis**
  - Outcome distribution — bar chart with counts and proportions
  - Descriptive statistics by diabetes status (Table 1) — means/SDs for continuous, n/% for categorical
- **Model building**
  - Univariable screening — separate proportional odds models for all 19 candidate predictors; Hosmer-Lemeshow p < 0.25 entry criterion; unadjusted ORs with 95% CIs (Table 2); screening decision with clinical relevance rationale
  - Full adjusted model — proportional odds model (`ordinal::clm`, logit link), 8 predictors: HighBP, HighChol, BMI, Smoker, PhysActivity, GenHlth, Sex, Age; threshold parameters (α₁, α₂) and regression coefficients (β) explained
- **Model diagnostics**
  - Multicollinearity: GVIF via proxy linear model (`car::vif()`); GVIF^(1/(2·Df)) interpreted against 2.0 threshold
  - BMI linearity: Pearson correlation between BMI quartile midpoints and empirical log-odds at both cumulative thresholds (≥ Pre-Diabetes, ≥ Diabetes); plot of log-odds vs quartile midpoint
- **Model results**
  - Adjusted ORs with 95% CIs and p-values (Table 3)
  - Threshold coefficients with ordering check (α₁ < α₂ confirmed) (Table 4)
- **Proportional odds assumption**
  - `nominal_test()` — tests whether each predictor's effect varies across thresholds; significant p < 0.05 = violation
  - `scale_test()` — tests whether latent variance differs across predictor levels
  - Rationale for not applying the Brant test: sparse Pre-Diabetes category (n = 150), convergence failure with multi-level predictors
- **Model fit**
  - Global LRT — full vs null model (χ², df, p-value)
  - Pseudo R² — McFadden and Nagelkerke
  - Variable contribution — drop-one LRT; top 3 contributors ranked by LRT statistic
- **Interaction analysis** — a priori HighBP × BMI interaction (shared insulin-resistance pathway); LRT comparison of main-effects vs interaction model; interaction OR with 95% CI; model selection decision
- **Model comparison** — AIC and BIC across Null, Reduced (−Smoker, −PhysActivity), and Full models (Table 5); LRT for Smoker + PhysActivity joint contribution
- **Interpretation of results**
  - Proportional odds interpretation framework — single OR constant across both cut-points explained
  - Summary table — adjusted ORs with direction and significance flag (Table 6)
  - Predictor-by-predictor interpretation — all 11 terms: HighBP, HighChol, BMI (5-unit compounding), Smoker, PhysActivity (largest modifiable effect), GenHlth dose-response gradient (Very Good → Good → Fair → Poor), Sex, Age (monotone gradient to 75+)
- **Prediction**
  - Classification accuracy — overall accuracy and per-class sensitivity (No Diabetes, Pre-Diabetes, Diabetes)
  - Confusion matrix (Table 7)
  - Fitted probabilities for four illustrative clinical profiles varying BMI, HighBP, Age, and GenHlth (Table 8)
  - Manual calculation — Hosmer et al. Eq. 8.25: cumulative probabilities P(Y ≤ j) via logistic formula, then differenced into category probabilities; verified against `predict()` to within rounding error
- **Discussion** — summary of findings; BMI compounding effect; physical activity as most actionable predictor; self-rated health dose-response; age gradient
- **Limitations** — cross-sectional design; self-reported diabetes status; oversampled Pre-Diabetes (ORs are sample-level only, population prevalence not derivable); proportional odds violations for flagged predictors; residual confounding

---

## Results

### Multinomial Logistic Regression

- Wife's **education** was the strongest predictor of long-term use (RRR = 4.69, high vs low)
- **Parity** drove contraceptive adoption for both methods equally (RRR ≈ 1.35–1.40 per child)
- **Religion** (Islam) reduced relative odds for long-term use more than short-term use
- **Media exposure** was a significant barrier specifically to short-term use

### Ordinal Logistic Regression

- **BMI**, **high blood pressure**, and **high cholesterol** were the strongest metabolic risk factors
- **Physical activity** was the largest modifiable protective factor
- A monotone **age gradient** and dose-response **self-rated health gradient** (Excellent → Poor) were confirmed
- Full results in `ordinal_logistic_regression_diabetes.html`

---

## Disclaimer

- This repository was developed as part of the **GDT600 Advanced Categorical Data Analysis** course, DrPH Programme, Universiti Sains Malaysia.
- All datasets are real public-use datasets used strictly for educational purposes. ORs and RRRs reflect sample-level associations and should not be used for clinical or policy decisions.
- Code and documentation were developed with assistance from **Claude (Anthropic)**, an AI assistant. All analytical choices, interpretations, and conclusions remain the sole responsibility of the author.
- Materials are intended exclusively for **educational and demonstration purposes**.

---

## License

This project is licensed under the **MIT License**.

---

## Version History

- v1.0.0 (2026-06-02): Initial release — ordinal logistic regression (diabetes, CDC BRFSS 2015).
- v2.0.0 (2026-06-02): Added multinomial logistic regression (contraceptive choice, Indonesia 1987). Repo restructured into two analysis subfolders.
- v2.1.0 (2026-06-02): Updated multinomial analysis — added model diagnostics (GVIF), interaction analysis (Religion × Education), model comparison (AIC/BIC), and final-model predicted probability profiles.
- v2.2.0 (2026-06-02): Fixed README Analysis 2 statistical flow to match ordinal QMD exactly — Model Comparison and Interpretation of Results restored as separate sections; section order corrected.
