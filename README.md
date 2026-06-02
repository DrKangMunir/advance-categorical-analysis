# Advance Categorical Data Analysis – GDT600

![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)
![R ≥ 4.0](https://img.shields.io/badge/R-≥%204.0-blue.svg)
![Quarto ≥ 1.4](https://img.shields.io/badge/Quarto-≥%201.4-orange.svg)
![GitHub stars](https://img.shields.io/github/stars/DrKangMunir/advance-categorical-analysis?style=social)
![GitHub forks](https://img.shields.io/github/forks/DrKangMunir/advance-categorical-analysis?style=social)

Reproducible **ordinal logistic regression** analysis using CDC BRFSS 2015 survey data for the Advanced Categorical Data Analysis course (DrPH, Universiti Sains Malaysia).

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

- Examines sociodemographic, behavioural, and clinical factors associated with **diabetes severity** (No Diabetes → Pre-Diabetes → Diabetes) using the **proportional odds model** (ordinal logistic regression) on a stratified sample from the CDC BRFSS 2015 dataset (n = 2,200).
- Uses **R**, **Quarto**, and **tidyverse-based workflows** to demonstrate data preparation, exploratory data analysis, model building, assumption testing, model fit evaluation, interaction analysis, and prediction.
- The proportional odds model (`ordinal::clm`) is applied following the framework of Hosmer, Lemeshow & Sturdivant (*Applied Logistic Regression*, 3rd ed., Chapter 8).
- Status: Individual assignment submission for GDT600 Advanced Categorical Data Analysis.

---

## Contributor

| Name                             | Student ID | Role                     |
|----------------------------------|------------|--------------------------|
| Dr. Nazirul Munir Bin Abu Hassan | 23202537   | Analysis & Documentation |

---

## Folder Structure

Layout within this repository:

```
├── ordinal_logistic_regression_diabetes.qmd
├── ordinal_logistic_regression_diabetes.html
├── diabetes_sample_2200.csv
└── README.md
```

- `ordinal_logistic_regression_diabetes.qmd` – fully reproducible Quarto source document
- `ordinal_logistic_regression_diabetes.html` – rendered HTML report (self-contained, no internet required)
- `diabetes_sample_2200.csv` – stratified random sample dataset (n = 2,200)
- `README.md` – this overview file

---

## Installation

### Prerequisites

- R (≥ 4.0.0) and RStudio (recommended)
- Quarto (≥ 1.4)
- Required R packages:

  `tidyverse`, `ordinal`, `gtsummary`, `broom`,
  `knitr`, `kableExtra`, `car`

### Setup

```bash
git clone https://github.com/DrKangMunir/advance-categorical-analysis.git
cd advance-categorical-analysis
```

Then in R:

```r
install.packages(c(
  "tidyverse", "ordinal", "gtsummary", "broom",
  "knitr", "kableExtra", "car"
))
```

---

## Usage

From the repository root:

```bash
# Quarto
quarto render ordinal_logistic_regression_diabetes.qmd
```

This generates `ordinal_logistic_regression_diabetes.html` with the full analysis — data preparation, EDA, model building, diagnostics, assumption testing, model fit, interaction analysis, prediction, and discussion.

Alternatively, open the `.qmd` file in RStudio and click **Render**.

---

## Data

### CDC BRFSS 2015 — Stratified Sample

- **Source:** `diabetes_sample_2200.csv` (stratified random sample from the full CDC BRFSS 2015 dataset, n = 253,680)
- **Sample size:** n = 2,200
- **Stratification on `Diabetes_012`:**

| Category     | n     | % in sample | Population prevalence |
|--------------|-------|-------------|-----------------------|
| No Diabetes  | 1,600 | 72.7%       | ~86%                  |
| Pre-Diabetes | 150   | 6.8%        | ~1.8% (oversampled)   |
| Diabetes     | 450   | 20.5%       | ~12%                  |

Pre-Diabetes was deliberately oversampled to ensure stable coefficient estimation (proportional sampling would yield fewer than 40 cases). ORs reflect sample-level associations and cannot be used to derive population prevalence.

### Key Variables

| Variable              | Type        | Description                                         |
|-----------------------|-------------|-----------------------------------------------------|
| `Diabetes_012`        | Ordered (0/1/2) | Outcome: No Diabetes / Pre-Diabetes / Diabetes  |
| `HighBP`              | Binary      | High blood pressure diagnosis                       |
| `HighChol`            | Binary      | High cholesterol diagnosis                          |
| `BMI`                 | Continuous  | Body mass index                                     |
| `Smoker`              | Binary      | Ever smoked ≥ 100 cigarettes lifetime               |
| `PhysActivity`        | Binary      | Physical activity in the past 30 days               |
| `GenHlth`             | Ordered (1–5) | Self-rated general health (Excellent → Poor)      |
| `Sex`                 | Binary      | Biological sex (Female / Male)                      |
| `Age`                 | Categorical | Age group (collapsed into 6 bands: 18–34 to 75+)   |

Full data documentation is available in the original CDC BRFSS 2015 codebook and the UCI Machine Learning Repository (DOI: [10.24432/C53919](https://doi.org/10.24432/C53919)).

---

## List of Analyses

### Ordinal Logistic Regression — Diabetes Health Status

- **Data Preparation**
  - Stratified sampling rationale and sample description
  - Variable recoding: binary factors, ordered outcome (`diabetes_cat`), collapsed age bands, GenHlth and Income factor levels
  - Ordering verification: No Diabetes < Pre-Diabetes < Diabetes

- **Exploratory Data Analysis**
  - Outcome distribution (bar chart with counts and proportions)
  - Descriptive statistics by diabetes status (Table 1) — means/SDs for continuous, n/% for categorical

- **Univariable Screening**
  - Separate proportional odds models for all 19 candidate predictors
  - Hosmer-Lemeshow criterion (p < 0.25) for multivariable entry
  - Unadjusted ORs with 95% CIs (Table 2)

- **Full Adjusted Model**
  - Proportional odds model (`ordinal::clm`, logit link) with 8 predictors: HighBP, HighChol, BMI, Smoker, PhysActivity, GenHlth, Sex, Age
  - Threshold parameters (α₁, α₂) and regression coefficients (β)

- **Model Diagnostics**
  - Multicollinearity: GVIF via proxy linear model (`car::vif`)
  - BMI linearity: Pearson correlation between BMI quartile midpoints and empirical log-odds at both thresholds

- **Model Results**
  - Adjusted ORs with 95% CIs and p-values (Table 3)
  - Threshold coefficients with ordering check (Table 4)
  - Predictor-by-predictor clinical interpretation

- **Proportional Odds Assumption**
  - `nominal_test()`: tests whether predictor effects vary across thresholds
  - `scale_test()`: tests whether latent variance differs across predictor levels
  - Rationale for not applying the Brant test (sparse middle category, convergence failure)

- **Model Fit**
  - Global likelihood ratio test (full vs. null)
  - McFadden and Nagelkerke pseudo R²
  - Drop-one LRT for variable contribution (Table 5)
  - AIC/BIC model comparison: Null vs. Reduced vs. Full (Table 6)

- **Interaction Analysis**
  - A priori HighBP × BMI interaction (shared insulin-resistance pathway)
  - LRT comparison of main-effects vs. interaction model
  - OR for interaction term with 95% CI

- **Prediction**
  - Classification accuracy and confusion matrix (Table 7)
  - Per-class sensitivity (No Diabetes, Pre-Diabetes, Diabetes)
  - Fitted probabilities for four illustrative clinical profiles (Table 8)
  - Manual probability calculation following Hosmer et al. Eq. 8.25, verified against `predict()`

- **Discussion**
  - Summary of findings, key ORs, dose-response patterns
  - Limitations: cross-sectional design, self-reported diabetes, oversampled Pre-Diabetes, proportional odds violations, residual confounding
  - Public health conclusion

---

## Results

- **Ordinal logistic regression:** Adjusted ORs, proportional odds assumption tests, pseudo R², model comparison, and prediction in `ordinal_logistic_regression_diabetes.html`.

Key findings:

- **BMI** (OR per unit), **high blood pressure**, and **high cholesterol** were the strongest metabolic risk factors for higher diabetes severity.
- **Physical activity** was the largest modifiable protective factor.
- A monotone **age gradient** and a dose-response **self-rated health gradient** (Excellent → Poor) were confirmed.
- The HighBP × BMI interaction was tested; the main-effects or interaction model was selected based on LRT results (see report).

---

## Disclaimer

- This repository was developed as part of the **GDT600 Advanced Categorical Data Analysis** course, DrPH Programme, Universiti Sains Malaysia.
- The dataset is a **real public-use dataset** (CDC BRFSS 2015) but represents a stratified sample for educational purposes. ORs are sample-level associations and should not be used to derive population prevalence estimates.
- Development of code and documentation was supported by **Claude (Anthropic)**, an AI assistant. Claude assisted with structuring code, automating inline interpretations, and refining documentation; however, all analytical choices, interpretations, and conclusions remain the sole responsibility of the author.
- The materials provided here are intended exclusively for **educational and demonstration purposes**. They should **not** be applied to clinical practice, policy decisions, or real-world patient data analysis without appropriate validation.

---

## License

This project is licensed under the **MIT License**.  
See the `LICENSE` file for full details.

---

## Version History

- v1.0.0 (2026-06-02): Initial release — ordinal logistic regression analysis of CDC BRFSS 2015 diabetes health indicators.
