# BSE Stock Volatility Analysis and Forecasting (Ambuja Cement & Suzlon)

This project builds a full workflow for analyzing and forecasting volatility in Indian stocks (BSE). It collects daily price data, stores it, calculates returns, measures volatility, and forecasts it using a GARCH(1,1) model.

## Goals
- Build a clean and validated stock data pipeline.
- Understand returns and volatility for Ambuja Cement (AMBUJACEM.BSE) and Suzlon (SUZLON.BSE).
- Forecast daily volatility for Ambuja Cement.

## Project Workflow

| Stage | Description |
|---|---|
| 1. Data Collection and Exploration | Daily data from Alpha Vantage (the API is simulated with `mock_alpha`) is converted to a time-based DataFrame and saved to CSV. The notebook calculates log returns, summary statistics, skewness, kurtosis, and 21-day rolling volatility. |
| 2. Modular Data Pipeline | Two custom classes are used: `AlphaVantageAPI` (gets data) and `SQLRepository` (saves and reads data from SQLite). The prices and returns of both stocks are compared. |
| 3. Volatility Modeling | Calculate daily and annual volatility, 50-day rolling volatility, and ACF/PACF for squared returns. Then use a GARCH(1,1) model (from the `arch` library), walk-forward validation, and a 10-day volatility forecast. |

## Tools and Libraries
Python, pandas, NumPy, matplotlib, statsmodels, arch, SQLite, requests.

## Main Findings
- Average returns are close to zero, but they vary widely and have heavy tails (Ambuja has very high excess kurtosis).
- Volatility is not constant and appears in clusters (*volatility clustering*), so GARCH is suitable.
- Suzlon is riskier: its annual volatility is about 57.4%, compared with about 31.3% for Ambuja.
- The GARCH(1,1) model was trained on 2,000 observations (AIC 8169.74; BIC 8192.14). Its 10-day daily volatility forecast ranges from 1.82% to 1.90%.

## Validation
Each stage uses `assert` checks and checkpoints for data types, date indexes, column names, missing values, and data length. The model is evaluated with walk-forward validation (500 retraining steps, each predicting one day ahead).

## Limitations and Next Steps
- There are extreme returns (about -203% for log returns), possibly due to corporate actions or unadjusted data. Check these values and consider using adjusted close prices.
- In notebook 2, `read_table(limit=2500)` only reads the first 2,500 rows (the earliest period), not the latest data.
- The model uses a Normal distribution. A Student-t distribution could be tried to better handle heavy tails.
- The evaluation is still visual. Add quantitative measures such as RMSE or a test of volatility interval coverage.

## How to Run
1. Install the dependencies: `pip install pandas numpy matplotlib statsmodels arch requests`.
2. Make sure the `mock_alpha` and `data` modules (with `AlphaVantageAPI` and `SQLRepository`) are in the project folder.
3. Run the notebooks in order, following the stages above.
