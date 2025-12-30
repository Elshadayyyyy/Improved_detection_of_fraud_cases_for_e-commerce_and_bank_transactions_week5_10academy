# Fraud Detection for E-commerce and Bank Transactions

## Project Overview
Adey Innovations Inc. aims to improve fraud detection for e-commerce and banking transactions using data-driven and machine learning techniques.  
This project focuses on building accurate, explainable, and robust fraud detection models while addressing challenges such as severe class imbalance and real-world transaction behavior.

## Business Problem
Fraud detection systems must balance two competing risks:
- **False Positives** – Legitimate transactions incorrectly flagged as fraud, harming user experience
- **False Negatives** – Fraudulent transactions that go undetected, leading to financial loss
This project emphasizes:
- Precision–Recall trade-offs  
- Business-aware model evaluation  
- Model explainability  

## Datasets
The following datasets are used:

- Fraud_Data.csv – E-commerce transaction data including user behavior, device, timing, and transaction details.  
- IpAddress_to_Country.csv – Maps IP address ranges to countries for geolocation analysis.  
- creditcard.csv – Bank transaction data with anonymized PCA-transformed features (used in later tasks).  

## Steps Taken
### 1. Data Preparation
- Loaded and explored the datasets.  
- Handled missing values and encoded categorical variables.  
- Split data into training and testing sets.  
- Addressed class imbalance using oversampling techniques (e.g., SMOTE).  
- Converted numeric features to float64 to ensure compatibility with SHAP.  

### 2. Model Building
- The dataset was split into training and testing sets, resulting in 120,889 samples for training and 30,223 samples for testing, each with 15 features.  
- Class imbalance was addressed using SMOTE, producing a perfectly balanced training set with 50% fraud and 50% non-fraud samples.  
- **Random Forest Classifier Results:**  
  - F1-score: 0.601  
  - Precision-Recall AUC: 0.621  
  - Confusion Matrix:
    ```
    [[26679,   714],
     [ 1307,  1523]]
    ```
  This shows strong performance, with relatively low false positives and false negatives.  
- **Logistic Regression Results:**  
  - F1-score: 0.292  
  - Precision-Recall AUC: 0.286  
  - Confusion Matrix:
    ```
    [[19168,  8225],
     [  939,  1891]]
    ```
  This indicates weaker performance, with higher false positives and lower overall predictive ability.  
- Overall, Random Forest significantly outperforms Logistic Regression for fraud detection on this dataset.

### 3. Model Explainability with SHAP
- Initialized a SHAP TreeExplainer on the trained Random Forest model.  
- Generated SHAP values for the test set.  
- Created force plots for:  
  - True Positive (TP) transactions  
  - False Positive (FP) transactions  
  - False Negative (FN) transactions  
- Visualized the contributions of individual features to model predictions.  

### 4. Results
- **True Positives:** High probability predictions driven by source_Direct, sex_M, and time_since_signup.  
- **False Positives:** Incorrect predictions influenced by hour_of_day, day_of_week, browser_FireFox, and source_Direct, despite the negative effect of time_since_signup.  
- **False Negatives:** Missed fraudulent transactions due to time_since_signup, gender, and user_id signals outweighing positive contributions like source_SEO.  

## Interpretation
- SHAP analysis shows that time_since_signup and source are the strongest predictors across all cases.  
- Gender (sex_M) consistently affects predictions, with males more likely flagged as fraudulent in this dataset.  
- Some misclassifications reveal counterintuitive effects, e.g., early login hours or browser choice pushing predictions up incorrectly.  
- Comparing SHAP importance with built-in Random Forest feature importance confirms the top drivers but provides per-transaction insights.  

**Top 5 drivers of fraud predictions:**
1. time_since_signup
2. source_Direct
3. sex_M
4. source_SEO
5. browser_FireFox  

**Surprising findings:**
- Features like hour_of_day and day_of_week can strongly mislead the model in certain edge cases.  

## Business Recommendations
1. **Enhanced verification for secent Signups:**  
   - **SHAP Insight:** time_since_signup is the strongest driver of positive fraud predictions.  

2. **Monitor direct source traffic:**  
   - Users coming from direct sources (source_Direct = 1) have higher fraud risk, so implement extra fraud checks or flag unusual transaction patterns for direct traffic.  

3. **Targeted browser:**  
   - Certain browsers (e.g., Firefox) and login times can increase false positives, so apply adaptive verification rules based on browser and access timing.  

4. **Gender specific risk awareness:**  
   - Male users appear slightly more likely to be flagged; consider this in combined feature checks but avoid discrimination, so focus on combination rules rather than single-feature enforcement.
=======
- **Fraud_Data.csv**  
  E-commerce transaction data including user behavior, device, timing, and transaction details.
- **IpAddress_to_Country.csv**  
  Maps IP address ranges to countries for geolocation analysis.
- **creditcard.csv**  
  Bank transaction data with anonymized PCA-transformed features (used in later tasks).

