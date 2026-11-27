[README.md](https://github.com/user-attachments/files/35497193/README.md)
# athlete-readiness-loao# Athlete readiness, fatigue and handgrip strength: leave-one-athlete-out validation

Code for the paper **"Predicting Athlete Readiness, Fatigue, and Handgrip Strength from Daily Monitoring Data with Leave-One-Athlete-Out Validation"** (IEEE BigData 2026 Workshop on AI-Sports).

Authors: Vedaant Puri and Atul Dubey.

## What this project does
It tests whether a daily questionnaire and one wrist-worn sleep sensor can predict three targets:
- **Readiness** (binary: score of 7 or above is high)
- **Fatigue** (binary: score of 3 or above is high)
- **Handgrip strength** (regression, in kg)

Models: Logistic Regression, K-Nearest Neighbors and Random Forest (Ridge instead of Logistic Regression for handgrip), over six feature sets, with leave-one-athlete-out cross-validation and an athlete-level train/test split.

## Data
The data is **not** included in this repository. Download the public REST dataset (Boeker et al., Scientific Data 13, 546, 2026) from Zenodo, DOI [10.5281/zenodo.16937033](https://doi.org/10.5281/zenodo.16937033), and follow its license.

## Repository contents
- `Codes`: notebooks for data processing, athlete-level splitting, model training and testing for readiness, fatigue and handgrip.
- `Models`: the saved best models (`.joblib`). These are the **original runs, whose Random Forests were trained without a fixed seed**, and they produce the figures and main tables in the paper.
- `Results`: result tables and figures from those runs.
- `scripts/` (from `analysis_scripts.zip`): the analysis added for the final version of the paper.
  - `baselines.py`: majority-class, previous-day and mean-handgrip baselines.
  - `selected_eval.py`: the selected models rerun with seeds 0-29, athlete-level bootstrap confidence intervals, and pooled leave-one-athlete-out evaluation.
  - `fatigue_cw.py`: class weighting test for fatigue and readiness (needs `selected_eval.py` in the same folder).

## Running the scripts
1. Put the derived tables (`readiness_train.csv`, `readiness_test.csv`, `fatigue_train.csv`, `fatigue_test.csv`, `handgrip_train.csv`, `handgrip_test.csv`) in a `Derived/` folder, or set the `DATA_DIR` environment variable to the folder path (with a trailing slash).
2. Run `python baselines.py`, `python selected_eval.py` and `python fatigue_cw.py`. Each writes a JSON file with its results.

Requires Python 3, pandas, numpy, scikit-learn and scipy.

## Reproducibility notes
- Split: athlete-level `GroupShuffleSplit`, test fraction 0.25, `random_state=42`.
- Selected models: readiness, Random Forest (max_depth 10, 200 trees); fatigue, Random Forest (max_depth 10, 100 trees); handgrip, Random Forest (max_depth 1, 10 trees).
- The saved models are unseeded, so rerunning the original notebooks can give slightly different numbers. The seeded results are in the paper's variability table.

## Citation
If you use this code, please cite the paper above.
