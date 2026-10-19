# Forecasting Life Expectancy 10 Years Ahead

Can a country's health, economic and demographic indicators today predict its life expectancy ten years from now? This group project for **CIS 5450 Big Data Analytics** (University of Pennsylvania, Spring 2026) builds and compares eight machine learning models to answer that question.

**Team:** Huy Dao, Aaron Watkins, Jeanne du Fay de Lavallaz, Harrison Gill

## My contribution
I owned **data cleaning and exploration** ([proposal](docs/project-proposal.md), [update log](docs/update-log.md)):
- Wrote the reusable cleaning module [`src/data_utils.py`](src/data_utils.py) (`clean_health_data()`): it removes stray columns, coerces year columns to numeric, de-duplicates and drops rows that are empty for every year.
- Analyzed missing values per row, chose the drop threshold from their distribution (50+ missing years means drop, fewer means impute), and implemented **linear interpolation across years**, including the edge cases at the start and end of each series.
- Checked that cleaning didn't distort trends by re-plotting key indicators before and after imputation.
- Restructured the shared notebook (sections, data loading order, smaller EDA cells, plot fixes) so the team could build models on top of it.

## Why it matters
Health interventions such as better sanitation, education or disease control take years to show results. A model that forecasts life expectancy a decade ahead, and shows which indicators drive it, can help policymakers decide where long-term investment pays off most.

## Data
[HealthStats: Health Nutrition and Population Statistics](https://www.kaggle.com/datasets/theworldbank/health-nutrition-and-population-statistics) (World Bank, via Kaggle): 345 indicators for 263 countries, yearly from 1960 to 2016.

## What I did
1. **Cleaning:** dropped rows with too few observed years (threshold chosen from the missing-value distribution) and filled remaining gaps with **linear interpolation across years**. I chose it over mean or forward fill because it keeps each country's trend intact.
2. **Reshaping:** converted the wide table (one column per year) to long format, then pivoted to a country-year modeling table with 240 indicator features.
3. **Target:** built a 10-year-ahead target by shifting each country's life expectancy forward 10 years.
4. **Temporal split:** trained on 1960–1995 and tested on 1996–2005, so the model never sees the future it predicts (8,676 training rows, 2,410 test rows).
5. **Modeling:** compared linear, regularized, dimension-reduced and tree-based models on RMSE, MAE and R².

## Results

| Model | Features | RMSE (years) | MAE | R² |
|---|---|---|---|---|
| **Random Forest** | 240 | **2.43** | **1.73** | **0.922** |
| LASSO regression | 26 | 2.60 | 1.75 | 0.911 |
| LASSO-selected + linear | 26 | 2.63 | 1.76 | 0.908 |
| Ridge top-20 + linear | 20 | 2.71 | 1.79 | 0.902 |
| Ridge regression | 240 | 2.75 | 1.80 | 0.900 |
| Gradient Boosting | 240 | 2.78 | 1.76 | 0.897 |
| PCA (95% var, 35 comps) + linear | 35 | 2.83 | 1.87 | 0.894 |
| Baseline linear regression | 240 | 3.33 | 2.19 | 0.853 |

- The Random Forest cut forecast error by **27% versus the baseline** (RMSE 3.33 → 2.43 years), which points to non-linear relationships between indicators.
- LASSO kept accuracy close to the best model while using **26 of 240 features**, which makes it the most interpretable choice.
- Beyond current life expectancy, the strongest predictors were **survival to age 65, adult and under-5 mortality, and maternal mortality risk**.

## Skills and tools
Python, pandas, NumPy, scikit-learn (LinearRegression, LassoCV, RidgeCV, PCA, RandomForestRegressor, GradientBoostingRegressor, Pipeline), Matplotlib, Seaborn, Google Colab. Methods used: time-series imputation, wide-to-long reshaping, feature selection, regularization, dimensionality reduction, temporal cross-validation, and regression metrics.

## Repository layout
```
notebooks/life_expectancy_forecasting_annotated.ipynb   # polished, annotated deliverable (start here)
notebooks/all_project_code.ipynb                        # full working notebook
src/data_utils.py                                       # cleaning module (my work)
data/data.csv.zip                                       # World Bank HNP dataset (unzip to data.csv)
docs/                                                   # proposal and update log
```

## Run it
1. Unzip `data/data.csv.zip`.
2. Open a notebook in Google Colab or Jupyter. Put `data.csv` and `src/data_utils.py` where the notebook expects them (the notebook uses `/content/drive/MyDrive/` by default), or edit the two path cells.
3. Run all cells.

Data: World Bank Health Nutrition and Population Statistics, used under CC BY 4.0.
