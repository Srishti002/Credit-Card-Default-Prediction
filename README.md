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

## Results: 
### Model Performance: 
<img width="583" height="412" alt="image" src="https://github.com/user-attachments/assets/397f1e4d-c526-407d-a82b-93bf9f99a1d0" />

Random Forest outperforms Logistic Regression on AUC-ROC and overall classification metrics.

### Top 5 Predictive Features (by Correlation & Random Forest Importance)
1. PAY_0              — Most recent repayment delay status
2. PAY_2              — Repayment delay 2 months ago
3. PAY_3              — Repayment delay 3 months ago
4. TOTAL_DELAY_MONTHS — Engineered: total months with delay
5. AVG_PAY_RATIO      — Engineered: how much of each bill was paid

### Confusion Matrix ( Random Forest):
<img width="526" height="477" alt="image" src="https://github.com/user-attachments/assets/e254bfaf-94a3-4d2d-8e29-0887f35cb111" />


## Installation:

1. Clone the repo
2. Install dependencies: pip install -r requirements.txt
3. Download the dataset : UCI_Credit_Card.csv
4. Open *Credit_card_default.ipynb* adn run cell by cell

## Feature Engineering Details: 

Three new features were created to improve model performance:
1. AVG_UTIL_RATE: Average Credit Utilization Rate
   
   AVG_UTIL_RATE = mean(BILL_AMT1..6) / LIMIT_BAL
   
   Measures what fraction of the credit limit is typically used. High values indicate the customer is close to their limit.

2. AVG_PAY_RATIO: Average Payment Ratio

   AVG_PAY_RATIO = mean(PAY_AMTi / BILL_AMTi)  for each month where BILL > 0

   Captures how consistently a customer pays their bills. A low ratio means they're only making minimum payments.

3. TOTAL_DELAY_MONTHS: Total Months with Payment Delay

   TOTAL_DELAY_MONTHS = count(PAY_x > 0)

   Counts how many of the 6 months had a recorded payment delay. A direct signal of repayment behavior.

## Fairness Analysis: 

<img width="1192" height="393" alt="image" src="https://github.com/user-attachments/assets/ffed4a03-9d0b-482d-a21f-8dd80ff901ec" />


The model was audited for demographic fairness by computing:

- False Positive Rate (FPR): Among people who did NOT default, how many were wrongly flagged?
- False Negative Rate (FNR): Among people who DID default, how many were missed?

These were computed separately for:
- Gender (Male vs Female)
- Education Level (Graduate / University / High School / Other)



## SHAP Explainability: 

<img width="792" height="639" alt="image" src="https://github.com/user-attachments/assets/dcf031a4-ee93-45ba-99af-c829cee1026b" />

<img width="778" height="911" alt="image" src="https://github.com/user-attachments/assets/2baa50b9-f8b9-48cb-8548-435b729ee15d" />


SHAP was used to make the Random Forest interpretable:

- Beeswarm Plot: Shows which features push predictions toward or away from default across the entire test set.
- Waterfall Plot: Explains a single customer's prediction step-by-step, showing which features contributed most to their risk score.

## SQL Insights: 
Three SQL queries were run on the dataset using an in-memory SQLite database:

- Default rate by education level :Which education group defaults most?
- nProfile of defaulters vs non-defaulters: Average credit limit and age.
- Highest-risk segments: Marriage × Education combinations with the highest default rates.

- <img width="793" height="352" alt="image" src="https://github.com/user-attachments/assets/4aa3f632-6d69-4d5d-9c0b-38c7bfdf38e4" />




   

   



   





