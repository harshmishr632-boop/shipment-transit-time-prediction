# Shipment Transit Time Prediction & Delay Analysis (Alteryx)

An end-to-end logistics analytics project that prepares shipment data, engineers operational features, trains predictive regression models in Alteryx Designer, scores shipment transit times, and evaluates prediction error.

> **Project status:** Portfolio-ready prototype. The repository includes the prepared dataset, Alteryx workflow, scoring outputs, documentation, and workflow screenshots. The workflow requires Alteryx Designer and is not currently runnable without updating its machine-specific file paths.

## What this project does

The project predicts `Transit_Days` for shipment records using route, carrier, distance, weight, cost, and engineered logistics features. It also compares model predictions with observed transit times using regression metrics such as MAE and RMSE.

This is a **transit-time regression** project. It is not a delay-classification model unless a promised-delivery threshold and an explicit on-time/late target are defined.

## Repository contents

```text
.
├── README.md
└── shipment-transit-prediction-alteryx/
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

### Key files

| File | Purpose |
|---|---|
| `data/logistics_features.csv` | Prepared feature dataset with 1,965 shipment records and 13 columns |
| `workflow/Shipment_Days_Scoring.yxmd` | Alteryx Designer workflow for sampling, model training, comparison, scoring, and export |
| `outputs/shipment_transit_predictions.csv` | Scored shipment records with predictions and error columns |
| `outputs/shipment_prediction_results.xlsx` | Workbook containing prediction results, summary metrics, and a data dictionary |
| `documentation/Alteryx_Workflow_Explanation.pdf` | Visual explanation of the Alteryx workflow |
| `documentation/METRICS_AND_DATA_NOTES.md` | Definitions and interpretation notes for evaluation metrics |

## Data and features

The prepared dataset includes the following fields:

- **Shipment attributes:** `Origin_Warehouse`, `Destination`, `Carrier`
- **Operational inputs:** `Weight_kg`, `Distance_miles`, `Cost_Numeric`
- **Data-quality flag:** `Cost_Was_Null`
- **Target:** `Transit_Days`
- **Shipment status:** `Status`
- **Engineered features:** `Cost_Per_Mile`, `Weight_Per_Mile`, `Distance_Band`, `Is_Heavy`

The dataset is a prepared feature artifact rather than a guaranteed copy of the untouched raw source data. It contains examples of missing-cost handling, categorical route/carrier fields, continuous operational variables, and derived ratios.

## Workflow

The Alteryx workflow follows this high-level sequence:

1. Read the prepared logistics feature CSV.
2. Select fields and convert numeric columns to appropriate types.
3. Split the data into estimation and validation samples using a documented 70:30 split and random seed `1`.
4. Train a random-forest model and a boosted regression model.
5. Compare model performance on the validation sample.
6. Score validation records with the selected model.
7. Export scored results to Excel.

The current workflow starts from an already prepared feature dataset. The repository README describes the upstream data-preparation work for context, but the `.yxmd` file does not contain a complete raw-data cleaning pipeline.

## Important modeling note: possible target leakage

`Status` is present in the current model predictor configuration. Values such as `Delivered`, `Delayed`, `Lost`, and `Returned` may only be known after shipment progress or completion, so they may not be available at dispatch time.

For a genuine dispatch-time prediction use case, remove `Status` from the predictors and retrain the models. Keep `Status` only for post-shipment analysis or if it is demonstrably available at prediction time.

## Evaluation fields and metrics

The scoring output uses:

- **Observed value:** `Transit_Days`
- **Predicted value:** `Score`
- **Prediction error:** `Transit_Days - Score`
- **Absolute error:** `abs(Transit_Days - Score)`
- **MAE:** mean absolute error in days; lower is better
- **RMSE:** root mean squared error in days; penalizes larger errors more heavily
- **R-squared:** descriptive fit statistic; interpret only after confirming the evaluation split and model context

The supplied scoring workbook contains 589 rows. The feature CSV contains 1,965 rows. These are different artifacts and should not be treated as identical datasets or as proof that every CSV record appears in the workbook.

## How to run the workflow

### Requirements

- Alteryx Designer compatible with the workflow version
- Alteryx Predictive Tools, including the Forest Model, Boosted Model, Model Comparison, and scoring macros
- A local copy of this repository

### Steps

1. Open `shipment-transit-prediction-alteryx/workflow/Shipment_Days_Scoring.yxmd` in Alteryx Designer.
2. Update the input path to:

   ```text
   shipment-transit-prediction-alteryx/data/logistics_features.csv
   ```

3. Update the output path to a local file under:

   ```text
   shipment-transit-prediction-alteryx/outputs/
   ```

4. Review the model predictors, especially the use of `Status`.
5. Run the workflow and inspect the validation metrics and scored output.

> The workflow currently contains absolute Windows paths from the original authoring environment. It will require path updates on another machine.

## Output interpretation

`shipment_transit_predictions.csv` includes the original shipment fields plus:

- `Score` — predicted transit time
- `Prediction_Error_Days` — signed error, observed minus predicted
- `Absolute_Error_Days` — magnitude of the prediction error

A positive signed error means the model underpredicted the observed transit time. A negative signed error means it overpredicted the observed transit time.

## Reproducibility and limitations

- Alteryx Designer is not included in this repository.
- The workflow depends on Alteryx Predictive Tools installed outside the repository.
- The current `.yxmd` contains machine-specific Windows paths and should be made portable before sharing as a reusable workflow.
- The checked-in CSV is a prepared feature dataset, not necessarily the original raw data.
- The current outputs should be treated as supplied scoring artifacts unless regenerated from the workflow.
- No automated test suite or CI validation is included yet.
- Data-quality checks for outliers, duplicates, zero weights, and unusual cost-per-mile values should be added before operational use.

## Skills demonstrated

Alteryx Designer · Data Cleaning · Feature Engineering · Predictive Analytics · Regression · Random Forest · Boosting · Model Scoring · MAE/RMSE Evaluation · Logistics Analytics · Excel Reporting

## Suggested resume title

**Shipment Transit Time Prediction & Delay Analysis | Alteryx Designer**
