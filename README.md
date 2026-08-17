# Telco Customer Churn Analysis

## Overview

Telecom customer churn is mainly driven by different factors like customer behavior, service plans, and contract types. In this project, I analyzed customer-level data to uncover the key factors that influence churn and evaluated whether these patterns can be used to predict which customers are most likely to leave.


The workflow combines exploratory data analysis, feature engineering, multivariate analysis, and predictive modeling using Logistic Regression and Random Forest, followed by cross-validation to check stability of results.

---

## Dataset

The dataset contains **7,043 customer records and 21 variables**, covering customer demographics, account information, subscribed services, billing details, and churn status.

Key variables include:

* `tenure` — how long the customer has stayed with the company
* `Contract` — contract type (month-to-month, one year, two year)
* `InternetService` — type of internet service
* `PaymentMethod` — how the customer pays
* `MonthlyCharges` — monthly billing amount
* `TotalCharges` — total amount billed to date
* `TechSupport`, `OnlineSecurity` — value-added services
* `Churn` — whether the customer left

---

## Analysis Approach

### 1. Understanding the Data

The first step was simply to understand what the dataset is actually telling us — structure, distributions, missing values, and whether anything looked inconsistent.

The dataset is clean in terms of duplicates, but required attention around data types (especially `TotalCharges`) and categorical consistency.

---

### 2. Data Cleaning

The main cleaning steps included:

* Converting `TotalCharges` into a numeric format
* Handling missing/blank values in `TotalCharges`
* Standardising service categories like *“No internet service”* and *“No phone service”*
* Converting `SeniorCitizen` into a more interpretable bins format

---

### 3. Exploratory Data Analysis

The EDA revealed several clear patterns in customer churn, with contract type emerging as the strongest predictor.

* Month-to-month customers have significantly higher churn rates than customers on longer-term contracts.
* Tenure is strongly associated with churn, with newer customers showing the highest churn rates.
* Service combinations matter, with fiber-optic customers on month-to-month contracts showing particularly high churn.
* Payment methods and additional services also show differences in churn, although their impact appears weaker than contract type and tenure.

Overall, the analysis suggests that contract type, customer commitment and tenure are key factors behind churn.

---

### 4. Feature Engineering

Feature engineering was used create variables that better represent customer profiles and behaviors.

This included:

* Creating variables that reflect customer engagement and service usage
* Grouping related services into more meaningful categories
* Structuring variables to better reflect customer engagement levels
* Adding a synthetic geographic component (based on population-style weighting) to explore whether location-based patterns.

The goal of enriching the dataset was to make it more representative of real-world customer behavior without adding unnecessary complexity.

---

### 5. Multicollinearity Check

Before modelling, numerical variables were checked for multicollinearity to identify highly overlapping information between the features/variables.

Results showed that some expected relationships exist between variables, but no severe multicollinearity that would impact model interpretation.

---

### 6. Predictive Modelling

Two models were tested to predict customer churn:

* Logistic Regression as the baseline model
* Random Forest as the comparison model

Logistic Regression model used class balancing to account for the lower number of churned customers.

The models were evaluated using

* Accuracy
* Precision
* Recall
* ROC-AUC

The main focus was recall and overall churn prediction, rather than accuract alonce, since correctly identifying customers likely to churn is more valuable for retention efforts.

---

### 7. Cross-Validation

To assess model stability, 5-fold stratified cross-validation was performed.

The Logistic Regression model achieved:

| Metric       | Mean Score |
| ------------ | ---------: |
| Accuracy     |     74.97% |
| ROC-AUC      |      0.843 |
| Churn Recall |      79.4% |

Consistent results across folds suggest that the model performs reliably across different subsets of the data rather than depending heavily on the single train-test split.

---

## Key Findings

### 1. Contract type is the strongest driver of churn

The clearest pattern in the entire dataset is contract structure:

| Contract       | Churn Rate |
| -------------- | ---------: |
| Month-to-month |      42.7% |
| One year       |      11.3% |
| Two year       |       2.8% |

Customers on month-to-month contracts churn at a much higher rate than those on longer-term contracts. This makes contract structure the most prominent churn pattern identified in the analysis.

---

### 2. Churn is concentrated among newer customers
Tenure shows a clear inverse relationship with churn. Customers are more likely to churn during the early stages of customer lifecycle, while the churn rates decrease as tenure increases.

This suggests that the first few months are a critical period for customer retention, making early engagement and onboarding experience important areas for reducing churn.

---

### 3. High-risk customer profile emerges clearly

The analysis reveals a clear high-risk customer profile. Customers are more likely to churn when they have:

* Month-to-month contract
* Fiber optic internet
* Short tenure
* Electronic check payment
* Fewer additional services

These characteristics frequently appear among customers with higher churn rates, suggesting that new, less-engaged customers on flexible contracts may require greater retention attention.

---

### 4. Model interpretation aligns with EDA

The Logistic Regression coefficients support many of the patterns identified during the EDA process.

**Higher churn association:**

* Fiber optic service
* Electronic check payment
* Paperless billing
* Streaming services

**Lower churn association:**

* Long-term contracts
* Tenure
* Online security
* Technical support

These results show that the model is capturing patterns already observed in the EDA. However, these are associations, not causal relationships. They are still useful for identifying customer segments that may require greater retention attention.

---

## Model Selection

Although Random Forest achieved higher accuracy, Logistic Regression performed better at identifying customers who churned.

* Logistic Regression: higher churn recall
* Random Forest: higher accuracy but weaker churn detection

Since the main goal is to identify customers at risk of leaving, Logistic Regression is the more suitable model for this analysis.

In this context, missing a potential churner is more costly than incorrectly flagging a customer who stays.

---

## Business Interpretation

The analysis shows that churn is concentrated in specific customer segments rather than being evenly distributed across the customer base.

The strongest factors to consider are:

* Contract type - strongest churn signal
* Tenure - newer customers are more likely to leave
  Service configuration - particularly fiber customers and their add-on services
* Payment method - certain payment methods are associated with higher churn

This suggests that retention efforts should be targeted toward high-risk customer segments rather than applied equally to all customers.

---

## Tools & Technologies

* Python
* pandas
* NumPy
* Matplotlib
* Seaborn
* scikit-learn
* Jupyter Notebook
* GitHub

---

## Project Structure

```text
telco-churn-analysis/
│
├── README.md
├── Telco_Churn.ipynb
└── data/
    └── telco_churn.csv
```

---

## Notes

The geographic component used in this project is synthetically generated and is included only to enrich segmentation analysis. It does not represent real customer location data.

---
