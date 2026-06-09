# Inventory Forecasting Backend (API Contract)

This document defines the backend forecasting system APIs, response formats, and rules.

It is intended for frontend integration only.

No frontend assumptions are made beyond API consumption.

---

# 🚀 BASE URL

http://127.0.0.1:8000/api/

---

# 📊 CORE FORECASTING ENDPOINT

## 1. Forecast Product Line

GET /forecast/<product_line>/

### Example
/api/forecast/Food%20and%20beverages/

---

## 📦 RESPONSE FORMAT

All responses strictly follow this schema:

{
  "product_line": string,

  "best_model": string,

  "score": float,

  "metrics": {
    "MAE": float,
    "RMSE": float,
    "SMAPE": float
  },

  "predictions": [float, float, ...],

  "all_models": {
    "ModelName": float,
    "ModelName": float
  }
}

---

# 📌 FIELD DEFINITIONS

## product_line
Name of the dataset category used for forecasting.

---

## best_model
Model selected by backend as best performer based on evaluation score.

Possible values:
- Naive
- Moving Average
- ARIMA
- SARIMA
- XGBoost

---

## score
Final evaluation score used for model selection.
Lower score = better performance.

---

## metrics

Evaluation metrics for the selected best model:

- MAE → Mean Absolute Error
- RMSE → Root Mean Squared Error
- SMAPE → Symmetric Mean Absolute Percentage Error

All values are numeric floats.

---

## predictions

Array of forecast values representing future demand.

Example:
[605.35, 594.32, 354.20, ...]

Length depends on forecasting horizon configured in backend.

---

## all_models

Comparison scores for all evaluated models.

Example:
{
  "Naive": 4.67,
  "Moving Average": 4.40,
  "ARIMA": 4.31,
  "SARIMA": 4.34,
  "XGBoost": 4.25
}

---

# ⚠️ API RULES

1. All responses are JSON only
2. No HTML or UI formatting is returned
3. All numeric values are floats
4. Missing models return:
   "error": "message"
5. Product names must be URL encoded
6. API is stateless (no session dependency)

---

# 🔁 EXAMPLE FULL RESPONSE

GET /api/forecast/Food%20and%20beverages/

{
  "product_line": "Food and beverages",
  "best_model": "XGBoost",
  "score": 4.25,

  "metrics": {
    "MAE": 350.31,
    "RMSE": 425.20,
    "SMAPE": 32.58
  },

  "predictions": [605, 594, 354, 673],

  "all_models": {
    "Naive": 4.67,
    "Moving Average": 4.40,
    "ARIMA": 4.31,
    "SARIMA": 4.34,
    "XGBoost": 4.25
  }
}

---

# 📡 OPTIONAL FUTURE ENDPOINTS

These may be added later without breaking existing API:

## Get all product lines
GET /api/products/

## Get forecasts for all products
GET /api/forecast/all/

## Forecast with horizon control
GET /api/forecast/<product_line>?steps=30

---

# 🧠 BACKEND BEHAVIOR

- Models are evaluated on each request
- Best model is selected dynamically
- No persistence layer required for predictions
- Dataset: supermarket_sales.csv

---

# 🔒 STABILITY GUARANTEE

This API contract will remain stable unless:

- A versioned API is introduced (/api/v2/)
- New models are added (non-breaking)
- Metrics are extended (backward compatible)
