# Regression Analysis & Model Comparison — Utility Revenue Forecasting

A single-variable linear regression study that compares two candidate predictors of a
utility company's monthly revenue, and picks the better one using out-of-sample forecast
accuracy rather than in-sample fit alone.

Built in Python with `pandas`, `statsmodels`, and `matplotlib`.

---

## Overview

The task: forecast monthly **revenue** for *Hotazel Steam* (a steam/utility company case).
Two economic drivers were tested as predictors, one model each:

- **Model 1** — revenue as a function of **production**
- **Model 2** — revenue as a function of **coolDD** (cooling degree days)

Each model is trained on historical data, then validated on a held-out later period. The
"winner" is chosen on **MAPE (mean absolute percentage error)** against the actual
revenue in the test window — i.e. how well it predicts data it never saw during training.

## Dataset

- Source file: `AICPA_regressionAnalysisData.csv` (an AICPA regression-analysis teaching dataset).
- Columns: `type`, `date`, `revenue`, `production`, `coolDD`, `heatDD`
- A `type` column splits the data into `dt4training` and `dt4testing`.
- Training window: **2011–2013**. Test window: **2014**.

## Method

1. Load the CSV, convert `date` to a datetime type, and plot revenue over time.
2. Check correlations between `revenue`, `production`, and `coolDD`.
3. Split into train/test using the `type` column.
4. Fit each OLS model on the training set with `statsmodels` (`sm.add_constant` for the intercept).
5. Predict revenue on the test set and compute absolute percentage error per row.
6. Compare the two models on mean MAPE and adjusted R².

## Results

| Model | Predictor | Adjusted R² (train) | Out-of-sample MAPE (test) |
|-------|-----------|---------------------|----------------------------|
| Model 1 | production | 0.379 | ~25.4% |
| Model 2 | coolDD | −0.012 | ~29.6% |

Supporting correlation (in-sample): production–revenue ≈ **0.63**; coolDD–revenue ≈ **−0.17**.

**Takeaway:** Model 1 (production) forecasts revenue more accurately out-of-sample.
Model 2's negative adjusted R² indicates coolDD explains essentially none of the variance
in revenue — arguably worse than using no predictor at all. The recommendation is to use
production as an interim predictor while noting that a ~25% MAPE still leaves meaningful
room for improvement, motivating a richer (e.g. multi-variable) model.

## Tech stack

- Python
- pandas
- statsmodels (OLS)
- matplotlib
- Jupyter / Google Colab

## Repository contents

- `RegressionModel_-_Utility_Co.ipynb` — the full analysis notebook
- `AICPA_regressionAnalysisData.csv` — input data (place alongside the notebook to run)

## How to run

```bash
pip install pandas numpy matplotlib statsmodels
```

Open the notebook in Jupyter or Google Colab, make sure
`AICPA_regressionAnalysisData.csv` is in the same directory (or upload it in Colab), and
run the cells top to bottom.

## Possible next steps

- Move from single-variable to multiple regression (e.g. production + degree-day terms).
- Add seasonal dummies or interaction terms to capture the periodic revenue swings.
- Report a train/validation error gap explicitly to check for over/underfitting.

## Author

**Anthony Quach**

A couple of small edits from the earlier version, so the pasted text reads clean on GitHub: I removed the parenthetical uncertainty asides (the "I don't have a confirmed URL" and "inferred from..." notes) since those were meant for you, not for a public README. That means two things now stated as fact are worth you confirming before you commit:

The 2011–2013 / 2014 train-test years — I inferred these from the dates in
