# Data Science Projects

This repository contains eight data science projects. They cover data analysis, machine learning, time series, clustering, and A/B testing. Each project has its own README with more details.

## Projects

1. **Mexico Real Estate: Property Price Analysis**  
   Cleans and explores house and apartment prices in Mexico. The notebooks cover visualization, correlation analysis, and price per square meter.  
   [Project details](./1.Foundational_data_science/README.md)

2. **Buenos Aires Real Estate: Apartment Price Prediction**  
   Uses linear regression, Ridge, and Lasso models to predict apartment prices. The best reported test RMSE is about 33,069 USD.  
   [Project details](./2.Supervised_machine_learning/README_buenos_aires.md)

3. **Nairobi Air Quality: PM2.5 Forecasting**  
   Uses time series models to forecast PM2.5 levels, including linear regression, autoregressive models, and ARMA. The README explains why the reported model scores are not directly comparable.  
   [Project details](./3.Time_Series_Modeling/README_nairobi.md)

4. **Predicting Building Damage from the 2015 Nepal Earthquake**  
   Uses logistic regression and decision trees to predict severe building damage. The projects explore data from Gorkha and Kavrepalanchok.  
   [Project details](./4.%20Classification%20Models/README.md)

5. **Company Bankruptcy Prediction**  
   Tests models for imbalanced bankruptcy data from Poland and Taiwan. The tuned Gradient Boosting model has a reported ROC-AUC of 0.894 on the Poland test set.  
   [Project details](./5.RandomForest_GradienBoosting/README%20(1).md)

6. **Credit-Fearful Household Analysis**  
   Uses K-Means clustering to explore groups of U.S. households that are afraid to apply for credit. The project also includes an interactive Dash dashboard.  
   [Project details](./6.Unsupervised_Learning/README.md)

7. **A/B Test of Reminder Emails**  
   Studies whether reminder emails help applicants complete a data science admissions quiz. The simulated results do not show a statistically significant improvement.  
   [Project details](./7.Experimentation_and_Decision_Making/README_AB_Test.md)

8. **BSE Stock Volatility Analysis and Forecasting**  
   Analyzes Ambuja Cement and Suzlon stock data and forecasts Ambuja Cement volatility with a GARCH(1,1) model.  
   [Project details](./8.Financial%20Time%20Series%20Data/README.md)

## Tools and Libraries

The projects use Python and libraries such as pandas, NumPy, scikit-learn, statsmodels, Matplotlib, Seaborn, Plotly, and Dash. Some projects also use MongoDB, SQLite, or DuckDB.

## How to Use

Open a project's README for its requirements and setup instructions. Run that project's notebooks in the order described there. Some projects require local data files or a database such as MongoDB.
