# Source-Journal-Level Experimental Results

This document summarizes the extended experiment designed to strengthen the study for an EI-indexed journal submission.

## Extended Experimental Design

The source-journal version adds four components beyond the initial baseline:

- Multi-model comparison: Logistic Regression, Random Forest, Extra Trees, HistGradientBoosting, and MLP
- Multi-window early warning: day 7, day 14, day 28, and day 56
- Feature ablation: behavior-only, assessment-only, behavior + assessment, and all features
- Fairness mitigation: group-specific post-processing thresholds calibrated on the validation set

## Best Model by Early Warning Window

| Window | Best model | Accuracy | Precision | Recall | F1 | AUC |
|---:|---|---:|---:|---:|---:|---:|
| 7 days | MLP | 0.6949 | 0.7268 | 0.6764 | 0.7007 | 0.7675 |
| 14 days | HistGradientBoosting | 0.7177 | 0.7638 | 0.6737 | 0.7160 | 0.7964 |
| 28 days | HistGradientBoosting | 0.7582 | 0.8075 | 0.7118 | 0.7566 | 0.8463 |
| 56 days | HistGradientBoosting | 0.8038 | 0.8494 | 0.7638 | 0.8043 | 0.8873 |

The results show a clear temporal pattern: prediction performance improves as more early-course learning behavior becomes available. This supports the framing of the study as an early warning system rather than a static classification task.

## Day-28 Model Comparison

| Model | Accuracy | Precision | Recall | F1 | AUC |
|---|---:|---:|---:|---:|---:|
| HistGradientBoosting | 0.7582 | 0.8075 | 0.7118 | 0.7566 | 0.8463 |
| RandomForest | 0.7616 | 0.8240 | 0.6976 | 0.7555 | 0.8449 |
| ExtraTrees | 0.7564 | 0.8323 | 0.6746 | 0.7452 | 0.8438 |
| MLP | 0.7578 | 0.8061 | 0.7127 | 0.7565 | 0.8421 |
| LogisticRegression | 0.7362 | 0.7891 | 0.6827 | 0.7321 | 0.8204 |

At the 28-day intervention point, the best AUC is achieved by HistGradientBoosting, while RandomForest provides a very close AUC and slightly higher precision.

## Feature Ablation at Day 28

| Feature set | Accuracy | Precision | Recall | F1 | AUC |
|---|---:|---:|---:|---:|---:|
| All features | 0.7616 | 0.8240 | 0.6976 | 0.7555 | 0.8449 |
| Behavior + assessment | 0.7495 | 0.7976 | 0.7042 | 0.7480 | 0.8294 |
| Behavior only | 0.7093 | 0.7540 | 0.6671 | 0.7079 | 0.7869 |
| Assessment only | 0.7052 | 0.7237 | 0.7144 | 0.7190 | 0.7432 |

The ablation experiment shows that neither online behavior nor assessment performance alone is sufficient to maximize prediction quality. The complete feature set produces the strongest AUC, suggesting that risk prediction benefits from integrating multiple dimensions of learning activity.

## Fairness Mitigation at Day 28

The fairness mitigation experiment uses the best day-28 model, HistGradientBoosting, and applies group-specific thresholds calibrated on the validation set.

| Attribute | Method | Demographic parity gap | Equal opportunity gap | False positive rate gap |
|---|---|---:|---:|---:|
| gender | Global threshold | 0.0502 | 0.0415 | 0.0297 |
| gender | Group threshold | 0.0032 | 0.0040 | 0.0191 |
| age_band | Global threshold | 0.2856 | 0.1883 | 0.1287 |
| age_band | Group threshold | 0.1605 | 0.1162 | 0.0179 |
| disability | Global threshold | 0.0913 | 0.0447 | 0.0235 |
| disability | Group threshold | 0.0660 | 0.0284 | 0.0158 |
| region | Global threshold | 0.1469 | 0.1794 | 0.0868 |
| region | Group threshold | 0.0990 | 0.0782 | 0.1095 |

The mitigation results show that group-specific thresholds can substantially reduce equal opportunity gaps for several sensitive attributes. For example, the gender equal opportunity gap decreases from 0.0415 to 0.0040, and the regional equal opportunity gap decreases from 0.1794 to 0.0782.

## EI Source Journal Readiness

These extended results are much closer to EI source-journal expectations because the study now includes:

- A clear engineering framework for early warning
- Multiple model baselines
- Temporal early-warning analysis
- Feature ablation evidence
- Explainability with SHAP
- Fairness auditing and mitigation

The next writing step is to turn these results into the paper sections: Methodology, Experimental Setup, Results, and Ethical Discussion.
