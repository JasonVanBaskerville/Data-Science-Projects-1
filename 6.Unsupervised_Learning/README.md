# Analysis of Credit-Fearful Households (Survey of Consumer Finances / SCF 2019)

This project analyzes U.S. households that are **afraid to apply for credit because they may be rejected** (`TURNFEAR == 1`). It uses data from the 2019 *Survey of Consumer Finances* (SCF), with 28,885 households and 351 variables. The project has four connected stages: data exploration, basic clustering, advanced clustering, and an interactive dashboard.

## Project Overview

| Stage | Focus | Main Results |
|---|---|---|
| 1. Exploratory Data Analysis (EDA) | Filter credit-fearful households, then analyze age, race, income, education, and correlations between assets, houses, and debt. | About 4,623 households (around 16%) are credit-fearful. More of them have low incomes (about 28.8% in the 0–20th percentile, compared with about 14.0% of other households). |
| 2. Basic K-Means | Group households using `DEBT` and `HOUSES`. | The final model uses k = 4, with a silhouette score of about 0.95. Most households are in one cluster, while the other small clusters contain extreme values. |
| 3. Advanced K-Means | Use households with net worth below $2 million (4,418 households). Select features with *trimmed variance*, scale them with `StandardScaler`, then visualize them with PCA. | The five selected features are `DEBT, NETWORTH, HOUSES, NFIN, ASSET`. The model uses k = 4 and finds groups ranging from households in the weakest financial condition to wealthy households with little debt. |
| 4. Interactive Dashboard (Dash) | Turn stage 3 into a web app. | Users can choose trimmed or regular variance and the number of clusters (2–12). The bar chart, inertia, silhouette score, and PCA scatter plot update automatically. |

## Tools and Libraries

Python, pandas, NumPy, SciPy (`trimmed_var`), scikit-learn (`KMeans`, `PCA`, `StandardScaler`, `silhouette_score`), Matplotlib, Seaborn, Plotly, and Dash.

## Quick Workflow

1. Load `SCFP2019.csv.gz` and filter for `TURNFEAR == 1` (and `NETWORTH < 2e6` for stages 3–4).
2. Select features with high variation using trimmed variance, then scale them.
3. Choose the number of clusters using inertia (the *elbow* method) and the silhouette score. Train the final model.
4. Describe each cluster using its average feature values, then visualize the clusters with 2D PCA.
5. Show the results in the Dash dashboard.

## Conclusion

- Fear of applying for credit is strongly linked to **low income**. Much of this group's debt is related to **home ownership** (`DEBT` and `HOUSES` have a very strong correlation).
- Credit-fearful households are **not all alike**. The groups include households with low or moderate assets and net worth, households with high debt, and wealthy households with little debt.
- Using *trimmed variance* and scaling helps prevent outliers and large-scale features from dominating the clusters.

## Notes and Limitations

- PCA in the dashboard and stage 3 uses **unscaled data**, while K-Means uses scaled data. For consistent results, PCA should also use the scaled data.
- In stages 3–4, `silhouette_score` is calculated using the original data, not the scaled data.
- In stage 2, the clusters are very uneven (one cluster has only a few households), and the features are not scaled.
- The results describe household groups; they do not show cause and effect.

## How to Run

```bash
pip install pandas numpy scipy scikit-learn matplotlib seaborn plotly dash
```

Put `SCFP2019.csv.gz` in the `data/` folder and run the notebooks in order. Then start the dashboard with `app.run(debug=False)` and open `http://127.0.0.1:8050`.
