# Company Bankruptcy Prediction (Poland and Taiwan)

This machine learning project predicts company bankruptcy based on financial ratios. The data comes from two countries: Poland (9,977 companies, 64 features from `feat_1` to `feat_64`) and Taiwan (6,137 companies, 95 features). The data is highly imbalanced: only about 4.7% of companies in Poland and 3.1% in Taiwan went bankrupt.

## Project Workflow

1. **Data Wrangling**: Read JSON/JSON.gz files, extract records, convert them to a DataFrame, standardize column names, and convert values to numbers. The process is wrapped in a reusable `wrangle()` function, with schema checks and an imbalance ratio calculation.
2. **Baseline and Metric Evaluation**: Compare `DummyClassifier` and Logistic Regression. High accuracy (about 97%) is misleading because the model does not detect any bankrupt companies (recall = 0). Evaluation uses precision, recall, F1, ROC-AUC, and PR-AUC, and tests different thresholds.
3. **Handling Imbalanced Data**: Use median imputation (instead of `dropna`), `class_weight="balanced"`, and Random Oversampling on the training data only. Random Forest raises ROC-AUC to about 0.84, but shows overfitting (training score 1.0).
4. **Gradient Boosting and Tuning**: Use `GradientBoostingClassifier` with `GridSearchCV` (ROC-AUC scoring). Train the best model on the oversampled data.
5. **Deployment**: Save the model with `pickle` (`models/poland_gradient_boosting.pkl`). The `predictor.py` file provides the `predict_bankruptcy_prob()` function to predict bankruptcy probabilities from new data files.

## Main Results (Poland Data, Test Set)

| Model | Precision | Recall | F1 | ROC-AUC |
|---|---|---|---|---|
| Logistic Regression | 0.00 | 0.00 | 0.00 | 0.565 |
| Random Forest | 0.64 | 0.06 | 0.11 | 0.840 |
| Gradient Boosting (baseline) | 0.26 | 0.65 | 0.37 | 0.883 |
| **Gradient Boosting (tuned)** | **0.38** | **0.58** | **0.46** | **0.894** |

## Conclusion

- For imbalanced data, accuracy is not enough. Use recall, F1, and ROC/PR-AUC.
- Gradient Boosting with oversampling performs best: it detects about 58% of bankrupt companies, with ROC-AUC of 0.894.
- The precision-recall trade-off can be adjusted with a threshold to meet business needs.
- The cross-validation score (0.996) is too optimistic because oversampling was done before cross-validation (data leakage). Use an `imblearn` pipeline instead.

## Tools and Libraries

Python, pandas, NumPy, scikit-learn, imbalanced-learn, matplotlib, ipywidgets.

## Future Improvements

Fix the data leakage in cross-validation, try SMOTE or XGBoost/LightGBM, choose a threshold using the Precision-Recall curve, and add input validation to `predictor.py`.
