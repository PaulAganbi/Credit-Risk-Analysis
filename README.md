# Credit Risk Modeling & Decision Strategy
## Overview
This project simulates a real-world credit risk workflow, from data generation to lending decisions.

The objective is to predict borrower default risk (PD) and translate model outputs into actionable credit strategy and portfolio risk insights.
## Business Problem
Financial institutions must balance loan growth with risk control.

This project addresses:
- How to predict default risk
- How to segment borrowers by risk
- How to translate predictions into lending decisions
- How to quantify financial risk using Expected Loss
## Dataset
Synthetic dataset generated to mimic a lending environment:

- 10,000 borrowers
- 12,000 loans
- Monthly repayment behavior over 12 months

Tables:
- dim_users: borrower attributes (income, credit score, employment)
- fact_loans: loan origination details
- fact_repayments: loan performance over time
## Methodology
### 1. Data Preparation
- Merged borrower and loan data into a Master Analytical File (MAF)
- Derived default labels from repayment behavior
### 2. Feature Engineering
- Credit score bands
- Income-to-loan ratio (affordability metric)
- Aggregated behavioral indicators
### 3. Modeling
Models used:
- Logistic Regression
- Random Forest (primary model)

Performance:
- AUC ≈ 0.71
### 4. Model Evaluation
- ROC-AUC for discrimination
- Decile analysis for risk ranking
- Calibration curve for probability accuracy
### 5. - Explainability
SHAP (SHapley Additive Explanations) used to:

- Identify key drivers of default risk
- Analyze feature interactions
- Explain individual predictions

Key insight:
- Credit score is the dominant risk driver
- Income plays a secondary role
### 6. Credit Decision Strategy
Borrowers segmented into:
- Approve (low PD)
- Review (medium PD)
- Reject (high PD)

This translates model outputs into actionable credit policy.
### 7. Portfolio Risk Analysis
Expected Loss (EL) calculated as:

EL = PD × LGD × EAD

Assumptions:
- LGD = 40%
- EAD = Loan Amount

This provides a monetary view of risk across the portfolio.
## Key Insights
- Model successfully ranks borrowers by risk (AUC ≈ 0.71)
- Default rates increase consistently across deciles
- Credit score is the primary driver of default risk
- Expected Loss highlights concentration of financial risk in high-value loans
## Tools & Technologies
- Python (pandas, numpy, sklearn)
- SHAP (model explainability)
- Matplotlib (visualization)
## Conclusion
This project demonstrates a full credit risk modeling pipeline, from data generation to business decision-making.

It highlights how predictive models can be integrated into lending strategy and risk management frameworks.
