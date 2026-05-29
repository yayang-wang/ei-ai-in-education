# Explainable and Fair AI in Education Lab

This repository contains a reproducible experiment for an EI-style AI in education study:

**Explainable and Fair AI-Based Early Warning System for Student Learning Risk in Online Education**

The project uses the Open University Learning Analytics Dataset (OULAD) to build a learning risk early warning model, explain predictions, and evaluate fairness across student groups.

## Research Focus

- Early prediction of student learning risk using first 4 weeks of online learning data
- Explainable AI analysis with SHAP or permutation importance
- Fairness auditing across student background variables
- Human-in-the-loop educational intervention design

## Dataset

The experiment uses the Open University Learning Analytics Dataset:

https://analyse.kmi.open.ac.uk/open-dataset

The raw dataset is not included in this repository. Download OULAD from the official site and extract the CSV files into:

```text
data/oulad/
```

Expected files include:

```text
studentInfo.csv
studentVle.csv
studentAssessment.csv
assessments.csv
studentRegistration.csv
vle.csv
courses.csv
```

## How to Run

1. Install dependencies.

```bash
pip install -r requirements.txt
```

2. Open the notebook.

```bash
jupyter lab oulad_explainable_fairness_lab.ipynb
```

3. Run the cells from top to bottom.

The notebook can also try to download OULAD automatically. If automatic download fails, download it manually from the official OULAD page.

## Outputs

Generated files are saved under:

```text
outputs/figures/
outputs/tables/
```

These outputs include model performance, confusion matrix, explainability figures, prediction results, and fairness tables.

The first completed experimental run is summarized in:

```text
RESULTS.md
results/
```

## Suggested Paper Title

**Design and Empirical Evaluation of an Explainable and Fair Online Learning Risk Early Warning System**

## Citation

When using OULAD, cite:

Kuzilek, J., Hlosta, M., & Zdrahal, Z. (2017). Open University Learning Analytics dataset. Scientific Data, 4, 170171. https://doi.org/10.1038/sdata.2017.171
