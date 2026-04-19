# Credit Default Prediction Model – Portfolio Risk Analysis

## Project Background

This project analyzes a neobank lending portfolio, the business operates in consumer unsecured lending, with products such as short-term personal loans and installment loans originated through a digital application journey.

The company has been active throughout 2024 in this simulated dataset and uses a data-driven underwriting model to balance growth, credit quality, and portfolio profitability. The lending business model is straightforward: acquire borrowers digitally, approve loans based on borrower and loan characteristics, collect repayments over the life of the loan, and manage losses through risk-based decisioning.

From a portfolio analytics point of view, the key business metrics are:

* **Probability of Default (PD)** to estimate borrower risk at origination
* **Expected Loss (EL)** to quantify credit exposure
* **Approval / rejection / manual review rates** to support underwriting strategy
* **Vintage and cohort performance** to monitor how loan books perform over time

This analysis uses a synthetic loan dataset designed to mimic realistic credit-risk patterns. It is structured to help stakeholders answer questions about borrower quality, risk segmentation, model performance, and portfolio loss.

Insights and recommendations are provided across the following areas:

* **Borrower Profile Risk**
* **Loan Performance and Default Timing**
* **Model Performance and Risk Ranking**
* **Portfolio Risk and Decision Strategy**


## Data Structure & Initial Checks

The company’s main analytical database consists of **three tables** with a total of **25,722 records** across the repayment history and borrower/loan master data.

The tables are structured as follows:

* **Table 1: `dim_users`**
  Borrower-level profile data including age, annual income, credit score, and employment status.

* **Table 2: `fact_loans`**
  Loan origination data including loan ID, borrower ID, loan amount, origination date, and term.

* **Table 3: `fact_repayments`**
  Monthly repayment status for each loan, used to identify whether a loan ever entered default.

These tables were combined into a master analytical file to create borrower-level features for modeling and reporting.

<img width="1195" height="493" alt="image" src="https://github.com/user-attachments/assets/9692be4b-4c80-46ed-b95c-ed06456574de" />

## Executive Summary

### Overview of Findings

Three themes stood out in the analysis: borrower risk falls sharply as credit score and income rise, defaults tend to emerge later in the loan life cycle rather than immediately at origination, and the model is able to rank risk reasonably well even though its probability estimates required calibration. The main takeaway is that underwriting can be improved by tightening rules for low-score borrowers, using a review band for borderline applications, and monitoring bad cohorts more closely after origination.


## Insights Deep Dive

### 1) Borrower Profile Risk

* Borrower risk is strongly differentiated when segmented by predicted probability of default (PD) deciles. Loans in the highest-risk decile show materially higher observed default rates compared to the lowest-risk decile, confirming that the model provides effective rank ordering of risk.
* The relationship between credit score and default risk is clearly monotonic: lower credit score segments consistently exhibit higher default rates. This reinforces credit score as one of the most important drivers of borrower risk in the portfolio.
* Income also shows a negative relationship with default, though less pronounced than credit score. Lower-income borrowers tend to default at higher rates, particularly when combined with weaker credit profiles.
* When combining features, risk compounds: borrowers with low credit scores and lower income levels represent the highest-risk segment of the portfolio.

<img width="846" height="470" alt="decile_analysis" src="https://github.com/user-attachments/assets/75bf29d9-2617-47c6-af42-efc62308fda7" />
<img width="460" height="680" alt="SHAP model explainability" src="https://github.com/user-attachments/assets/96984752-8ce9-435e-894e-75c6a2be1b07" />


### 2) Loan Performance and Default Timing

* Defaults do not appear evenly across the loan lifecycle; they tend to cluster after a few months on books, which is consistent with real-world credit behavior.
* This suggests that early performance monitoring is important, but the company should also pay close attention to medium-term seasoning risk.
* Vintage tracking helps identify cohorts that deteriorate faster than the portfolio average.

<img width="917" height="547" alt="vintage-analysis" src="https://github.com/user-attachments/assets/826fe86b-2416-4533-89cd-2528cb3b1d18" />


### 3) Model Performance and Risk Ranking

* The Random Forest model achieved an **AUC of 0.71**, which indicates moderate discriminative power.
* Decile analysis showed strong rank ordering: predicted PD increased steadily from the lowest to highest risk deciles, and actual default rates rose in the same direction.
* However, predicted probabilities were initially too high relative to observed rates, so calibration was used to improve interpretability.

<img width="536" height="470" alt="AUC diagram" src="https://github.com/user-attachments/assets/483fae89-cfd5-4661-888d-3b9f16468f63" />
<img width="536" height="547" alt="DR to PD diagram" src="https://github.com/user-attachments/assets/262c7721-9a12-4d79-9b87-dba08a089f92" />


### 4) Portfolio Risk and Decision Strategy

* Expected Loss on the test set averaged about **1,978** per loan, with a median of **1,429** and a maximum of **9,221**.
* The approval strategy separated accounts into three groups: **Approve**, **Review**, and **Reject**.
* Approved loans had an average PD of only **13.7%** and an observed default rate of **14.7%**, while rejected loans averaged **57.2%** PD.
* This shows that the decision framework meaningfully filters out high-risk applications, but the review band remains important for borderline borrowers.

[Visualization specific to category 4]

## Recommendations

Based on the insights and findings above, we would recommend the **credit risk / underwriting team** to consider the following:

* Borrowers with low credit scores and weak affordability should be routed to tighter rules or manual review. Reduce exposure to the lowest-risk-quality segment and reserve approvals for stronger profiles.
* Increase monitoring in the first several months after origination, since defaults appear to emerge after seasoning rather than immediately. Early-warning triggers should be added for emerging delinquency.
* Use the review band more actively for borderline applications. This helps preserve growth while preventing avoidable losses from accounts that sit near the approval threshold.
* Incorporate cohort and vintage performance into monthly portfolio governance. Bad cohorts should trigger investigation into underwriting drift, channel quality, or macro-like effects in origination timing.
* Align loan pricing and limits more closely with PD and EL. Higher-risk segments should either be priced for risk, capped more conservatively, or declined entirely.

## Assumptions and Caveats

Throughout the analysis, multiple assumptions were made to manage the synthetic dataset and the modeling workflow. These assumptions and caveats are noted below:

* The dataset is **synthetic** and was generated to resemble realistic loan performance patterns, so the results should be interpreted as a portfolio analytics exercise rather than a production scorecard.
* A loan was treated as **defaulted if it entered `Default` status at any point** in its repayment history.
* Monthly repayment status was used as the performance source of truth, while borrower attributes were treated as static at origination.
* The calibration step was necessary because initial model probabilities were systematically higher than observed default rates.
* Cohort labeling was based on cumulative performance, so “bad” and “good” cohorts reflect relative portfolio behavior rather than permanent borrower labels.
* Any missing or unusual records in the original synthetic construction were handled conservatively to preserve analytical consistency.

## Repository Contents

* `master_analytical_file.csv` – final modeling dataset
* Python notebook – data generation, cleaning, EDA, feature engineering, modeling, and evaluation
* SQL scripts – targeted business queries for portfolio and underwriting analysis

## Final Note

This project demonstrates an end-to-end credit risk workflow: raw borrower and repayment data are transformed into a master analytical file, modelled for default prediction, and translated into business decisions that can support lending strategy and portfolio governance.
