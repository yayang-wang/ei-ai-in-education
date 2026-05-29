# First Experimental Results

This file summarizes the first completed run of the OULAD explainable and fairness-aware learning risk early warning experiment.

## Experimental Setting

- Dataset: Open University Learning Analytics Dataset (OULAD)
- Prediction target: high-risk student = `Fail` or `Withdrawn`
- Early warning window: first 28 course days
- Model: Random Forest classifier
- Sensitive attributes audited: gender, age band, disability, IMD band, region

## Model Performance

| Metric | Score |
|---|---:|
| Accuracy | 0.7675 |
| Precision | 0.8241 |
| Recall | 0.7113 |
| F1 | 0.7636 |
| AUC | 0.8469 |

The first baseline model is strong enough to support a paper-level experiment. The AUC of 0.8469 indicates that early learning behavior during the first four weeks contains meaningful signals for identifying students at risk.

## Fairness Summary

| Attribute | Demographic parity gap | Equal opportunity gap | False positive rate gap |
|---|---:|---:|---:|
| region | 0.1553 | 0.1551 | 0.1266 |
| age_band | 0.1817 | 0.1155 | 0.0558 |
| imd_band | 0.1710 | 0.0791 | 0.0895 |
| gender | 0.0440 | 0.0541 | 0.0184 |
| disability | 0.0754 | 0.0136 | 0.0184 |

The largest equal opportunity gap appears across regions, followed by age bands. This suggests that the model may identify actual at-risk students at different rates across student groups. This is a useful ethical finding for the paper because it connects predictive modeling with fairness auditing and human-in-the-loop intervention design.

## Group-Level Findings

Gender:

| Group | N | Actual risk rate | Predicted risk rate | TPR | FPR | Precision |
|---|---:|---:|---:|---:|---:|---:|
| M | 4489 | 0.5335 | 0.4754 | 0.7353 | 0.1781 | 0.8252 |
| F | 3660 | 0.5210 | 0.4314 | 0.6812 | 0.1597 | 0.8227 |

Disability:

| Group | N | Actual risk rate | Predicted risk rate | TPR | FPR | Precision |
|---|---:|---:|---:|---:|---:|---:|
| N | 7343 | 0.5170 | 0.4482 | 0.7097 | 0.1683 | 0.8186 |
| Y | 806 | 0.6278 | 0.5236 | 0.7233 | 0.1867 | 0.8673 |

Age:

| Group | N | Actual risk rate | Predicted risk rate | TPR | FPR | Precision |
|---|---:|---:|---:|---:|---:|---:|
| 0-35 | 5752 | 0.5567 | 0.4833 | 0.7242 | 0.1808 | 0.8342 |
| 35-55 | 2334 | 0.4614 | 0.3916 | 0.6750 | 0.1488 | 0.7954 |
| 55<= | 63 | 0.3651 | 0.3016 | 0.6087 | 0.1250 | 0.7368 |

## Generated Artifacts

Local generated figures:

- `results/figures/confusion_matrix.png`
- `results/figures/shap_summary.png`

Versioned result tables:

- `results/tables/model_metrics.csv`
- `results/tables/fairness_summary.csv`

## Paper Interpretation

These results support the core claim that an early warning model can achieve good predictive performance while still requiring fairness and explainability checks before being used in educational decision-making. The paper should emphasize that the system is not intended to automatically label or punish students; it should provide transparent evidence to teachers and support timely, human-reviewed intervention.
