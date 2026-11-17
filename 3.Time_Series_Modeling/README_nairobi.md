# Nairobi Air Quality: PM2.5 Forecasting

This project analyzes and forecasts PM2.5 levels in Nairobi using time series data from MongoDB and Python (pandas, scikit-learn, statsmodels, and matplotlib). It has 4 notebooks that should be run in order.

## Data

The `air-quality.nairobi` collection in MongoDB contains **10,000 documents** from **10 sensors** (`nairobi_001` to `nairobi_010`), with one measurement type (`P2` = PM2.5).

| Field | Description |
|---|---|
| `sensor_id` | Sensor ID |
| `timestamp` | Measurement time |
| `value` | PM2.5 level |
| `value_type` | Measurement type (`P2`) |
| `location` | Sensor coordinates (`lat`, `lon`) |

After cleaning, the `pm25` time series uses Nairobi time (UTC+3) and covers 2024-01-01 to 2025-02-20. The modeling analysis uses data from **2024** (8,781 rows).

## Notebook Workflow

### 1. Getting and Cleaning Data (MongoDB)
- Connect to MongoDB with `pymongo` and explore the databases, collections, and sensors (`count_documents`, `distinct`, and `$group` aggregation).
- The `wrangle_data()` function in `mongo_wrangle.py` gets `P2` data, converts timestamps to datetime, changes UTC to `Africa/Nairobi` time, removes missing values, and renames the value column to `pm25`.
- For one sensor (953 readings), the average PM2.5 level is about 39.8, with no missing values.

### 2. Linear Regression with a Lag Feature
- Use the `lag_1` feature (the previous value) and split the data in time order, 80:20 (7,024 for training, 1,756 for testing).
- Baseline ("previous value"): MAE of 9.26. Model: test MAE of **7.46** (about 19.5% better), but test R² is only **0.07**.

### 3. Autoregressive (AR) Model
- Calculate ACF and PACF manually (without statsmodels).
- AR(5) with `AutoReg` and walk-forward validation (30 predictions): MAE of **7.82** (15.5% better than the baseline).
- Compare AR orders 1, 3, 5, and 7: AR(7) performs slightly better (MAE of 7.76, with the lowest AIC and BIC).

### 4. ARMA Model
- Use ARMA(2,1) with `ARIMA(order=(2,0,1))` to forecast 100 hours ahead: MAE of **7.94** (14.2% better than the baseline), with a negative R².
- AIC: ARMA(2,1) at 51,347 performs better than AR(1) at 51,468 and MA(1) at 51,590.

## Results Summary

| Model | Test MAE | Evaluation method |
|---|---|---|
| Baseline (previous value) | 9.26 | All test data |
| Linear regression (lag 1) | 7.46 | 1 step, 1,756 data points |
| AR(5) | 7.82 | 1-step walk-forward, 30 data points |
| ARMA(2,1) | 7.94 | 100 steps at once, 100 data points |

## Conclusion

All models perform about 14% to 20% better than the baseline, but R² is close to zero. This means that previous PM2.5 values explain only a small part of the changes. Forecasts tend to be smooth and move toward the average. Daily patterns (seasonality) and other variables are not included, and should be considered to improve results.

## Notes and Limitations
- **The MAE values above are not directly comparable** because the evaluation methods differ (test data size and prediction method). A fair comparison needs the same test data, prediction method, and metric.
- The final comparison table in notebook 4 uses **estimated** values for linear regression and AR, not calculated results. Do not use it to choose the best model.
- The raw data combines readings from 10 sensors in one time series. This means "lag" refers to the previous row in time order, which may be from a different sensor. The data should be averaged by hour or filtered by sensor.
- AR(5) and ARMA(2,1) were chosen as examples, while AIC and BIC favor higher orders.

## How to Run

```bash
pip install pandas numpy matplotlib scikit-learn statsmodels pymongo
```

Set up MongoDB (default: `localhost:27017`), then run the notebooks in order (1 to 4). Notebooks 2 to 4 use `from mongo_wrangle import wrangle_data`.
