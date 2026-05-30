# Design and Empirical Evaluation of an Explainable and Fair Online Learning Risk Early Warning System

## Abstract

Artificial intelligence has been increasingly used in online education to identify students who may be at risk of failure or withdrawal. However, many learning risk prediction systems remain difficult for teachers to interpret and may produce uneven outcomes across different student groups. To address these issues, this study proposes an explainable and fairness-aware online learning risk early warning framework. The framework integrates early-course behavioral feature engineering, machine learning-based risk prediction, explainability analysis, group-level fairness auditing, and human-in-the-loop intervention design. Using the Open University Learning Analytics Dataset (OULAD), we construct learning risk prediction tasks based on four early warning windows: 7, 14, 28, and 56 days. Five models are compared, including Logistic Regression, Random Forest, Extra Trees, HistGradientBoosting, and Multilayer Perceptron. The experimental results show that prediction performance improves as more early learning behavior becomes available. The best model achieves an AUC of 0.8463 at day 28 and 0.8873 at day 56. Feature ablation results further show that the integration of learning behavior, assessment performance, registration information, and student background features outperforms behavior-only and assessment-only settings. Fairness auditing reveals performance gaps across sensitive attributes such as gender, age band, disability, socioeconomic band, and region. A group-specific threshold post-processing method substantially reduces several equal opportunity gaps, including the gender gap from 0.0415 to 0.0040 and the regional gap from 0.1794 to 0.0782. These findings suggest that educational early warning systems should not be evaluated only by predictive accuracy; explainability and fairness are also necessary for responsible deployment in learning environments.

**Keywords:** AI in education; learning analytics; early warning system; explainable AI; fairness; student risk prediction; OULAD

## 1. Introduction

Online and blended learning platforms generate large volumes of behavioral traces, including resource access, video interaction, assessment submission, forum participation, and registration records. These data provide opportunities for learning analytics and educational data mining systems to identify students who may require timely support. Student learning risk early warning systems are especially important because withdrawal and failure often develop gradually during the early stages of a course. If risk can be detected early, teachers and institutions can provide targeted academic support before students become disengaged.

Despite their potential value, AI-based early warning systems also raise ethical and practical concerns. First, many machine learning models operate as black boxes, making it difficult for teachers to understand why a student is classified as high risk. A prediction without explanation may reduce teacher trust and may also lead to inappropriate interventions. Second, prediction systems may perform unevenly across student groups. For example, a model may detect risk more effectively for one age group than another, or it may generate more false alerts for students from specific regions or socioeconomic backgrounds. Such uneven behavior is especially sensitive in educational contexts because model outputs may influence teacher attention, institutional resources, and student self-perception.

The engineering challenge is therefore not simply to build a more accurate classifier. A deployable educational AI system must connect prediction, explanation, fairness auditing, and intervention governance into a reproducible workflow. Many existing studies report model performance, but fewer studies examine how early warning time windows, feature sources, explanation evidence, and group fairness metrics jointly shape the responsible use of educational predictions. This gap limits the practical value of learning risk prediction systems in real teaching scenarios.

Therefore, responsible AI in education requires more than maximizing predictive accuracy. A practical early warning system should be accurate, interpretable, and fair enough to support human decision-making. It should also avoid using risk predictions as automatic labels or punitive decisions. Instead, model outputs should help teachers understand learning signals, review possible risk factors, and provide supportive interventions.

This study proposes and evaluates an explainable and fairness-aware learning risk early warning framework for online education. The framework is designed around four components: early learning feature construction, machine learning-based risk prediction, explainability analysis, and fairness auditing with mitigation. The empirical study uses the Open University Learning Analytics Dataset (OULAD), a public dataset containing student demographics, course registration information, online learning activity, assessment records, and final course outcomes.

The research questions are:

**RQ1:** How does early warning performance change across different prediction windows in an online learning environment?

**RQ2:** How do different feature groups, including behavior, assessment, registration, and background information, contribute to learning risk prediction?

**RQ3:** Do educational risk prediction models produce unequal error patterns across student groups, and can transparent post-processing reduce these gaps?

**RQ4:** How can prediction, explainability, and fairness auditing be integrated into a teacher-facing human-in-the-loop early warning workflow?

The main contributions of this study are as follows:

1. The study proposes an engineering-oriented responsible AI framework for online learning risk early warning, integrating prediction, explanation, fairness auditing, mitigation, and human-in-the-loop intervention.
2. The study conducts a multi-window early warning experiment at 7, 14, 28, and 56 days, showing how the value of learning behavior changes over time.
3. The study compares five machine learning models and performs feature ablation to distinguish the roles of behavioral, assessment, registration, and contextual features.
4. The study evaluates group-level fairness using demographic parity, equal opportunity, and false positive rate gaps across gender, age band, disability, socioeconomic band, and region.
5. The study applies a transparent group-threshold mitigation method and discusses its ethical boundary conditions for teacher-supported educational decision-making.

## 2. Related Work

### 2.1 AI-Based Learning Risk Prediction

Learning risk prediction is a central task in learning analytics and educational data mining. Existing studies commonly use student demographic information, online learning behavior, assessment records, and historical academic data to predict outcomes such as failure, dropout, or low performance. Traditional models include Logistic Regression, Decision Trees, Random Forests, and Support Vector Machines, while more recent work has explored gradient boosting methods, neural networks, and sequence-based models.

However, many studies focus primarily on predictive performance. In practical educational settings, accuracy alone is insufficient because teachers must understand prediction reasons and decide what kind of intervention is appropriate. A model that improves AUC but cannot support interpretable intervention may have limited educational value. This motivates the integration of explainable AI methods into learning risk prediction systems.

### 2.2 Explainable AI in Education

Explainable AI aims to make model behavior understandable to human users. In education, explainability can help teachers identify which learning behaviors are most associated with risk, such as low platform engagement, few active learning days, poor assessment scores, or late submissions. Post hoc explanation methods such as SHAP and permutation importance are commonly used to estimate feature contributions after a model has been trained.

For early warning systems, explainability has two roles. At the global level, it helps researchers and institutions understand what factors drive risk predictions across the dataset. At the local level, it can help teachers interpret why an individual student is flagged as high risk. This study focuses mainly on global explainability as a first step toward a teacher-facing decision support system, and positions explanation as part of an intervention workflow rather than as a standalone visualization.

### 2.3 Fairness and Ethics in AI Education Systems

Fairness is increasingly important in AI-supported education because educational predictions may influence access to support, teacher attention, and institutional decision-making. A model that performs well overall may still produce unequal outcomes across demographic or socioeconomic groups. Common fairness metrics include demographic parity gap, equal opportunity gap, and false positive rate gap.

In learning risk prediction, fairness is especially complex. A higher predicted risk rate for a group may reflect real differences in learning outcomes, model bias, or both. Therefore, fairness auditing should not be treated as a purely technical procedure. Instead, it should be combined with human review, contextual interpretation, and supportive intervention design. This study addresses this issue by linking fairness metrics to a transparent mitigation strategy and by explicitly discussing how model outputs should be governed in educational practice.

## 3. Proposed Framework

The proposed framework contains five stages:

1. **Data integration:** student demographic information, registration records, online activity logs, and assessment records are integrated by course and student identifiers.
2. **Early feature construction:** learning behavior and assessment features are constructed within a specified early warning window.
3. **Risk prediction:** machine learning models estimate the probability that a student will fail or withdraw.
4. **Explanation and fairness auditing:** model outputs are analyzed through explainability methods and group-level fairness metrics.
5. **Human-in-the-loop intervention:** predictions are used as decision support for teachers rather than automatic labels.

The prediction target is defined as a binary variable. Students with final outcomes of `Fail` or `Withdrawn` are labeled as high-risk students, while students with `Pass` or `Distinction` are labeled as non-risk students.

## 4. Methodology

### 4.1 Dataset

This study uses the Open University Learning Analytics Dataset (OULAD). The dataset contains information about students enrolled in online courses at the Open University, including student background variables, registration data, virtual learning environment interactions, assessment records, and final course outcomes. OULAD is suitable for this study because it supports both learning risk prediction and fairness analysis. It contains sensitive or background variables such as gender, age band, disability, socioeconomic band, and region.

### 4.2 Feature Engineering

For each early warning window, features are constructed using only data available from day 0 to the window endpoint. Four windows are used: 7, 14, 28, and 56 days. This design simulates realistic early warning scenarios in which future data are not available at prediction time.

The main feature groups include:

- Online behavior features: total clicks, active days, average clicks per event, maximum clicks per event, accessed resource count, activity-type click counts, and clicks per active day.
- Assessment features: submitted assessment count, mean score, minimum score, weighted score sum, and late submission count.
- Registration features: registration date, whether the student registered before course start, and whether early unregistration occurred.
- Course and background features: course module, course presentation, studied credits, and previous attempts.

### 4.3 Models

Five machine learning models are evaluated:

- Logistic Regression
- Random Forest
- Extra Trees
- HistGradientBoosting
- Multilayer Perceptron

These models represent linear, ensemble, boosting, and neural network approaches. The comparison provides stronger evidence than relying on a single model.

### 4.4 Evaluation Metrics

Prediction performance is evaluated using accuracy, precision, recall, F1-score, and AUC. AUC is used as the primary ranking metric because it measures discrimination ability across thresholds.

Fairness is evaluated using three group-level metrics:

- **Demographic parity gap:** difference between the maximum and minimum predicted risk rates across groups.
- **Equal opportunity gap:** difference between the maximum and minimum true positive rates across groups.
- **False positive rate gap:** difference between the maximum and minimum false positive rates across groups.

### 4.5 Fairness Mitigation

A group-specific threshold post-processing strategy is used as a fairness mitigation method. First, a global model is trained without directly using sensitive attributes as model inputs. Then, thresholds are calibrated on the validation set for each sensitive group to approximate a target recall level. The calibrated thresholds are applied to the test set. This method is simple, transparent, and suitable for analyzing the trade-off between predictive performance and fairness gaps.

## 5. Experimental Setup

The dataset is split into training, validation, and test sets. The training set is used to fit the model, the validation set is used for group-specific threshold calibration, and the test set is used for final evaluation. The random seed is fixed to improve reproducibility.

The source-journal experiment includes three major analyses:

1. **Model and time-window comparison:** all five models are evaluated across 7, 14, 28, and 56 days.
2. **Feature ablation:** Random Forest is used to compare behavior-only, assessment-only, behavior + assessment, and full feature settings at day 28.
3. **Fairness mitigation:** the best day-28 model is used to compare global thresholding and group-specific thresholding across sensitive attributes.

To strengthen the engineering validity of the study, the experimental design follows four principles. First, the temporal design evaluates whether the system can provide useful signals early enough for intervention. Second, the model comparison avoids relying on a single algorithm and provides linear, ensemble, boosting, and neural baselines. Third, the feature ablation study tests whether the proposed feature integration strategy is necessary. Fourth, the fairness experiment evaluates not only overall accuracy but also group-level error patterns that may affect the equitable allocation of teacher attention and institutional support.

## 6. Results

### 6.1 Multi-Window Model Comparison

The best model performance improves as the early warning window becomes longer. At day 7, the best AUC is 0.7675, achieved by MLP. At day 14, HistGradientBoosting achieves an AUC of 0.7964. At day 28, HistGradientBoosting achieves an AUC of 0.8463. At day 56, HistGradientBoosting achieves the best AUC of 0.8873.

| Window | Best model | Accuracy | Precision | Recall | F1 | AUC |
|---:|---|---:|---:|---:|---:|---:|
| 7 days | MLP | 0.6949 | 0.7268 | 0.6764 | 0.7007 | 0.7675 |
| 14 days | HistGradientBoosting | 0.7177 | 0.7638 | 0.6737 | 0.7160 | 0.7964 |
| 28 days | HistGradientBoosting | 0.7582 | 0.8075 | 0.7118 | 0.7566 | 0.8463 |
| 56 days | HistGradientBoosting | 0.8038 | 0.8494 | 0.7638 | 0.8043 | 0.8873 |

These results indicate that early behavioral and assessment signals are already useful within the first two weeks, but prediction becomes substantially stronger by day 28 and day 56. This supports the feasibility of using AI-based early warning systems to guide timely educational interventions.

### 6.2 Day-28 Model Comparison

At day 28, HistGradientBoosting achieves the highest AUC, while Random Forest achieves slightly higher accuracy and precision. Logistic Regression performs worse than the nonlinear models, suggesting that learning risk prediction benefits from modeling nonlinear relationships among activity, assessment, and registration features.

| Model | Accuracy | Precision | Recall | F1 | AUC |
|---|---:|---:|---:|---:|---:|
| HistGradientBoosting | 0.7582 | 0.8075 | 0.7118 | 0.7566 | 0.8463 |
| RandomForest | 0.7616 | 0.8240 | 0.6976 | 0.7555 | 0.8449 |
| ExtraTrees | 0.7564 | 0.8323 | 0.6746 | 0.7452 | 0.8438 |
| MLP | 0.7578 | 0.8061 | 0.7127 | 0.7565 | 0.8421 |
| LogisticRegression | 0.7362 | 0.7891 | 0.6827 | 0.7321 | 0.8204 |

### 6.3 Feature Ablation

The full feature set achieves the best AUC of 0.8449 in the Random Forest ablation experiment. Behavior + assessment features achieve an AUC of 0.8294, while behavior-only and assessment-only settings achieve lower AUC values of 0.7869 and 0.7432, respectively.

| Feature set | Accuracy | Precision | Recall | F1 | AUC |
|---|---:|---:|---:|---:|---:|
| All features | 0.7616 | 0.8240 | 0.6976 | 0.7555 | 0.8449 |
| Behavior + assessment | 0.7495 | 0.7976 | 0.7042 | 0.7480 | 0.8294 |
| Behavior only | 0.7093 | 0.7540 | 0.6671 | 0.7079 | 0.7869 |
| Assessment only | 0.7052 | 0.7237 | 0.7144 | 0.7190 | 0.7432 |

This result suggests that different data sources capture complementary aspects of learning risk. Online behavior reflects engagement, assessment features reflect academic performance, and registration/background features add contextual information.

### 6.4 Fairness Mitigation

Fairness auditing shows that group-level performance gaps exist under a global decision threshold. Group-specific thresholding reduces several gaps, especially equal opportunity gaps.

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

The gender equal opportunity gap decreases from 0.0415 to 0.0040, and the regional equal opportunity gap decreases from 0.1794 to 0.0782. These results show that fairness-aware post-processing can reduce unequal detection rates while maintaining a similar level of predictive performance.

### 6.5 Engineering Interpretation

The experimental results support the proposed framework in three ways. First, the multi-window results show that day-28 is a practical intervention point: it provides substantially stronger prediction than day 7 or day 14 while still leaving time for teachers to intervene before final outcomes are determined. Second, the ablation results show that learning risk is multidimensional. Behavior-only signals capture engagement, assessment-only signals capture academic performance, and the full feature set better captures the interaction among engagement, performance, registration, and background context. Third, the fairness results show that a high-performing model can still produce unequal group-level outcomes, which means that fairness auditing should be part of the deployment pipeline rather than a separate afterthought.

From an engineering perspective, these findings suggest that a responsible educational early warning system should be evaluated as a pipeline. The pipeline must answer four questions before deployment: whether the model predicts risk accurately, whether teachers can understand the major risk signals, whether prediction errors are unevenly distributed across student groups, and whether mitigation can reduce problematic gaps without removing human judgment from the intervention process.

## 7. Discussion and Ethical Implications

The results highlight an important ethical point: an educational AI system with good overall AUC may still behave differently across student groups. Therefore, fairness auditing should be a standard part of educational early warning system design. However, fairness mitigation must be applied carefully. Group-specific thresholds may reduce certain statistical gaps, but they also require institutional justification, transparency, and ongoing monitoring.

In practice, the system should be used as a teacher-support tool rather than an automated decision-maker. A high-risk prediction should not be interpreted as a fixed label or a basis for punishment. Instead, it should trigger a human review process in which teachers examine explanation results, consider contextual information, and choose supportive interventions. Possible interventions include personalized feedback, tutoring recommendations, learning resource suggestions, and outreach messages.

The proposed framework therefore follows a human-in-the-loop principle. AI provides evidence, but teachers remain responsible for interpretation and action. This design can reduce the risk of over-reliance on automated predictions and help align AI-supported education with transparency, fairness, and student support.

The ethical contribution of this study is not limited to identifying fairness gaps. It also proposes a practical governance logic for educational AI. Prediction should be treated as a signal for support, explanation should be used to make the signal inspectable, fairness auditing should be used to monitor uneven consequences, and mitigation should be applied only with institutional accountability. This view prevents the system from becoming a purely automated ranking mechanism and keeps the final intervention decision within a professional educational context.

There are also limitations. First, OULAD is a public historical dataset, so the results should be validated on more recent institutional datasets before real deployment. Second, this study focuses mainly on global explainability; future work should develop local student-level explanations that teachers can inspect case by case. Third, group-specific thresholding is transparent but may not solve all forms of educational unfairness, especially when disparities are caused by structural learning conditions rather than model behavior. Fourth, future studies should include teacher-facing usability evaluation to examine whether explanation and fairness information actually improves intervention decisions.

## 8. Conclusion

This study proposed an explainable and fairness-aware online learning risk early warning framework and evaluated it using the OULAD dataset. The experiment compared five machine learning models across four early warning windows, performed feature ablation, and applied fairness auditing and threshold-based mitigation. The results show that prediction performance improves from day 7 to day 56, with HistGradientBoosting achieving an AUC of 0.8463 at day 28 and 0.8873 at day 56. Feature ablation confirms that integrating multiple feature groups improves predictive performance. Fairness analysis further shows that group-level disparities exist and can be reduced through post-processing thresholds.

The findings suggest that responsible AI-based early warning systems should combine prediction, explanation, fairness auditing, and human intervention. Future work will further develop local student-level explanations, evaluate teacher trust through user studies, and test the framework across additional educational datasets.

## References

### Strict Submission Working References

The following working list is restricted to recent high-impact journals that are plausible
SCI/SSCI JCR Q1 or equivalent high-quartile venues for this topic. Final submission still needs
year-specific verification in Web of Science/JCR or the CAS journal ranking system.

1. Baneres, D., Rodriguez-Gonzalez, M. E., Guerrero-Roldan, A.-E., & Cortadas, P. (2023). An early warning system to identify and intervene online dropout learners. *International Journal of Educational Technology in Higher Education, 20*, Article 3. https://doi.org/10.1186/s41239-022-00371-5

2. Crompton, H., & Burke, D. (2023). Artificial intelligence in higher education: The state of the field. *International Journal of Educational Technology in Higher Education, 20*, Article 22. https://doi.org/10.1186/s41239-023-00392-8

3. Bond, M., Khosravi, H., De Laat, M., Bergdahl, N., Negrea, V., Oxley, E., Pham, P., Chong, S. W., & Siemens, G. (2024). A meta systematic review of artificial intelligence in higher education: A call for increased ethics, collaboration, and rigour. *International Journal of Educational Technology in Higher Education, 21*, Article 4. https://doi.org/10.1186/s41239-023-00436-z

4. Mah, D.-K., & Gross, N. (2024). Artificial intelligence in higher education: Exploring faculty use, self-efficacy, distinct profiles, and professional development needs. *International Journal of Educational Technology in Higher Education, 21*, Article 58. https://doi.org/10.1186/s41239-024-00490-1

5. Kamali, J., Alpat, M. F., & Bozkurt, A. (2024). AI ethics as a complex and multifaceted challenge: Decoding educators' AI ethics alignment through the lens of activity theory. *International Journal of Educational Technology in Higher Education, 21*, Article 62. https://doi.org/10.1186/s41239-024-00496-9

6. Delcker, J., Heil, J., Ifenthaler, D., Seufert, S., & Spirgi, L. (2024). First-year students AI-competence as a predictor for intended and de facto use of AI-tools for supporting learning processes in higher education. *International Journal of Educational Technology in Higher Education, 21*, Article 18. https://doi.org/10.1186/s41239-024-00452-7

7. Paulsen, L., & Lindsay, E. (2024). Learning analytics dashboards are increasingly becoming about learning and not just analytics: A systematic review. *Education and Information Technologies, 29*, 14279-14308. https://doi.org/10.1007/s10639-023-12401-4

8. Stojanov, A., & Daniel, B. K. (2024). A decade of research into the application of big data and analytics in higher education: A systematic review of the literature. *Education and Information Technologies, 29*, 5807-5831. https://doi.org/10.1007/s10639-023-12033-8

9. Tiukhova, E., Vemuri, P., Lopez Flores, N., Islind, A. S., Oskarsdottir, M., Poelmans, S., Baesens, B., & Snoeck, M. (2024). Explainable Learning Analytics: Assessing the stability of student success prediction models by means of explainable AI. *Decision Support Systems, 182*, 114229. https://doi.org/10.1016/j.dss.2024.114229

10. Zhang, Y., et al. (2025). The affordances of Artificial Intelligence (AI) and ethical considerations across the instruction cycle: A systematic review of AI in online higher education. *The Internet and Higher Education, 67*, 101039. https://doi.org/10.1016/j.iheduc.2025.101039

11. Nguyen, A., Ngo, H. N., Hong, Y., Dang, B., & Nguyen, B.-P. T. (2023). Ethical principles for artificial intelligence in education. *Education and Information Technologies, 28*, 4221-4241. https://doi.org/10.1007/s10639-022-11316-w

12. Dwivedi, R., Dave, D., Naik, H., Singhal, S., Omer, R., Patel, P., Qian, B., Wen, Z., Shah, T., Morgan, G., & Ranjan, R. (2023). Explainable AI (XAI): Core ideas, techniques, and solutions. *ACM Computing Surveys, 55*(9), Article 194. https://doi.org/10.1145/3561048

13. Ali, S., Abuhmed, T., El-Sappagh, S., Muhammad, K., Alonso-Moral, J. M., Confalonieri, R., Guidotti, R., Del Ser, J., Diaz-Rodriguez, N., & Herrera, F. (2023). Explainable Artificial Intelligence (XAI): What we know and what is left to attain Trustworthy Artificial Intelligence. *Information Fusion, 99*, 101805. https://doi.org/10.1016/j.inffus.2023.101805

14. Pagano, T. P., Loureiro, R. B., Lisboa, F. V. N., Cruz, G. O. R., Peixoto, R. M., Guimaraes, G. A. S., Santos, L. L., Araujo, M. M., Cruz, M. A. S., Oliveira, E. L. S., Winkler, I., & Nascimento, E. G. S. (2023). Bias and unfairness in machine learning models: A systematic literature review. *Artificial Intelligence Review, 56*, 13547-13624. https://doi.org/10.1007/s10462-023-10502-1

15. Kuzilek, J., Hlosta, M., & Zdrahal, Z. (2017). Open University Learning Analytics dataset. *Scientific Data, 4*, 170171. https://doi.org/10.1038/sdata.2017.171
