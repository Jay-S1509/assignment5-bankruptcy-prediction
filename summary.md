# Assignment 5 Summary: Bankruptcy Prediction
**Student:** Jayanth  
**Date:** April 24, 2026

---

## Problem Statement
- Predict if a company will go bankrupt (binary classification)
- Positive class: 1 = Bankrupt
- Missing a bankrupt company is worse than false alarm

---

## Dataset Summary
- File: data.csv
- Rows: 6819, Columns: 96
- Target: Bankrupt?
- No missing values, no duplicates

---

## Class Imbalance
- 220 bankrupt (3.23%) vs 6599 not bankrupt (96.77%)
- Dataset is highly imbalanced
- Accuracy is misleading here

---

## Metric Explanation
- **PR-AUC:** Main metric, focuses on positive class, better than ROC-AUC for imbalanced data
- **Brier Score:** Measures probability quality, lower is better
- **Accuracy:** NOT used as main metric, misleading for imbalanced data
- **Precision/Recall/F1:** Secondary metrics, describe one operating point only
- Thresholded metrics do not describe overall model quality

---

## Feature Selection
- Feature Set A: 95 features (all usable features)
- Feature Set B: Top 25 features from XGBoost importance
- Reduction of 70 features
- Feature Set B slightly lower PR-AUC (0.4452 vs 0.4945)
- Feature Set A performs better so winner uses Set A

---

## Experiment Results Table

| exp_id | model | feature_set | train_pr_auc | val_pr_auc | overfit_gap | val_roc_auc | val_brier | threshold | val_precision | val_recall | val_f1 | selected_finalist |
|--------|-------|-------------|-------------|-----------|-------------|-------------|-----------|-----------|--------------|-----------|--------|------------------|
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
- Best Brier Score tied (0.0238)
- Used scale_pos_weight to handle imbalance
- Lower threshold (0.3) improves recall
- Catching bankrupt companies matters more in business context

---

## Final Test Results
- Test PR-AUC: (your value)
- Test ROC-AUC: (your value)
- Test Brier: (your value)
- Test Precision: (your value)
- Test Recall: (your value)
- Test F1: (your value)
- Threshold: 0.3

---

## Confusion Matrix
- True Negatives: correctly identified safe companies
- False Positives: safe companies flagged as risky
- False Negatives: missed bankrupt companies
- True Positives: correctly caught bankrupt companies

---

## Calibration Result
- Brier Score: 0.0238 (very good)
- Model probabilities are reasonably calibrated
- Higher predicted probabilities correspond to higher bankruptcy rates
- Slight overconfidence at extreme probability values

---

## Feature Importance
- Top feature: most important financial ratio
- Makes business sense as financial ratios predict bankruptcy
- Limitation: importance shows usage in splits not causation

---

## Overfitting Discussion
- Overfit gap: 0.5055 (large)
- Train PR-AUC: 1.0000, Val PR-AUC: 0.4945
- Model memorizes training data
- Still generalizes well enough for risk ranking
- Addressed with regularization and scale_pos_weight

---

## AI Usage
- **Tool used:** Claude AI and Codex in VS Code
- **Helped with:** Evaluation function, train/val/test split, plotting code, notebook structure
- **Verified manually:** Test set was never used during model selection
- **Mistake caught:** AI suggested accuracy as main metric, corrected to PR-AUC
- **Fixed:** Seaborn palette warning by adding hue parameter
