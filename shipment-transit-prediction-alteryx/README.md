# Shipment Transit Time Prediction & Delay Analysis (Alteryx)

This directory contains the data, Alteryx workflow, model outputs, documentation, and screenshots for a logistics analytics project that predicts shipment transit time and evaluates prediction error.

> **Project status:** Portfolio-ready prototype. The workflow requires Alteryx Designer and currently contains machine-specific Windows paths that must be updated before running on another computer.

## Project objective

Predict `Transit_Days` using shipment route, carrier, distance, weight, cost, and engineered logistics features. The workflow trains predictive regression models, compares their validation performance, scores shipment records, and exports the results for analysis.

This is a **transit-time regression** project. It is not a delay-classification model unless a promised-delivery threshold and an explicit on-time/late target are defined.

## Directory structure

```text
shipment-transit-prediction-alteryx/
├── data/
│   └── logistics_features.csv
├── documentation/
│   ├── Alteryx_Workflow_Explanation.pdf
│   └── METRICS_AND_DATA_NOTES.md
├── images/
│   ├── workflow_overview_1.png
│   └── workflow_overview_2.png
├── outputs/
│   ├── booster_scoring_output_original.xlsx
│   ├── shipment_prediction_results.xlsx
│   └── shipment_transit_predictions.csv
├── workflow/
│   └── Shipment_Days_Scoring.yxmd
└── README.md
```

## Dataset and features

`data/logistics_features.csv` is a prepared feature dataset containing 1,965 shipment records and 13 columns.

### Included fields

- **Shipment attributes:** `Origin_Warehouse`, `Destination`, `Carrier`
- **Operational inputs:** `Weight_kg`, `Distance_miles`, `Cost_Numeric`
- **Data-quality flag:** `Cost_Was_Null`
- **Target:** `Transit_Days`
- **Shipment status:** `Status`
- **Engineered features:** `Cost_Per_Mile`, `Weight_Per_Mile`, `Distance_Band`, `Is_Heavy`

The CSV is a prepared feature artifact rather than a guaranteed copy of the untouched raw source data.

## Workflow steps

`workflow/Shipment_Days_Scoring.yxmd` performs the following sequence:

1. Reads the prepared logistics feature CSV.
2. Selects fields and converts numeric columns to appropriate types.
3. Splits the data into estimation and validation samples using a documented 70:30 split and random seed `1`.
4. Trains a random-forest regression model and a boosted regression model.
5. Compares model performance on validation data.
6. Scores validation records with the selected model.
7. Exports scored results to Excel.

The current workflow starts from an already prepared feature dataset. The repository documentation describes the upstream cleaning and feature-engineering work for context, but the `.yxmd` does not contain a complete raw-data preparation pipeline.

## Important modeling note: possible target leakage

The current model configuration includes `Status` as a predictor. Values such as `Delivered`, `Delayed`, `Lost`, and `Returned` may only be known after shipment progress or completion, so they may not be available when transit time is predicted at dispatch.

For a genuine dispatch-time prediction use case, remove `Status` from the predictors and retrain the models. Keep it only for post-shipment analysis or when it is demonstrably available at prediction time.

## Outputs

### `outputs/shipment_transit_predictions.csv`

Scored shipment records containing the input fields plus:

- `Score` — predicted transit time
- `Prediction_Error_Days` — signed error, calculated as `Transit_Days - Score`
- `Absolute_Error_Days` — absolute prediction error

A positive signed error means the model underpredicted the observed transit time. A negative signed error means it overpredicted the observed transit time.

### `outputs/shipment_prediction_results.xlsx`

Workbook containing prediction results, error fields, summary metrics, and a data dictionary.

### `outputs/booster_scoring_output_original.xlsx`

Original supplied booster scoring artifact used as the source for the checked-in prediction output.

The supplied scoring workbook contains 589 rows, while the feature CSV contains 1,965 rows. These are different artifacts and should not be treated as identical datasets or as proof that every CSV record appears in the workbook.

## Evaluation metrics

- **Observed value:** `Transit_Days`
- **Predicted value:** `Score`
- **Prediction error:** `Transit_Days - Score`
- **Absolute error:** `abs(Transit_Days - Score)`
- **MAE:** mean absolute error in days; lower is better
- **RMSE:** root mean squared error in days; penalizes larger errors more heavily
- **R-squared:** descriptive fit statistic; interpret only after confirming the evaluation split and model context

See `documentation/METRICS_AND_DATA_NOTES.md` for the metric definitions and data notes.

## How to run

### Requirements

- Alteryx Designer compatible with the workflow version
- Alteryx Predictive Tools, including the Forest Model, Boosted Model, Model Comparison, and scoring macros
- A local clone of this repository

### Steps

1. Open `workflow/Shipment_Days_Scoring.yxmd` in Alteryx Designer.
2. Update the input path to:

   ```text
   data/logistics_features.csv
   ```

3. Update the output path to a local file under:

   ```text
   outputs/
   ```

4. Review the model predictors, especially the use of `Status`.
5. Run the workflow and inspect the validation metrics and scored output.

> The workflow currently contains absolute Windows paths from the original authoring environment. It must be updated before it can run on another machine.

## Reproducibility and limitations

- Alteryx Designer is not included in this repository.
- The workflow depends on Alteryx Predictive Tools installed outside the repository.
- The `.yxmd` contains machine-specific Windows paths and should be made portable before reuse.
- The checked-in CSV is prepared data, not necessarily the original raw dataset.
- The current outputs should be treated as supplied scoring artifacts unless regenerated from the workflow.
- No automated test suite or CI validation is included yet.
- Data-quality checks for outliers, duplicates, zero weights, and unusual cost-per-mile values should be added before operational use.

## Documentation and screenshots

- `documentation/Alteryx_Workflow_Explanation.pdf` — step-by-step workflow explanation
- `documentation/METRICS_AND_DATA_NOTES.md` — metric definitions and data notes
- `images/workflow_overview_1.png` — workflow overview screenshot
- `images/workflow_overview_2.png` — additional workflow screenshot

## Skills demonstrated

Alteryx Designer · Data Cleaning · Feature Engineering · Predictive Analytics · Regression · Random Forest · Boosting · Model Scoring · MAE/RMSE Evaluation · Logistics Analytics · Excel Reporting

## Suggested resume title

**Shipment Transit Time Prediction & Delay Analysis | Alteryx Designer**
