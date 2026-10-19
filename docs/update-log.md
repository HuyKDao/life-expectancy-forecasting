# Notebook Update Log

## 03/13/26 (Huy Dao)

### Notebook structure
- Added a Google Drive mount cell so the notebook can read files from Drive.
- Reorganized Section I (Data Overview): moved data loading to right after the imports and removed duplicate imports.
- Split the large basic EDA cell into smaller cells, added section headers and labels, and removed redundant cells.
- Fixed the overlapping year labels on a plot's x-axis.

### Data preprocessing (Section II)
- Imported `data_utils.py` and used `clean_health_data()` for preprocessing.
- Checked the cleaned dataframe and documented each cleaning step.
- Analyzed missing values per row to decide between dropping and imputing.
- Built a histogram of missing values per row and set the threshold: drop rows with 50 or more missing values, and keep and impute the rest.
- Implemented linear interpolation across year columns, and explained why it beats mean or median imputation and forward or backward fill here.
- Handled edge cases where values are missing at the start or end of a series.
- Re-plotted trends on the cleaned data and compared them with the original plots to confirm the trends were preserved.

### data_utils.py
- Created `data_utils.py` for reusable cleaning functions.
- Implemented `clean_health_data()` to remove unnamed columns, standardize formatting, find the year columns, convert them to numeric, drop duplicates and drop rows where every year is missing.

### Suggested next steps
- Move the project folder to a shared drive so everyone uses the same file paths.
- Build heatmaps and feature visualizations to explore relationships between indicators.
- Select modeling features based on those visualizations.
