# Retail Churn Prediction

Analysis of small business e-commerce/retail transaction data to predict customer 
churn, using real POS and web order history from 
a family-owned retail store.

## Overview

This project explores the following question using transaction-level retail data:
**Which customers are unlikely to return** within a 6-month window? (churn prediction)

## Data

Data consists of ~2 years of transaction-level order data (line-item granularity) 
from a small retail business, including order IDs, dates, customer billing names, 
product categories, and order totals.

**Note:** Raw data is not included in this repository due to customer privacy. 
Code assumes a similarly structured dataset (see Data Dictionary below).

## Data Quality Issues Identified & Corrected

Several data quality issues surfaced during exploration and required correction 
before modeling:

- **Line-item duplication:** Data was stored one row per line item, with order-level 
  fields (total, customer name) repeated across every item in an order. Naive 
  aggregation without deduplication inflated customer spend totals by up to ~31x 
  for customers with multi-item orders.
- **Promotional $0 orders:** ~241 free/promotional items were logged as separate 
  zero-dollar orders rather than as part of the original transaction, artificially 
  inflating order counts and distorting purchase-interval calculations. These were 
  filtered out.
- **POS system migration:** The business began logging all in-store transactions 
  through this POS system starting January 2026. Prior to this, in-store activity 
  is undercounted in the dataset. This is a known limitation — churn estimates may 
  be less reliable for customers whose primary channel was in-store before the migration.

## Approach: Churn Prediction

- **Feature window:** Jan 2024 – Dec 2025 (customer behavior calculated only from this period)
- **Outcome window:** Jan 2026 – Jun 2026 (used only to determine whether a customer returned)
- **Features:** recency, tenure, order frequency, total spend, average order value, 
  category diversity, purchase interval variability, order value trend, single-order flag
- **Models compared:** Logistic Regression vs. Random Forest

### Results

| Model | Precision (retained) | Recall (retained) | ROC-AUC |
|---|---|---|---|
| Logistic Regression | 0.35 | 0.65 | **0.746** |
| Random Forest | 0.34 | 0.53 | 0.690 |

Logistic Regression outperformed Random Forest on both recall and AUC, suggesting 
the relationship between customer behavior and churn risk in this dataset is largely 
linear. Random Forest's added flexibility did not improve results, likely due to 
limited nonlinear structure in the data and a relatively small training set (940 customers).

**Top predictive features:** recency, total spend, average order value, and tenure 
were the strongest predictors. Engineered behavioral features (purchase interval 
variability, single-order flag) contributed minimally — likely because 84% of 
customers in this dataset placed only a single order in the feature window.

### Limitations

- 84% of customers in the feature window were single-order buyers, meaning this 
  model is better understood as predicting "likelihood of any return purchase" 
  rather than churn from an established repeat-purchase relationship.
- Only 215 non-churned examples exist in the full dataset, limiting confidence in 
  evaluation metrics.
- Findings may be affected by the POS migration described above.


## Tech Stack

Python (pandas, scikit-learn, scipy), Jupyter/Colab
