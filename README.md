# Retail Sales Forecasting and Promotion Effectiveness

## Project Overview

This project focuses on forecasting retail sales using historical weekly sales data and evaluating different forecasting techniques to identify the most suitable model for future demand prediction.

The project compares traditional time-series forecasting methods with a machine learning approach using lag-based and rolling-window features.

## Objectives

- Analyze historical retail sales patterns.
- Perform time-based train-test splitting.
- Compare multiple forecasting techniques.
- Develop a machine learning model for sales forecasting.
- Evaluate models using MAE, RMSE, and MAPE.
- Select the best-performing forecasting model.
- Generate a 13-week future sales forecast.
- Analyze promotion effectiveness as part of the overall retail analytics framework.

## Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Scikit-learn
- Statsmodels
- Joblib
- Jupyter Notebook

## Forecasting Methodology

The following forecasting approaches were evaluated:

1. Naive Forecast
2. 4-Week Moving Average
3. Exponential Smoothing
4. Seasonal Naive
5. ARIMA
6. Random Forest
7. Improved Random Forest

The machine learning models use historical lag features and rolling averages to capture temporal demand patterns.

## Model Features

The final Random Forest model uses:

- Lag 1
- Lag 2
- Lag 3
- Lag 4
- Lag 8
- 4-Week Rolling Average
- 8-Week Rolling Average

## Model Evaluation

Models were evaluated using:

- Mean Absolute Error (MAE)
- Root Mean Squared Error (RMSE)
- Mean Absolute Percentage Error (MAPE)

### Final Model Performance

| Model | MAE | RMSE | MAPE |
|---|---:|---:|---:|
| Naive Baseline | 9066.00 | 11766.36 | 13.81% |
| 4-Week Moving Average | 8515.63 | 11447.59 | 13.59% |
| Exponential Smoothing | 8999.71 | 11217.75 | 14.18% |
| Seasonal Naive | 9240.69 | 11577.37 | 17.06% |
| ARIMA | 9182.32 | 10774.26 | 15.38% |
| Random Forest v1 | 8088.06 | 9775.37 | 13.73% |
| **Improved Random Forest v2** | **8115.24** | **9888.69** | **13.57%** |

The Improved Random Forest v2 model was selected because it achieved the lowest MAPE of **13.57%**.

## Future Forecast

The selected model was used to generate a 13-week future sales forecast.

**Forecast Period:** 11 January 2012 to 4 April 2012

The forecasted demand ranges approximately from **62,754 to 72,955 units** during the forecast period.

## Project Outputs

### Forecast

`outputs/final_13_week_demand_forecast.csv`

Contains the predicted sales/demand for the next 13 weeks.

### Model Comparison

`outputs/model_comparison_results.csv`

Contains the performance metrics of all evaluated forecasting models.

### Trained Model

`models/final_random_forest_model.pkl`

Contains the trained Random Forest model.

## Promotion Effectiveness

The project also includes promotion effectiveness as a key retail analytics component.

Promotion effectiveness can be evaluated by comparing sales during promotional and non-promotional periods, measuring sales uplift, and identifying whether promotional activities contribute to increased demand.

## Project Structure

```text
Tailwyndz_Propel_Assessment3/
│
├── data/
│
├── notebooks/
│   └── Retail_Sales_Forecasting.ipynb
│
├── models/
│   └── final_random_forest_model.pkl
│
├── outputs/
│   ├── final_13_week_demand_forecast.csv
│   └── model_comparison_results.csv
│
├── README.md
└── requirements.txt