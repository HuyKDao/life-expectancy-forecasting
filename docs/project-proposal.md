# Project Proposal: Predicting Future Life Expectancy

**Course:** CIS 5450 Big Data Analytics, University of Pennsylvania, Spring 2026

## Team and responsibilities
| Member | Responsibilities |
|---|---|
| **Huy Dao** | Data cleaning and exploration |
| Aaron Watkins | Version control, feature engineering and modeling |
| Jeanne du Fay de Lavallaz | Project idea, data ingestion, interpreting results |
| Harrison Gill | Project proposal, feature engineering and modeling |

## Data source
HealthStats: Health Nutrition and Population Statistics (World Bank, via Kaggle): 345 indicators for 263 countries, collected yearly from 1960 to 2016. 54,959 rows remain after cleaning and dropping nulls.

## Plan
1. **Clean the data:** handle missing values, most likely with some form of imputation.
2. **Explore the data:** correlations and trends between features and life expectancy, time-series plots by country, and heat maps.
3. **Reduce dimensions:** PCA, plus feature selection with LASSO and tree-based importance, to cut complexity and multicollinearity.
4. **Model:** start from a linear regression baseline, then try Ridge and LASSO, then Random Forest and Gradient Boosting.
5. **Evaluate:** RMSE, MAE and R² on a held-out set. To avoid temporal leakage, train on earlier years and test on later ones.
6. **Interpret:** use feature importance and coefficients to find which indicators best predict future life expectancy.

## Objective
Predict a nation's life expectancy **ten years ahead**, so that health policy, social security and resource planning can rely on forward-looking estimates.

## Expected challenge
Missing data. Data may be sparse for some countries or time periods, which can bias models if the gaps are related to other characteristics of those countries.
