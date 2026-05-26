## Overview:
This project builds an end-to-end binary classification system to predict whether a credit card customer will default on their next payment. It covers:

- Exploratory Data Analysis (EDA):distributions, default rates by demographics, correlation heatmaps
- Feature Engineering:3 new domain-informed features derived from billing and repayment history
- Model Training:Logistic Regression and Random Forest, with SMOTE to handle class imbalance
- Evaluation:Classification reports, ROC curves, confusion matrices, and 5-fold cross-validation
- SQL Analysis:Business queries run on the dataset via SQLite
- Fairness Check:False positive/negative rates broken down by gender and education level
- SHAP Explainability:Global beeswarm and per-customer waterfall plots

## Dataset
UCI_Credit_Card.csv

## Pipeline:

1. Data Loading & Cleaning: Renaming target column to "default", dropping ID column and fixing undocumented codes (EDUCATION, MARRIAGE)
2. Exploratory Data Analysis
   - Distribution plots (LIMIT_BAL, AGE, PAY_0)
   - Default rate by sex, education, marriage, age
   - Repayment delay heatmap
   - Correlation heatmap

3. Feature Engineering
   - AVG_UTIL_RATE = avg(BILL_AMTs) / LIMIT_BAL
   - AVG_PAY_RATIO = avg(PAY_AMTi / BILL_AMTi)
   - TOTAL_DELAY_MONTHS = count(PAY_x > 0)
  
4. Model Development:
   - Train/Test Split (80/20, stratified)
   - StandardScaler (fit on train only)
   - SMOTE oversampling (train only)
   - Logistic Regression + Random Forest (100 trees)

5. Evaluation:
   - Classification report
   - AUC-ROC score
   - 5-fold stratified cross-validation
   - ROC curve comparison + confusion matrix
  
6. SQL queries (SQLite in-memory)
7. Fairness audit — FPR/FNR by gender & education
8. SHAP beeswarm + waterfall plots
   





