# Bank Term Deposit Subscription Prediction

A business-focused machine learning project that predicts which bank customers are more likely to subscribe to a term deposit **before** a marketing call is made.

## Business Goal
Direct marketing campaigns are costly when all customers are contacted equally.  
The goal of this project is to help the bank identify high-potential customers in advance, so campaign resources can be used more efficiently.

## Problem Type
- **Task:** Binary Classification
- **Target:** `deposit`
- **Classes:** `yes`, `no`

## Business Problem
Banks run direct marketing campaigns to promote products such as term deposits. Calling every customer is expensive and inefficient, especially when only a small percentage of customers actually subscribe.

This project uses machine learning to support a practical business decision:

- Which customers should be prioritized for contact?
- How can the bank improve conversion with fewer calls?
- How can marketing resources be used more effectively?

## Dataset
This project uses the `bank` dataset loaded through **PyCaret**.

### Dataset Summary
- **Rows:** 45,211
- **Original Features:** 17
- **Missing Values:** 0
- **Target Distribution:** Imbalanced

### Target Distribution
- `no`: 39,922
- `yes`: 5,289

Only about **11.7%** of customers subscribed, so accuracy alone is not enough for evaluation.

## Why This Project Matters
This is a realistic business use case because the model can directly support **pre-call campaign targeting**.

Instead of contacting all customers equally, the bank can use the model to:
- rank customers by likelihood of subscription
- prioritize high-potential customers
- reduce wasted marketing effort
- improve conversion efficiency

## Important Business Decision
The feature `duration` was excluded from the main model.

### Why was `duration` removed?
Because call duration is only known **after** the call starts, while the decision in this project must happen **before** the call is made.

Including `duration` would create **data leakage** and make the model unrealistic for actual campaign planning.

## Non-ML Baseline
A non-ML approach could use simple manual rules such as:
- prioritizing customers with previous successful contact outcomes
- prioritizing customers with higher balances
- prioritizing older customers
- or contacting all customers equally

This kind of approach is limited because it does not combine all available signals systematically.

## Why Machine Learning?
Machine learning can combine customer demographics, financial status, and campaign history more effectively than manual rules.

It is useful for:
- identifying hidden patterns
- handling interactions between variables
- ranking customers by probability
- improving marketing efficiency

## Data Preparation
The following steps were performed before modeling:
- loaded and inspected the dataset
- checked data types and missing values
- analyzed target imbalance
- performed business-focused exploratory data analysis
- created engineered features
- excluded leakage-prone features
- applied model-based feature selection inside the pipeline

## Exploratory Data Analysis — Key Insights
Several business-relevant patterns appeared during EDA:

- customers with better previous campaign outcomes were more likely to subscribe
- customers with lower debt burden performed better
- some contact methods and months were associated with better outcomes
- age and balance showed useful predictive value
- repeated campaign pressure appeared to be associated with lower conversion

These insights helped shape the final feature engineering and modeling decisions.

## Feature Engineering
To keep the project clean, interpretable, and business-relevant, only a small set of engineered features was retained:

- `age_group`
- `balance_segment`
- `campaign_pressure`
- `debt_profile`

### Why these engineered features?
These features were selected because they:
- make the data easier to interpret from a business perspective
- reduce unnecessary complexity
- capture useful customer segmentation patterns
- support a cleaner modeling story

### Feature Descriptions
- **`age_group`**: groups customers into age segments for clearer customer profiling
- **`balance_segment`**: converts raw balance values into financial bands
- **`campaign_pressure`**: indicates whether the customer was contacted many times in the current campaign
- **`debt_profile`**: combines housing loan and personal loan status into one clearer financial profile

## Feature Selection
Feature selection was handled in two ways:

### 1. Business-driven selection
- `duration` was removed to avoid leakage
- redundant engineered features were removed to keep the final design simple and interpretable

### 2. Model-driven selection
- PyCaret was configured with `feature_selection=True`

This helped reduce noise and improve model quality.

## Modeling Approach
The project used **PyCaret Classification** with the following setup:
- target: `deposit`
- train size: 80%
- cross-validation: **Stratified 5-Fold**
- feature selection enabled
- multicollinearity handling enabled
- class imbalance handling enabled
- `duration` excluded from modeling

## Models Compared
The following models were compared:
- Logistic Regression
- Random Forest
- Gradient Boosting Classifier
- AdaBoost Classifier

### Baseline Model
- **Logistic Regression**

### Final Model
- **Tuned Random Forest**

## Evaluation Metrics
Because the dataset is imbalanced, evaluation focused on more than just accuracy.

The following metrics were considered:
- Accuracy
- AUC
- Precision
- Recall
- F1-score
- Kappa
- MCC

Business-oriented metrics were also used:
- conversion rate among predicted yes customers
- lift
- capture rate
- contact rate

## Final Results

### Baseline — Logistic Regression
#### Cross-Validation
- **Accuracy:** `0.6452`
- **AUC:** `0.5861`
- **Kappa:** `0.0779`
- **MCC:** `0.0929`

#### Holdout
- **Accuracy:** `0.5733`
- **AUC:** `0.5670`

### Final Model — Tuned Random Forest
#### Cross-Validation
- **Accuracy:** `0.7220`
- **AUC:** `0.6419`
- **Kappa:** `0.1359`
- **MCC:** `0.1531`

#### Holdout
- **Accuracy:** `0.7147`
- **AUC:** `0.6448`
- **Precision (`yes`):** `0.1934`
- **Recall (`yes`):** `0.4537`
- **F1-score (`yes`):** `0.2712`

## Business Impact
- **Baseline subscription rate:** `11.70%`
- **Conversion rate among customers predicted as yes:** `19.34%`
- **Lift:** `1.65x`
- **Capture rate of actual subscribers:** `45.37%`
- **Share of customers flagged for contact:** `27.45%`

### Business Interpretation
The model is intended to help the bank focus on a smaller, higher-probability customer segment instead of contacting everyone equally.

Instead of calling all customers, the bank can target about **27.45%** of customers identified by the model and expect a subscription rate of **19.34%**, compared with the natural baseline of **11.70%**.

This makes the model useful as a **lead prioritization tool**.

## Confusion Matrix Summary
For the final model on the holdout set:
- **True Negatives:** `5983`
- **False Positives:** `2002`
- **False Negatives:** `578`
- **True Positives:** `480`

This confirms that the model is stronger at identifying non-subscribers than subscribers, which is common in imbalanced classification problems. However, it still provides meaningful lift and useful ranking value.

## Most Important Features
The final model highlighted several important features, such as:
- `pdays`
- `age`
- `day`
- `balance`

These suggest that customer history, timing, age, and financial status all play meaningful roles in predicting term deposit subscription.

## How the Model Can Be Used
The model can be used in two practical ways:

### 1. Binary Targeting
Classify customers as likely or unlikely to subscribe.

### 2. Probability Ranking
Rank customers by predicted probability and prioritize the top-scoring customers for outreach.

The second use case is especially valuable for campaign planning because it helps the bank target fewer customers more effectively.

## Project Structure
```text
.
├── notebook.ipynb
├── README.md
├── bank_deposit_model.pkl
├── top_leads.csv
└── slides.pptx
