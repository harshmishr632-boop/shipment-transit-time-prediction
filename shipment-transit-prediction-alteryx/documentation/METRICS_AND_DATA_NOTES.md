# Project notes and metric definitions

- **Observed value:** `Transit_Days`
- **Predicted value:** `Score`
- **Prediction error:** `Transit_Days - Score`
- **Absolute error:** `abs(Transit_Days - Score)`
- **MAE:** average absolute error in days; lower is better.
- **RMSE:** square root of mean squared error in days; penalizes larger misses more heavily.
- **R-squared:** descriptive fit statistic calculated on the supplied scoring rows. Interpret cautiously unless the dataset's evaluation role is confirmed.

The workbook contains 589 rows. The feature CSV contains 1,965 rows; these are different artifacts and should not be assumed to be the same split.
