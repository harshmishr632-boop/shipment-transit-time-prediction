# Shipment Transit Time Prediction & Delay Analysis (Alteryx)

## Project overview
An end-to-end predictive analytics project using Alteryx Designer to prepare logistics shipment data, engineer operational features, score shipment transit times, and compare model predictions with observed transit duration. The business objective is to support better delivery estimates, shipment planning, and carrier performance review.

## Repository contents
- `data/logistics_features.csv` — prepared feature dataset supplied with the project (1,965 rows; 13 columns).
- `workflow/Shipment_Days_Scoring.yxmd` — Alteryx Designer workflow file.
- `outputs/shipment_transit_predictions.csv` — prediction/scoring output derived from the provided Booster Excel output.
- `outputs/shipment_prediction_results.xlsx` — Excel workbook with predictions, error columns, summary metrics, and a data dictionary.
- `documentation/Alteryx_Workflow_Explanation.pdf` — step-by-step workflow documentation.
- `images/` — screenshots of the Alteryx workflow canvas.

## Workflow summary
1. Load and profile shipment data.
2. Clean shipping-cost values and handle incomplete records.
3. Convert fields to suitable data types and filter transit-day values to the documented range.
4. Engineer `Cost_Per_Mile`, `Weight_Per_Mile`, `Distance_Band`, and `Is_Heavy`.
5. Split data into training and validation samples (documented as 70:30).
6. Train/score predictive models and compare error metrics.
7. Export scored shipment records for further analysis.

## Results file
The supplied `booster 000001.xlsx` scoring output contained 589 records. In this repository, the `Score` field is treated as the predicted transit time and `Transit_Days` as the observed transit time. The results workbook calculates MAE, RMSE, and a descriptive R-squared value from those rows. These are **metrics for the provided scoring workbook only**; do not describe them as held-out test metrics unless you verify that those 589 rows are an independent test/validation set.

## Open the workflow
1. Install/open Alteryx Designer with the required predictive tools available.
2. Open `workflow/Shipment_Days_Scoring.yxmd`.
3. The workflow originally references Windows paths from the machine where it was created. Update the Input Data and Output Data paths in Alteryx to the local copies in this repository before running.
4. Confirm the model/tool configuration and run the workflow. Output paths may be set to `outputs/`.

## Important notes
- The workflow file is provided as an Alteryx artifact; this repository does not include Alteryx itself or guarantee execution in every environment.
- The current CSV is a prepared feature dataset, not necessarily the untouched original raw dataset.
- `Status` may reflect an outcome of the shipment. Avoid using it as a predictor if it leaks information unavailable at dispatch time.
- Predicting transit days is a regression task. Calling it a delay classifier would require a defined promised-delivery threshold and a separate on-time/late label.

## Skills demonstrated
Alteryx Designer · Data Cleaning · Feature Engineering · Predictive Analytics · Regression · Model Scoring · MAE/RMSE Evaluation · Logistics Analytics · Excel

## Suggested resume title
**Shipment Transit Time Prediction & Delay Analysis | Alteryx Designer**
