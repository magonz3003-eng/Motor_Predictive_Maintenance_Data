# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-notebook machine learning project for industrial motor failure prediction. The entire codebase lives in `Motor_Predictive_Maintenance_Data.ipynb`. There are no separate modules, scripts, or package structure.

## Running the Notebook

The notebook is designed to run in **Google Colab** (see the "Open in Colab" badge at the top). It can also run locally with Jupyter.

```bash
# Local execution
jupyter notebook Motor_Predictive_Maintenance_Data.ipynb

# Or with JupyterLab
jupyter lab Motor_Predictive_Maintenance_Data.ipynb
```

No `requirements.txt` exists. Required packages: `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `seaborn`.

## Dataset

The notebook loads `motor_predictive_maintenance_data.csv`, which is **not committed to this repository**. This file must be present in the working directory (or mounted in Colab) before the notebook will run. The dataset has these columns:

| Column | Description |
|---|---|
| `Timestamp` | Datetime of sensor reading (dropped before training) |
| `Motor_Temp_C` | Motor temperature in Celsius |
| `Vibration_Hz` | Vibration frequency |
| `Current_Amp` | Current draw in amperes |
| `Cycle_Count` | Number of operational cycles |
| `Failure_Label` | Binary target: `0` = healthy, `1` = failing |

## ML Pipeline Architecture

The notebook implements a linear pipeline with no abstraction layers:

1. **Data ingestion** — `pd.read_csv('motor_predictive_maintenance_data.csv')`
2. **Feature/target split** — drops `Timestamp` and `Failure_Label` from `X`; `y = Failure_Label`
3. **Train/test split** — 80/20, `random_state=42`
4. **Model training** — `RandomForestClassifier(n_estimators=100, random_state=42)`
5. **Evaluation** — `classification_report` (precision, recall, F1 per class)
6. **Feature importance plot** — horizontal bar chart of `model.feature_importances_`

All `random_state=42` for reproducibility. The current model achieves 1.00 F1-score on the test set, which likely reflects a clean synthetic dataset.

## Key Domain Facts

- **Vibration variance** is the strongest predictor of motor degradation in this dataset
- **Temperature spikes** precede failure events; torque instability correlates with bearing wear
- The system predicts binary failure classification; RUL (Remaining Useful Life) regression is listed as a future improvement but not yet implemented

## Planned Future Work (from README)

- LSTM-based RUL time-series prediction
- Isolation Forest anomaly detection layer
- Real-time Streamlit/Power BI dashboard
- Streaming sensor data pipeline
- REST API deployment
