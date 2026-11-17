# Predicting Building Damage from the 2015 Nepal Earthquake

This data science project predicts whether a building had **severe damage** (damage Grade 4-5) in the 2015 Nepal earthquake. It uses building and household survey data from several districts. Target: `severe_damage` (1 = severe, 0 = not severe).

## Dataset
- Source: `nepal.sqlite` and CSV files in the `data/` folder.
- Tables: `building_structure`, `building_damage`, `household_demographics`, `id_map`.
- The data includes 234,835 buildings and 249,932 households. The main focus is **Gorkha** (district_id 4, about 70,000-76,000 buildings) and **Kavrepalanchok** (district_id 3, 82,684 buildings).
- The `*_post_eq`, `damage_grade`, and `building_id` columns are removed to prevent data leakage.

## Project Workflow

| No. | Notebook | Content | Main Results |
|---|---|---|---|
| 1 | Data Exploration and Wrangling | SQLite vs. DuckDB queries, `wrangle_nepal_data()` function (in `duckdb_wrangle.py`) | DuckDB is about 2.4x faster; Gorkha: 70,836 rows, 64.26% severely damaged |
| 2 | Logistic Regression (building structure) | One-hot encoding, odds ratios, ROC/AUC | Test accuracy 70.1%; AUC 0.633; recall 96%, precision 69% |
| 3 | Decision Tree and Tuning | Train/validation/test split, `max_depth` tuning, feature importance | Best `max_depth` is 7; test accuracy 71.2%, slightly higher than logistic regression |
| 4 | Logistic Regression with Demographics and Location | Combine demographic data, group rare caste values, analyze by municipality | Test accuracy 72.4%; damage rates by municipality range from 43% to 93% |
| 5 | Kavrepalanchok Task | Decision Tree (`max_depth=10`) for district 3 | Test accuracy 66.6% (baseline about 54.7%) |

## Main Findings
- **Foundation type and building materials** are the most important factors. Reinforced concrete (RC/RCC) provides protection, while stone with mud mortar and wood/bamboo roofs increase risk by 2-3.5 times.
- **Location** has a strong effect, with large differences between municipalities. The link to caste makeup should not be treated as cause and effect because it is based on only 11 municipalities and may be affected by location.
- Simple models improve the results by only about 7-9 points over the baseline. Building structure features alone are not enough.

## Tools and Libraries
Python, pandas, NumPy, Matplotlib/Seaborn, DuckDB, SQLite, scikit-learn, category_encoders.

## Suggestions for Improvement
- Scale numeric features (to address `ConvergenceWarning`) and remove the constant `district_id` column.
- Report precision, recall, and AUC, not only accuracy.
- Try Random Forest or Gradient Boosting, add location and number-of-floors features, and tune other hyperparameters.

## How to Run
1. Put `nepal.sqlite` and the `data/` folder in the project directory.
2. Install the packages: `pip install pandas numpy matplotlib seaborn duckdb scikit-learn category_encoders`.
3. Run the notebooks in the order shown in the table above.
