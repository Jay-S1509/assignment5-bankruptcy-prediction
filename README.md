# Assignment 5: Bankruptcy Prediction

## Student
Name: Jayanth

## Problem
Binary classification to predict if a company will go bankrupt.

## Setup Instructions
Install required libraries:
pip install pandas numpy scikit-learn xgboost matplotlib seaborn

## How to Run
1. Open Jayanth_Assignment_5.ipynb in VS Code
2. Run all cells from top to bottom
3. Make sure data.csv is in the same folder

## Dataset
- Source: https://github.com/bipin-a/ml-workshop
- Rows: 6819, Columns: 96
- Target: Bankrupt? (1=bankrupt, 0=not bankrupt)
- Class imbalance: 3.23% bankrupt

## Results
- Best Model: XGBoost with Imbalance Handling
- Val PR-AUC: 0.4945
- Val Brier Score: 0.0238
- Threshold: 0.3

## Key Libraries
- pandas, numpy
- scikit-learn
- xgboost
- matplotlib, seaborn
