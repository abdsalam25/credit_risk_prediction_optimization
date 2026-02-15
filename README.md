# Credit Risk Analysis & Rating System

A two-part project exploring credit risk modeling and optimal rating bucket creation using real loan data.

## Overview

This repository contains two related projects focused on credit risk assessment:

**Part 1: Default Prediction Models**  
Testing different machine learning approaches to predict whether borrowers will default on loans

**Part 2: Rating Bucket Optimizer**  
Finding the best way to group borrowers into risk categories using dynamic programming

---

## Part 1: Predicting Loan Defaults

### Objective
Build models that can estimate default probability for any borrower based on their financial profile.

### Models Implemented
- **Logistic Regression** - classic approach, works well as baseline
- **Random Forest** - ensemble method, captures non-linear patterns  
- **Gradient Boosting** - usually performs best for tabular data

### Features Used
- FICO score
- Income and employment history
- Outstanding debts and credit lines
- Loan amount

### Performance
All three models achieved solid results:
- Logistic Regression: ~0.75 AUC
- Random Forest: ~0.78 AUC  
- Gradient Boosting: ~0.80 AUC

### Expected Loss Calculation
Once you have default probability, you can calculate expected loss:
```
Expected Loss = PD × LGD × EAD
```

Where:
- **PD** = Probability of Default (from our model)
- **LGD** = Loss Given Default (assume 90% since recovery rate is approximately 10%)
- **EAD** = Exposure at Default (the loan amount)

Example: A $10,000 loan with 15% default probability results in an expected loss of $1,350

---

## Part 2: Optimal Rating Buckets

### Problem Statement
How do you divide thousands of borrowers into meaningful risk categories? Each bucket should be internally consistent with similar default rates while being clearly different from other buckets.

### Methodology
Used **dynamic programming** to find the optimal split points that maximize the log-likelihood across all buckets. The algorithm evaluates every possible way to divide the data and selects the configuration that makes the most statistical sense.

### Algorithm Steps
1. Sort all borrowers by FICO score
2. Use dynamic programming to find best split points and maximize separation between groups
3. Calculate default rate for each resulting bucket
4. Assign ratings from 0 (best) to 4 (highest risk)

### Sample Output
```
Rating    Score Range      Count     Defaults    Default Rate
------------------------------------------------------------------
0         740 - 850        2,847     142         0.0499
1         680 - 739        2,156     259         0.1201
2         640 - 679        1,923     346         0.1799
3         580 - 639        1,734     433         0.2497
4         300 - 579        1,340     487         0.3634
```

The algorithm determined that 5 buckets provide clear separation without overfitting.

---

## Running the Code

**For default prediction:**
```bash
python credit_risk_pred.py
```

**For rating optimization:**
```bash
python credit_optimization.py
```

### Expected Data Format
Both scripts expect CSV files with these columns:
- `customer_id`
- `fico_score`
- `income`
- `years_employed`
- `credit_lines_outstanding`
- `loan_amt_outstanding`
- `total_debt_outstanding`
- `default` (0 or 1)

---

## Key Findings

**Model Performance:** Gradient Boosting slightly outperformed other models, though all three were competitive  

**Feature Importance:** FICO scores are predictive but not the complete picture - debt ratios and income also play significant roles  

**Rating Buckets:** Five rating buckets provided good separation without being too granular  

**Business Application:** Expected loss framework helps translate statistical predictions into actionable business metrics  

---

## Future Enhancements

Some potential improvements to explore:
- ROC curves and precision-recall plots for better model evaluation
- Feature importance analysis across models
- Cost-sensitive learning to account for expensive false negatives
- Testing different numbers of buckets for rating optimization
- Cross-validation for the bucketing algorithm
- Interactive web interface for real-time predictions

---

## Repository Structure
```
credit-risk-analysis/
├── credit_risk_pred.py          # Default prediction models
├── credit_optimization.py             # Rating bucket optimizer
├── Task_3_and_4_Loan_Data.csv  # Dataset
└── README.md                   # Documentation
```

---

## Notes

This was a learning project focused on understanding credit risk fundamentals. The models are intentionally straightforward. In a production environment, you would need more sophisticated validation, regulatory compliance checks, and fairness testing.
