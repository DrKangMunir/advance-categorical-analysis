# Advance Categorical Data Analysis

**DrPH Programme — Semester 2**  
**Course: Advanced Categorical Data Analysis**

---

## Assignment: Ordinal Logistic Regression — Diabetes Health Status

**Dataset:** CDC BRFSS 2015 Health Indicators (stratified sample, n = 2,200)  
**Method:** Proportional Odds Model (`ordinal::clm`)  
**Software:** R / Quarto

### Files

| File | Description |
|------|-------------|
| `ordinal_logistic_regression_diabetes.qmd` | Quarto source — fully reproducible analysis |
| `ordinal_logistic_regression_diabetes.html` | Rendered HTML report |
| `diabetes_sample_2200.csv` | Stratified sample dataset (n = 2,200) |

### Analysis Overview

| Section | Content |
|---------|---------|
| Data Preparation | Variable recoding, factor ordering |
| EDA | Outcome distribution, descriptive table by group |
| Model Building | Univariable screening (p < 0.25), full adjusted model |
| Diagnostics | Multicollinearity (GVIF), BMI linearity check |
| Model Results | Adjusted ORs, threshold coefficients |
| Proportional Odds | Nominal test, scale test |
| Model Fit | Global LRT, pseudo R², drop-one LRT, AIC/BIC comparison |
| Interaction | HighBP × BMI |
| Prediction | Classification accuracy, fitted probabilities, manual calculation |
| Discussion | Summary, limitations, conclusion |

### Research Question

> What sociodemographic, behavioural, and clinical factors are associated with severity of diabetes status (No Diabetes → Pre-diabetes → Diabetes)?

### Key Findings

- **BMI**, **high blood pressure**, and **high cholesterol** were the strongest metabolic risk factors
- **Physical activity** was the largest modifiable protective factor
- A monotone **age gradient** and a dose-response **self-rated health gradient** were confirmed
- Proportional odds assumption was assessed via nominal and scale tests

---

*Reference: Hosmer, Lemeshow & Sturdivant (2013). Applied Logistic Regression, 3rd ed., Chapter 8.*
