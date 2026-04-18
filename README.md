# Credit Default Prediction Model – Portfolio Risk Analysis

## Project Background

This project analyzes a fictional neobank lending portfolio, the business operates in consumer unsecured lending, with products such as short-term personal loans and installment loans originated through a digital application journey.

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

[Visualization, including a graph of overall trends or snapshot of a dashboard]

## Insights Deep Dive

### 1) Borrower Profile Risk

* Borrowers in the lowest credit-score quintile had an observed default rate of **65.3%**, compared with **14.2%** in the highest quintile.
* Default rates also declined steadily with income: the lowest income quintile showed a **43.7%** default rate versus **28.2%** in the highest income quintile.
* This confirms a strong risk gradient across core underwriting variables, especially credit score.
* Loan size also contributes to risk, particularly when paired with lower income and weaker credit profiles.

[Visualization specific to category 1]

### 2) Loan Performance and Default Timing

* The repayment table shows **118,277 Current** statuses and **25,723 Default** statuses, indicating that delinquency is meaningful but not overwhelming in the portfolio.
* Defaults do not appear evenly across the loan lifecycle; they tend to cluster after a few months on books, which is consistent with real-world credit behavior.
* This suggests that early performance monitoring is important, but the company should also pay close attention to medium-term seasoning risk.
* Vintage tracking helps identify cohorts that deteriorate faster than the portfolio average.

[Visualization specific to category 2]

### 3) Model Performance and Risk Ranking

* The Random Forest model achieved an **AUC of 0.7108**, which indicates moderate discriminative power.
* On the test set, the model produced **67% accuracy**, with default-class recall of **0.59** and precision of **0.54**.
* Decile analysis showed strong rank ordering: predicted PD increased steadily from the lowest to highest risk deciles, and actual default rates rose in the same direction.
* However, predicted probabilities were initially too high relative to observed rates, so calibration was used to improve interpretability.

[Visualization specific to category 3]

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
