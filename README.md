# Advance Categorical Data Analysis

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

This repository contains two individual assignment submissions for Advanced Categorical Data Analysis:

| # | Analysis | Dataset | Method |
|---|----------|---------|--------|
| 1 | Contraceptive method choice | 1987 National Indonesia Contraceptive Prevalence Survey (n = 1,473) | Multinomial logistic regression (`VGAM::vglm`) |
| 2 | Diabetes severity | CDC BRFSS 2015 stratified sample (n = 2,200) | Ordinal logistic regression / proportional odds model (`ordinal::clm`) |

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

- **`Multinomial-Logistic-Regression/`** contains the multinomial logistic regression analysis on contraceptive method choice
  - `cmc_multinomial_analysis.qmd` is the Quarto source file (fully reproducible)
  - `cmc_multinomial_analysis.html` is the rendered HTML report
  - `cmc.csv` is the 1987 Indonesia Contraceptive Prevalence Survey data (n = 1,473)

- **`Ordinal-Logistic-Regression/`** contains the proportional odds model on diabetes severity
  - `ordinal_logistic_regression_diabetes.qmd` is the Quarto source file (fully reproducible)
  - `ordinal_logistic_regression_diabetes.html` is the rendered HTML report
  - `diabetes_sample_2200.csv` is the stratified sample from CDC BRFSS 2015 (n = 2,200)

- `README.md` is this overview file

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
| `wife_edu` | Ordinal (1-4) | Wife's education level |
| `husb_edu` | Ordinal (1-4) | Husband's education level |
| `num_children` | Continuous | Number of children ever born |
| `wife_religion` | Binary | Religion: 0 = Non-Islam, 1 = Islam |
| `wife_working` | Binary | Working status: 0 = Yes, 1 = No |
| `husb_occupation` | Nominal (1-4) | Husband's occupation |
| `sol_index` | Ordinal (1-4) | Standard of living index |
| `media_exposure` | Binary | Media exposure: 0 = Good, 1 = Not good |

### 2. CDC BRFSS 2015 Stratified Sample

- **File:** `Ordinal-Logistic-Regression/diabetes_sample_2200.csv`
- **Source:** CDC BRFSS 2015 / UCI ML Repository (DOI: [10.24432/C53919](https://doi.org/10.24432/C53919))
- **Sample size:** n = 2,200 (stratified with Pre-Diabetes oversampled)

| Category | n | % in sample |
|----------|---|-------------|
| No Diabetes | 1,600 | 72.7% |
| Pre-Diabetes | 150 | 6.8% (oversampled) |
| Diabetes | 450 | 20.5% |

Key predictors: `HighBP`, `HighChol`, `BMI`, `Smoker`, `PhysActivity`, `GenHlth`, `Sex`, `Age`

---

## List of Analyses

### Analysis 1: Multinomial Logistic Regression on Contraceptive Method Choice

- **Data preparation** covers variable recoding, factor labelling, dichotomisation of education and standard of living (low = levels 1-2, high = levels 3-4), and reference category assignment for `VGAM` (no_use as last level) and `nnet` (no_use as first level)
- **Exploratory data analysis** presents descriptive statistics by contraceptive method (Table 2) and group-level distributions for all predictors
- **Unadjusted models**
  - Model 1 uses wife's religion as the sole predictor and reports RRR, 95% CI, and log-odds (Table 3)
  - Model 2 uses wife's education as a four-level predictor to examine the dose-response pattern with RRR and 95% CI (Table 4)
- **Adjusted models**
  - Model 3 (full model) enters all seven predictors with four-level education and SoL to inspect the coefficient pattern
  - Model 4 (final model) uses dichotomised education and SoL for a more parsimonious fit and is the model carried forward for inference
- **Model diagnostics** assess multicollinearity using GVIF via `car::vif()` on a proxy OLS model
- **Interaction analysis** tests an a priori Religion x Education interaction using LRT comparison of the main-effects model vs the interaction model and reports interaction RRRs and 95% CIs
- **Model fit** reports the global LRT against the null model and McFadden R²
- **Model comparison** presents AIC and BIC across the null, full (4-level), and final (dichotomised) models
- **Model results** reports adjusted RRRs, log-odds, and 95% CIs for both equations (long-term vs no-use and short-term vs no-use) in Table 5 with predictor-by-predictor interpretation
- **Prediction**
  - Predicted log-odds and probabilities from Model 1 using `predict.vgam` with `type = "link"` and `type = "response"`
  - Manual derivation of predicted probabilities using the multinomial probability formula for a non-Islamic and an Islamic profile, verified against `predict.vgam()`
  - Predicted probabilities from the final adjusted model for four covariate profiles varying education, religion, and standard of living
- **Cross-validation** compares coefficients between `VGAM::vglm` and `nnet::multinom`, derives p-values via z-test from `nnet`, and extracts 95% CIs from the three-dimensional `confint` array
- **Discussion and limitations** cover interpretation of education, parity, religion, age, standard of living, and media effects alongside the Religion x Education interaction conclusion and cross-sectional and generalisability caveats

---

### Analysis 2: Ordinal Logistic Regression on Diabetes Severity

- **Data preparation** covers variable recoding for binary factors, the ordered outcome (`diabetes_cat`), collapsed age bands (13 groups into 6 clinical bands), and GenHlth and Income factor levels. Ordering is verified as No Diabetes < Pre-Diabetes < Diabetes
- **Exploratory data analysis**
  - Outcome distribution is shown as a bar chart with counts and proportions
  - Descriptive statistics by diabetes status are presented in Table 1 with means and SDs for continuous variables and n and % for categorical variables
- **Model building**
  - Univariable screening fits separate proportional odds models for all 19 candidate predictors using the Hosmer-Lemeshow p < 0.25 entry criterion and presents unadjusted ORs with 95% CIs in Table 2 with a screening decision based on clinical relevance
  - Full adjusted model fits a proportional odds model using `ordinal::clm` with the logit link and 8 predictors (HighBP, HighChol, BMI, Smoker, PhysActivity, GenHlth, Sex, Age) and explains the role of threshold parameters (α₁, α₂) and regression coefficients (β)
- **Model diagnostics**
  - Multicollinearity is assessed using GVIF via `car::vif()` on a proxy linear model with GVIF^(1/(2·Df)) interpreted against the 2.0 threshold
  - BMI linearity is checked using Pearson correlation between BMI quartile midpoints and empirical log-odds at both cumulative thresholds (≥ Pre-Diabetes and ≥ Diabetes) with a supporting plot
- **Model results**
  - Table 3 presents adjusted ORs with 95% CIs and p-values
  - Table 4 presents threshold coefficients with confirmation that α₁ < α₂ (no reversal)
- **Proportional odds assumption**
  - `nominal_test()` checks whether each predictor's effect varies across thresholds, where a significant p < 0.05 indicates a violation
  - `scale_test()` checks whether latent variance differs across predictor levels
  - Rationale is provided for not applying the Brant test due to the sparse Pre-Diabetes category (n = 150) and convergence failure with multi-level predictors
- **Model fit**
  - Global LRT compares the full model against the null model reporting χ², df, and p-value
  - Pseudo R² reports both McFadden and Nagelkerke values
  - Variable contribution ranks each predictor by drop-one LRT statistic and identifies the top three contributors
- **Interaction analysis** tests an a priori HighBP x BMI interaction (shared insulin-resistance pathway) using LRT comparison of the main-effects and interaction models and reports the interaction OR with 95% CI and the model selection decision
- **Model comparison** presents AIC and BIC across Null, Reduced (-Smoker, -PhysActivity), and Full models in Table 5 along with the LRT for the joint contribution of Smoker and PhysActivity
- **Interpretation of results**
  - The proportional odds interpretation framework explains that a single OR applies constantly across both cut-points
  - Table 6 presents adjusted ORs with direction and significance flags
  - Predictor-by-predictor interpretation covers all 11 terms including HighBP, HighChol, BMI (with the 5-unit compounding effect), Smoker, PhysActivity (the largest modifiable effect), the GenHlth dose-response gradient from Very Good to Poor, Sex, and the monotone Age gradient to 75+
- **Prediction**
  - Classification accuracy reports overall accuracy and per-class sensitivity for No Diabetes, Pre-Diabetes, and Diabetes
  - Confusion matrix is presented in Table 7
  - Table 8 presents fitted probabilities for four illustrative clinical profiles varying BMI, HighBP, Age, and GenHlth
  - Manual calculation follows Hosmer et al. Eq. 8.25 by computing cumulative probabilities P(Y ≤ j) via the logistic formula and differencing them into category probabilities, verified against `predict()` to within rounding error
- **Discussion** summarises findings including the BMI compounding effect, physical activity as the most actionable predictor, the self-rated health dose-response, and the age gradient
- **Limitations** address the cross-sectional design, self-reported diabetes status, oversampled Pre-Diabetes (ORs are sample-level only and population prevalence cannot be derived), proportional odds violations for flagged predictors, and residual confounding

---

## Results

### Multinomial Logistic Regression

- Wife's **education** was the strongest predictor of long-term use (RRR = 4.69, high vs low)
- **Parity** drove contraceptive adoption for both methods equally (RRR approximately 1.35 to 1.40 per child)
- **Religion** (Islam) reduced relative odds for long-term use more than short-term use
- **Media exposure** was a significant barrier specifically to short-term use

### Ordinal Logistic Regression

- **BMI**, **high blood pressure**, and **high cholesterol** were the strongest metabolic risk factors
- **Physical activity** was the largest modifiable protective factor
- A monotone **age gradient** and a dose-response **self-rated health gradient** (Excellent to Poor) were confirmed
- Full results are available in `ordinal_logistic_regression_diabetes.html`

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

- v1.0.0 (2026-06-02): Initial release with ordinal logistic regression analysis on diabetes (CDC BRFSS 2015).
- v2.0.0 (2026-06-02): Added multinomial logistic regression on contraceptive choice (Indonesia 1987). Repo restructured into two analysis subfolders.
- v2.1.0 (2026-06-02): Updated multinomial analysis to include model diagnostics (GVIF), interaction analysis (Religion x Education), model comparison (AIC/BIC), and final-model predicted probability profiles.
- v2.2.0 (2026-06-02): Fixed Analysis 2 statistical flow to match the ordinal QMD section order exactly.
- v2.3.0 (2026-06-02): Rewrote README to remove em dashes and semicolons throughout.
