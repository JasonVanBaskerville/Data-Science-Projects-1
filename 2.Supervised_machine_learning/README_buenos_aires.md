# Buenos Aires Real Estate: Apartment Price Prediction

This machine learning project predicts apartment prices in Capital Federal, Buenos Aires, using Python (pandas, scikit-learn, matplotlib, and seaborn). It has 4 notebooks that should be run in order.

## Data

Five CSV files (`buenos-aires-real-estate-*.csv`) are combined into 43,029 rows and 16 columns, then cleaned with the `clean_files()` function in the `wrangle.py` module.

| Final column | Description |
|---|---|
| `price_aprox_usd` | Price in USD (prediction target) |
| `surface_covered_in_m2` | Building area (m²) |
| `lat`, `lon` | Location coordinates |
| `neighborhood` | Neighborhood name (category) |

Final result: **6,247 rows, 5 columns, and no missing values**.

## Notebook Workflow

### 1. Data Wrangling
- Combine the five CSV files using `glob` and `pd.concat`.
- Keep only apartments in Capital Federal priced below 400,000 USD.
- Remove building-area outliers (values below the 10th or above the 90th percentile).
- Get `lat` and `lon` from `lat-lon`, and `neighborhood` from `place_with_parent_names`.
- Remove columns that are mostly empty (`floor`, `expenses`, `rooms`), duplicate price or target leakage columns, columns with only one value, ID columns, and `surface_total_in_m2` (highly correlated with building area).
- Split the data 80:20 (4,997 for training, 1,250 for testing, `random_state=42`).

### 2. Simple Linear Regression (One Feature)
- Feature: building area. Target: price.
- Baseline (average price): MAE of 44,516 USD. Model: test MAE of **30,177 USD** and test R² of **0.47**.
- Coefficient: about **2,282 USD per m²**.
- The training and test scores are almost the same, so the model is not overfitting. However, area alone does not explain prices well enough.

### 3. Ridge and Lasso (All Features)
- Pipeline: `OneHotEncoder` for `neighborhood`, `StandardScaler`, then Ridge or Lasso (59 features after encoding).
- Test RMSE drops to about **33,050 to 33,090 USD** (from 41,814 USD with the one-feature model).
- Ridge keeps all features, while Lasso removes weak features.
- The features with the largest positive effect on price are area, Palermo, Puerto Madero, latitude, and Belgrano. Features with a negative effect are Balvanera, Once, Boca, Congreso, and Constitución.

### 4. Model Comparison with Cross-Validation
- Models are compared using the same test data:

| Model | Test RMSE (USD) |
|---|---|
| Baseline (average) | 57,576 |
| Linear regression | 33,088 |
| Ridge (alpha 43.29 from `RidgeCV`) | 33,071 |
| Lasso (alpha 32.75 from `LassoCV`) | 33,069 |

- Lasso is the recommended model because it has the lowest RMSE and is slightly simpler.

## Conclusion

Building area is the strongest factor, and adding location (coordinates and neighborhood) clearly improves predictions. Ridge, Lasso, and standard linear regression perform almost the same (about 20 USD apart), so regularization does not provide a big benefit because there are many more data points than features. According to the notebook summary, the funnel-shaped residuals show that linear models have limits when predicting prices for expensive apartments.

## Notes and Limitations
- The data includes only apartments in Capital Federal priced below 400,000 USD, with areas between the 10th and 90th percentiles. The results may not apply to other properties.
- The final model was chosen based on a very small difference, so the three models are practically equal.
- An error of about 33,000 USD is still large, so use caution if these results are used to make real decisions.

## How to Run

```bash
pip install pandas numpy matplotlib seaborn scikit-learn category_encoders missingno
```

Put the CSV files in the `data/` folder and run the notebooks in order (1 to 4). Notebooks 2 to 4 use `from wrangle import clean_files`.
