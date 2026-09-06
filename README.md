# Predicting a disease progression benchmark

**2026-09-05 · REGRESSION**

## Research question

Do baseline measurements improve on predicting the average one-year progression score?

## Results

Ridge regression was selected using training cross-validation. Its holdout rmse was 53.6263, versus 73.2225 for the dummy baseline; it improved on that baseline on this holdout.

Training rmse was 53.7356. The training/holdout difference is descriptive; it is not an independent estimate of model uncertainty.

bmi had the largest mean permutation score drop (9.3891). This measures the fitted model's reliance on a feature, not a causal effect; correlated features can share importance.

The largest holdout absolute error was 149.9152 target units. Error examples are retained in error_analysis.csv.

The strongest absolute Pearson feature correlation in the training data was s1 / s2 (|r| = 0.891); this suggests checking redundancy, not concluding causality.

## Evaluation design

Random holdout; shuffled K-fold cross-validation on training rows. Training rows: 353; final holdout: 89. Seed: 42.

Missing-value imputation, scaling and category encoding are fitted inside each CV training fold. Model selection uses training CV only. The frozen winner and dummy baseline are then scored on the holdout. The baseline is allowed to win.

### Training cross-validation

| Model | rmse | Fold SD |
| --- | --- | --- |
| Ridge regression | 55.7303 | 1.6950 |
| Random forest | 57.0889 | 2.4869 |
| Dummy baseline | 78.2018 | 3.0275 |

Fold standard deviations are descriptive spread, not confidence intervals. The best CV score is selection-optimistic.

### Final holdout

| Model | Metric | Value |
| --- | --- | --- |
| Ridge regression | rmse | 53.6263 |
| Ridge regression | mae | 42.8568 |
| Ridge regression | r2 | 0.4572 |
| Dummy baseline | rmse | 73.2225 |
| Dummy baseline | mae | 64.0065 |
| Dummy baseline | r2 | -0.0120 |

Approximate 95% percentile bootstrap interval for rmse: **45.5076–61.8659** (200 resamples). Percentile bootstrap on fixed-model holdout predictions; stratified by class for classification. Conditional on this fitted model and split; excludes training/model-selection uncertainty.

## Data provenance

Source: [https://scikit-learn.org/stable/datasets/toy_dataset.html#diabetes-dataset](https://scikit-learn.org/stable/datasets/toy_dataset.html#diabetes-dataset)

License: scikit-learn bundled dataset; source attribution included. Attribution and source description are in SOURCE.md. The exact analyzed snapshot and its SHA-256 are retained.

## Data quality

| Check | Value |
| --- | --- |
| original rows | 442 |
| original features | 10 |
| missing cells | 0 |
| missing target rows removed | 0 |
| exact duplicates removed | 0 |
| usable rows | 442 |

Excluded from predictors: none specified. See data_dictionary.csv for column types, missingness and uniqueness.

## Visual evidence

![Missing values and target distribution](figures/data-quality.png)

Missing values and target distribution.

![Numeric feature distributions; up to six features by training/sample variance](figures/distributions.png)

Numeric feature distributions; up to six features by training/sample variance.

![Feature associations; correlation does not imply causation](figures/correlations.png)

Feature associations; correlation does not imply causation.

![Candidate model comparison; error bars are fold standard deviations, not confidence intervals](figures/model-comparison.png)

Candidate model comparison; error bars are fold standard deviations, not confidence intervals.

![Regression predictions and residuals on the holdout](figures/error-analysis.png)

Regression predictions and residuals on the holdout.

![Permutation feature importance of the selected model](figures/feature-importance.png)

Permutation feature importance of the selected model.

## Limitations

A small, historical observational benchmark; results are educational and are not a clinical model. Raw features are loaded with scaled=False, then standardized inside each training fold.

The automated pipeline cannot infer all leakage paths, sampling bias, entity grouping or business meaning. Feature importance is post-hoc and is not used to choose the winner. Any follow-up tuned after inspecting this holdout needs a fresh final evaluation.

## Reproduce

From the repository root, install requirements.txt and run:

```bash
python daily_ds/reproduce.py projects/2026-09-05-diabetes
```

The command uses the saved snapshot, configuration and dataset specification, and writes to reproduced/. analysis.ipynb provides a readable walkthrough. Numeric results may differ slightly across platforms; software versions and code hashes are recorded.
