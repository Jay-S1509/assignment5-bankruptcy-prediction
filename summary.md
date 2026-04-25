# Assignment 5 Summary: Bankruptcy Prediction
**Student:** Jayanth  
**Date:** April 24, 2026

---

## Problem Statement
- Predict if a company will go bankrupt (binary classification)
- Target variable: Bankrupt?
- Positive class: 1 = Bankrupt, Negative class: 0 = Not Bankrupt
- Missing a bankrupt company (false negative) is worse than false alarm
- Business goal: Help risk team identify companies likely to go bankrupt

---

## Dataset Summary
- File: data.csv
- Rows: 6819, Columns: 96
- Target: Bankrupt?
- No missing values, no duplicates
- All features are numeric

---

## Class Imbalance
- 220 bankrupt (3.23%) vs 6599 not bankrupt (96.77%)
- Dataset is highly imbalanced
- Accuracy is misleading here
- A model predicting not bankrupt every time = 96.8% accuracy but catches ZERO bankruptcies

---

## Metric Explanation
- **PR-AUC:** Main metric, focuses on positive class, better than ROC-AUC for imbalanced data
- **Brier Score:** Measures probability quality, lower is better, 0 = perfect
- **Accuracy:** NOT used as main metric, completely misleading for imbalanced data
- **Precision/Recall/F1:** Secondary metrics, describe one operating point only
- Thresholded metrics do not describe overall model quality
- PR-AUC is preferred because only 3.23% of companies are bankrupt

---

## Train Validation Test Split
- Train: 70% = 4773 rows
- Validation: 15% = 1023 rows
- Test: 15% = 1023 rows
- Stratified sampling used to preserve class balance
- Test set was locked until final evaluation only

---

## Feature Selection
- Feature Set A: 95 features (all usable features)
- Feature Set B: Top 25 features from XGBoost importance
- Reduction of 70 features
- Feature Set B PR-AUC = 0.4452 vs Feature Set A PR-AUC = 0.4945
- Feature Set A performs better so winner uses Set A
- Feature Set B is easier to explain but slightly weaker

---

## Experiment Results Table

| exp_id | model | feature_set | train_pr_auc | val_pr_auc | overfit_gap | val_roc_auc | val_brier | threshold | val_precision | val_recall | val_f1 | selected |
|--------|-------|-------------|-------------|-----------|-------------|-------------|-----------|-----------|--------------|-----------|--------|----------|
| 1 | Logistic Regression | A | 0.4756 | 0.2732 | 0.2024 | 0.8853 | 0.0934 | 0.5 | 0.1769 | 0.7879 | 0.2889 | No |
| 2 | XGBoost Baseline | A | 1.0000 | 0.4889 | 0.5111 | 0.9527 | 0.0238 | 0.5 | 0.6250 | 0.3030 | 0.4082 | No |
| 3 | XGBoost Imbalance | A | 1.0000 | 0.4945 | 0.5055 | 0.9582 | 0.0238 | 0.3 | 0.4483 | 0.3939 | 0.4194 | **Yes** |
| 4 | XGBoost Tuned | A | 0.9986 | 0.4915 | 0.5072 | 0.9647 | 0.0289 | 0.3 | 0.3375 | 0.8182 | 0.4779 | No |
| 5 | XGBoost Selected | B | 0.9780 | 0.4452 | 0.5328 | 0.9502 | 0.0393 | 0.3 | 0.2747 | 0.7576 | 0.4032 | No |

---

## Winning Model
**Experiment 3: XGBoost Imbalance Handling**

Why it won:
- Highest Val PR-AUC (0.4945)
- Best Brier Score tied with Exp 2 (0.0238)
- Used scale_pos_weight to handle class imbalance
- Lower threshold (0.3) improves recall
- Catching bankrupt companies matters more in business context
- Simpler than Experiment 4 with similar performance

---

## Final Test Results
- Test PR-AUC: 0.5238
- Test ROC-AUC: 0.9400
- Test Brier: 0.0234
- Test Precision: 0.5172
- Test Recall: 0.4545
- Test F1: 0.4839
- Threshold: 0.3

---

## Confusion Matrix
- True Negatives: 976 (correctly identified safe companies)
- False Positives: 14 (safe companies wrongly flagged as risky)
- False Negatives: 18 (missed bankrupt companies)
- True Positives: 15 (correctly caught bankrupt companies)
- Out of 33 total bankrupt companies, we caught 15

---

## Calibration Result
- Test Brier Score: 0.0234 (very good, close to 0)
- Calibration curve shows reasonable probability calibration
- Higher predicted probabilities correspond to higher bankruptcy rates
- Model is not overconfident
- Probabilities are trustworthy enough for risk decisions

---

## Feature Importance
- Top feature: Continuous interest rate (after tax) - importance 0.3808
- Second: Total debt/Total net worth - importance 0.0568
- Third: Retained Earnings to Total Assets - importance 0.0389

Why this makes sense:
- High interest burden = company struggling to pay debt
- High debt ratio = company over leveraged
- Low retained earnings = company not profitable

Limitation:
- XGBoost importance shows usage in splits not causation
- A feature can appear important just because it correlates with others
- SHAP values would give better individual explanations

---

## Overfitting Discussion
- Overfit gap: 0.5055 (large, above 0.10 threshold)
- Train PR-AUC: 1.0000, Val PR-AUC: 0.4945
- Model memorizes training data perfectly
- Still generalizes reasonably for risk ranking
- Addressed with regularization in Exp 4 and fewer features in Exp 5
- Test PR-AUC of 0.5238 is actually higher than val PR-AUC confirming model generalizes

---

## AI Usage
- **Tool used:** Claude AI and Codex in VS Code
- **Helped with:** Evaluation function, train/val/test split, plotting code, notebook structure
- **Verified manually:** Test set was never used during model selection or feature selection
- **Mistake caught:** Initially suggested accuracy as main metric, corrected to PR-AUC for imbalanced data
- **Fixed:** Seaborn palette warning by adding hue parameter manually