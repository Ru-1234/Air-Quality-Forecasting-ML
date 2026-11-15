# Air Quality Forecasting Using Machine Learning: Phase 1 (Base Paper Implementation)

CS3002 – Machine Learning course project, FAST-NUCES.
Group 56: Maham Anjum (24I-0053), Romaisa (24I-0061), Section AI-5C.

This project reproduces the paper:

> Y. Özüpak, F. Alpsalaz and E. Aslan, "Air Quality Forecasting Using Machine Learning: Comparative Analysis and Ensemble Strategies for Enhanced Prediction," *Water, Air, & Soil Pollution*, vol. 236, art. 464, 2025. https://doi.org/10.1007/s11270-025-08122-8

The notebook predicts hourly **benzene (C6H6)** concentration with the paper's 10 regression models. It compares default settings, the paper's hyperparameters and Bayesian optimisation, adds a stacking ensemble, and explains the models with SHAP and partial dependence plots. Every result is compared with the paper's Table 4.

## Folder contents

| File | What it is |
|---|---|
| `24i-0053_24i-0061_ML_Phase1.ipynb` | The complete implementation (one notebook, run top to bottom) |
| `24i-0053_24i-0061_ML_Phase1_Report.pdf` | Phase 1 report |
| `README.md` | This file: setup and run instructions |
| `requirements.txt` | Python libraries needed (pinned versions) |
| `results_all_models.csv` | All test results; created when the notebook runs |

## Requirements

- Python 3.10 or newer (we used Python 3.13.9)
- The libraries in `requirements.txt`
- About 15–30 minutes of CPU time. No GPU is needed.

## Installation

```bash
python -m venv venv
venv\Scripts\activate          # Windows
# source venv/bin/activate     # macOS / Linux
pip install -r requirements.txt
```

## Dataset

The dataset is not included in this folder. Download it from either source:

- Kaggle: https://www.kaggle.com/datasets/fedesoriano/air-quality-data-set
- UCI Machine Learning Repository: https://archive.ics.uci.edu/dataset/360/air+quality

Save the CSV file **in the same folder as the notebook** with the name **`AirQuality.csv`**. The file uses `;` as the separator and `,` as the decimal mark. Missing values are coded as `-200`; the notebook handles all of this.

## How to run

1. Start Jupyter: `jupyter notebook`
2. Open `24i-0053_24i-0061_ML_Phase1.ipynb`.
3. Choose **Kernel → Restart & Run All**.

The notebook runs in this order:

| Section | What it does |
|---|---|
| 1–3 | Setup, loading the data, exploratory analysis (paper Figs. 2–5, Table 2) |
| 4 | Preprocessing: drop NMHC(GT) and missing rows, IQR outlier removal, 80/20 time split, standardisation |
| 5–7 | 10 models with default settings and with the paper's Table 3 hyperparameters |
| 8 | Bayesian optimisation (Optuna TPE, 20 trials, 5-fold CV, paper's search ranges) |
| 9 | Comparison with the paper's Table 4 |
| 10 | Reproducing the paper's headline SVR result |
| 11 | Stacking ensembles |
| 12 | Feature importance: Pearson, SHAP, PDP |
| 13–14 | Final results table and conclusion |

## Reproducibility

- One random seed (`SEED = 42`) is used for every model, split and optimiser, so a rerun gives the same numbers.
- Settings such as `N_TRIALS` and `CV_FOLDS` are at the top of the notebook, in Section 1.
- The first cell prints the exact library versions used; `requirements.txt` pins the same versions.

## Main results

| Result | Ours | Paper |
|---|---|---|
| Best tuned single model | Random Forest, R² 1.0000 | SVR, R² 0.9994 |
| SVR with the paper's settings, PT08.S2(NMHC) sensor only | R² 0.9999 | 0.9994 |
| Stacking vs best single model | 1.0% worse | claimed 15–20% better |

Benzene in this dataset is almost an exact function of the PT08.S2(NMHC) sensor (cubic fit R² = 0.99997), which explains the very high scores. See the report for the full comparison and discussion.
