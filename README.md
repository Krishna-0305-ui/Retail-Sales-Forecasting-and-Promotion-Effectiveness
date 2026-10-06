# Retail Sales Forecasting and Promotion Effectiveness

## Project Overview

This project develops a machine learning based retail sales forecasting system to predict weekly product-store demand and evaluate the impact of pricing and promotional activities.

The project uses historical retail transaction data containing product, store, sales, price, and promotion information. A product-store level forecasting dataset was created and evaluated using a time-based 13-week holdout period.

The project combines:

- Data quality analysis
- Exploratory data analysis
- Product and store data integration
- Time-series feature engineering
- Baseline forecasting
- Random Forest forecasting
- Feature importance analysis
- Ablation study
- Price scenario analysis
- Forecast uncertainty estimation

---

## Objectives

1. Identify and address data-quality issues.
2. Create a weekly product-store modelling dataset.
3. Develop leakage-safe demand forecasting features.
4. Analyze the relationship between price, discounts, promotions, and unit sales.
5. Compare a naive forecasting baseline with a machine learning model.
6. Forecast weekly product-store demand for a 13-week holdout period.
7. Estimate forecast uncertainty using prediction intervals.
8. Identify the most important forecast drivers.
9. Evaluate the contribution of price, promotion, product, and store features.
10. Estimate demand changes under a price-reduction scenario.

---

## Dataset

The project uses the Dunnhumby "Breakfast at the Frat" retail dataset.

### Main data sources

- `dh Transaction Data`
- `dh Products Lookup`
- `dh Store Lookup`

### Transaction information

The transaction dataset contains:

- Week end date
- Store number
- UPC
- Units sold
- Visits
- Households
- Spend
- Price
- Base price
- Feature promotion
- Display promotion
- TPR promotion

### Product information

Product attributes include:

- Description
- Manufacturer
- Category
- Sub-category
- Product size

### Store information

Store attributes include:

- Store name
- City
- State
- Store segment
- Parking capacity
- Sales area
- Average weekly baskets

---

## Data Quality and Preparation

The transaction dataset contains 524,950 records covering 156 weekly periods from January 2009 to January 2012.

Data-quality checks included:

- Missing values
- Duplicate transaction keys
- Product mapping validation
- Store mapping validation
- Invalid prices
- Zero sales
- Product-store coverage
- Duplicate store identifiers

Two store IDs had conflicting segment information. Instead of duplicating transactions or arbitrarily selecting a segment, these stores were marked as ambiguous during store lookup cleaning.

The final product-store dataset contained 524,950 transaction-level records with complete product and store mappings.

---

## Product-Store Forecasting Dataset

The forecasting problem was defined at the:

**Product × Store × Week**

level.

There were 3,909 product-store combinations in the complete historical dataset.

The final evaluation population contained:

- **2,492 product-store combinations**
- **13 test weeks**
- **32,396 product-store-week observations**

The final 13-week holdout period was:

**12 October 2011 – 4 January 2012**

Training data covered the preceding 143 weeks.

---

## Feature Engineering

Leakage-safe historical demand features were created separately for every product-store combination.

### Historical demand features

- Lag 1 week
- Lag 2 weeks
- Lag 3 weeks
- Lag 4 weeks
- Lag 8 weeks
- 4-week rolling average
- 8-week rolling average

Rolling features were calculated using previous observations only to prevent target leakage.

### Price and promotion features

- Price
- Base price
- Discount percentage
- Feature promotion
- Display promotion
- TPR promotion

### Product features

- UPC
- Category
- Sub-category
- Product size

### Store features

- Store number
- Store segment
- Parking capacity
- Sales area
- Average weekly baskets

### Calendar features

- Week of year
- Month

---

## Forecasting Models

### 1. Product-Store Naive Baseline

The previous week's unit sales were used as the forecast.

Results:

| Metric | Naive |
|---|---:|
| MAE | 9.84 |
| RMSE | 25.39 |
| MAPE | 63.96% |

### 2. Random Forest

A Random Forest regression model was trained using historical demand, price, promotion, product, store, and calendar features.

Model configuration:

- 200 trees
- Maximum depth: 12
- Minimum samples per leaf: 2
- Random state: 42

Results:

| Metric | Random Forest |
|---|---:|
| MAE | **6.53** |
| RMSE | **13.44** |
| MAPE | **52.51%** |

The Random Forest substantially outperformed the naive baseline.

---

## Model Improvement

Compared with the naive baseline:

- MAE improved by approximately **33.6%**
- RMSE improved by approximately **47.1%**
- MAPE improved by approximately **17.9%**

This indicates that product-store demand patterns can be forecast more accurately by incorporating historical demand, price, promotion, and product/store information.

---

## Feature Importance

The most important Random Forest features were:

| Feature | Importance |
|---|---:|
| LAG_1 | 47.18% |
| ROLLING_8 | 13.94% |
| FEATURE | 10.44% |
| DISCOUNT_PCT | 5.49% |
| PRICE | 5.05% |
| ROLLING_4 | 3.21% |
| LAG_8 | 2.34% |
| BASE_PRICE | 1.59% |

Recent sales history was the strongest predictor of future demand, followed by longer-term demand patterns and promotional activity.

Feature importance represents predictive contribution and should not be interpreted as causal impact.

---

## Ablation Study

An ablation study was performed to evaluate the contribution of different feature groups.

| Model | MAE | RMSE | MAPE |
|---|---:|---:|---:|
| Historical only | 8.81 | 23.12 | 64.98% |
| Historical + Price | 7.24 | 17.38 | 54.81% |
| Historical + Price + Promotion | 6.58 | 14.05 | **51.81%** |
| Full Model | **6.53** | **13.45** | 52.49% |

The results show that price and promotion information substantially improves forecasting performance.

The promotion-enhanced model achieved the lowest MAPE, while the full model achieved the lowest MAE and RMSE.

---

## Price Scenario Analysis

A model-based scenario analysis was conducted by reducing product price by 10% while keeping other conditions unchanged.

### Results

- Baseline predicted demand: **23.14 units**
- Scenario predicted demand: **26.45 units**
- Average demand increase: **3.31 units**
- Estimated percentage increase: **13.75%**

This is a model-based scenario estimate and should not be interpreted as a causal price elasticity estimate.

---

## 13-Week Forecast

The final forecasting system generated:

- **2,492 product-store combinations**
- **13 forecast weeks**
- **32,396 forecasts**

Each forecast includes:

- Product/store identifiers
- Forecasted units
- Lower uncertainty bound
- Upper uncertainty bound

The uncertainty bounds provide an empirical estimate of forecast uncertainty based on the variation across Random Forest trees.

For product-store combinations with insufficient historical observations for recursive Random Forest forecasting, a historical-average fallback was used.

---

## Business Recommendations

### 1. Use recent sales history for inventory planning

The strong importance of lagged demand indicates that recent product-store sales should be incorporated into replenishment decisions.

### 2. Use promotion information in demand planning

Feature and discount variables materially improved forecast accuracy. Promotion calendars should therefore be incorporated into forecasting workflows.

### 3. Evaluate price changes using scenario analysis

The model can be used to simulate potential pricing changes before implementation.

### 4. Focus on product-store combinations

Demand varies across both products and stores. Forecasting at this level provides more actionable information than relying only on overall aggregate demand.

### 5. Use uncertainty for inventory decisions

Prediction intervals can help identify forecasts with higher uncertainty and support safety-stock decisions.

---

## Limitations

- The dataset represents a historical retail environment and may not reflect current customer behavior.
- MAPE can become unstable when actual unit sales are very small.
- Random Forest feature importance indicates predictive importance rather than causality.
- The price scenario is a model-based simulation and is not a causal experiment.
- Some product-store combinations have limited historical coverage.
- Future price and promotion inputs are assumed to be available for forecasting scenarios.
- Prediction intervals based on Random Forest tree variation are approximate and are not formally calibrated probabilistic intervals.

---

## Project Structure

```text
Retail-Sales-Forecasting-and-Promotion-Effectiveness/
│
├── data/
│
├── models/
│
├── notebooks/
│   └── Retail_Sales_Forecasting.ipynb
│
├── outputs/
│   ├── ablation_results.csv
│   ├── final_13_week_demand_forecast.csv
│   ├── final_product_store_13_week_forecast.csv
│   ├── model_comparison_results.csv
│   ├── price_reduction_scenario_analysis.csv
│   ├── product_store_feature_importance.csv
│   └── product_store_model_comparison.csv
│
├── presentation/
│
├── reports/
│
├── src/
│
├── requirements.txt
└── README.md