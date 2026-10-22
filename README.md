# Diabetes Disease Progression Prediction

**Regression Study · Machine Learning Benchmark**

Benchmark analysis evaluating baseline clinical predictors for one-year disease progression on the standard scikit-learn diabetes dataset.

---

## Overview

* **Objective:** Determine whether baseline physiological measurements improve regression predictions over a naive average baseline.
* **Dataset:** 442 patient records, 10 numerical features.
* **Selected Model:** **Ridge Regression** (Holdout $RMSE = 53.63$, $R^2 = 0.4572$).
* **Primary Driver:** Body Mass Index (`bmi`) yielded the largest permutation score drop.

---

## Key Results

Ridge Regression outperformed both Random Forest and the Dummy Baseline across cross-validation and holdout splits.

### Training Cross-Validation (K-Fold)

| Model | RMSE | Fold SD |
| :--- | :--- | :--- |
| **Ridge Regression** | **55.73** | 1.70 |
| Random Forest | 57.09 | 2.49 |
| Dummy Baseline | 78.20 | 3.03 |

### Final Holdout Evaluation (89 Rows)

| Model | RMSE | MAE | $R^2$ |
| :--- | :--- | :--- | :--- |
| **Ridge Regression** | **53.63** | **42.86** | **0.4572** |
| Dummy Baseline | 73.22 | 64.01 | -0.0120 |

> **95% Bootstrap CI for Ridge RMSE:** 45.51 – 61.87 (200 resamples).

---

## Data Summary

* **Source:** [scikit-learn Diabetes Toy Dataset](https://scikit-learn.org/stable/datasets/toy_dataset.html#diabetes-dataset)
* **Quality Check:** 442 total samples, zero missing cells, zero exact duplicates.
* **Correlations:** Strongest predictor correlation observed between `s1` and `s2` ($\vert{}r\vert{} = 0.891$).

---

## Visualizations

| Target & Distributions | Model Comparison |
| :---: | :---: |
| ![Distributions](figures/distributions.png) | ![Model Comparison](figures/model-comparison.png) |

| Feature Importance | Error Analysis |
| :---: | :---: |
| ![Feature Importance](figures/feature-importance.png) | ![Error Analysis](figures/error-analysis.png) |

---

## Repository Structure

```text
├── figures/                 # Diagnostic and performance plots
├── analysis.ipynb           # Interactive exploration and pipeline walkthrough
├── audit.json               # Data integrity and run metadata
├── data_dictionary.csv      # Feature descriptions and types
├── descriptive_statistics.csv # Summary stats per feature
├── error_analysis.csv       # Residuals and largest absolute errors
├── feature_importance.csv   # Permutation importance rankings
├── metrics.json             # Final CV and holdout performance metrics
└── README.md
