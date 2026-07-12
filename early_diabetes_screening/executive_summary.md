<h1 align="center">Executive Summary</h1>

<h2 align="center">Early Diabetes Risk Screening Using Interpretable Machine Learning</h2>

<p align="center">
  <strong>Project Members:</strong> Tanmoy Kumar Debnath, Shirin Alimirzaei
</p>

## Problem

Diabetes is a major public health concern, and early identification of individuals at higher risk can help support timely follow-up and preventive care. The goal of this project is to build a machine learning model that can flag adults who may be at higher risk of diabetes using demographic, clinical, body-measurement, and lifestyle-related information.

This project is designed for **risk screening**, not clinical diagnosis. A positive prediction should be interpreted as **predicted high diabetes risk**, meaning the participant may need further clinical evaluation.

## Data Sources

The project uses data from the **NHANES 2017–March 2020 Pre-Pandemic cycle**. Multiple NHANES files were merged using the participant identifier `SEQN`. The data sources included:

- Demographics
- Body measurements
- Blood pressure examination
- Glycohemoglobin
- Fasting glucose
- Diabetes questionnaire
- Blood pressure and cholesterol questionnaire
- Physical activity questionnaire
- Smoking questionnaire
- Alcohol use questionnaire
- Dietary behavior questionnaire

The diabetes target variable was created using doctor-diagnosed diabetes status, HbA1c, and fasting glucose. A participant was labeled as diabetes if they reported doctor-diagnosed diabetes, had HbA1c at least 6.5%, or had fasting glucose at least 126 mg/dL.

After cleaning, the final dataset contained **9,232 adult participants**:

| Diabetes Status | Count |
|---|---:|
| Non-diabetes | 7,412 |
| Diabetes | 1,820 |

The diabetes prevalence in the cleaned dataset was approximately **19.7%**, so the dataset was imbalanced.

## Key Performance Indicators

Because this is an early screening problem, the primary KPI was 'Recall'. Recall was prioritized because the main goal was to identify as many diabetes cases as possible and reduce false negatives. Secondary KPIs included:

- PR-AUC
- ROC-AUC
- Balanced accuracy
- F1-score
- Precision
- Accuracy

Accuracy was reported for reference, but it was not used as the main model-selection metric because the dataset was imbalanced.

## Methodology

The project workflow included data collection, data assessment, data learnability, data cleaning, exploratory data analysis, feature engineering, preprocessing, model training, hyperparameter tuning, model comparison, final test evaluation, error analysis, and interpretability.

Leakage variables were removed from the predictor set, including doctor diagnosis, HbA1c, and fasting glucose, because these variables were used to define the target.

Several clinically meaningful features were engineered, including:

- Average systolic blood pressure
- Average diastolic blood pressure
- Pulse pressure
- Obesity indicator
- High waist circumference indicator
- Age-BMI interaction
- Age-waist interaction
- Metabolic risk score
- Missingness indicator features

The modeling pipeline used imputation, scaling, and one-hot encoding as needed. Models were evaluated using stratified cross-validation on the training data, and the final model was evaluated on a holdout test set.

## Models Evaluated

The baseline models are following:
- Dummy Classifier
- Logistic Regression
- Decision Tree

The advanvced models are following:
- Regularized Logistic Regression
- Random Forest
- Gradient Boosting
- XGBoost

Advanced models were tuned using cross-validation, with recall used as the main refit metric.

## Results

The cross-validation model comparison showed that **XGBoost achieved the highest diabetes recall**.

| Model | Recall | PR-AUC | ROC-AUC | Balanced Accuracy | F1-score | Precision | Accuracy |
|---|---:|---:|---:|---:|---:|---:|---:|
| Dummy Classifier | 0.0000 | 0.1972 | 0.5000 | 0.5000 | 0.0000 | 0.0000 | 0.8028 |
| Logistic Regression | 0.7520 | 0.4800 | 0.8054 | 0.7300 | 0.5114 | 0.3875 | 0.7167 |
| Decision Tree | 0.7555 | 0.4007 | 0.7635 | 0.7028 | 0.4752 | 0.3471 | 0.6708 |
| Regularized Logistic Regression | 0.7582 | 0.4795 | 0.8058 | 0.7319 | 0.5127 | 0.3874 | 0.7159 |
| Random Forest | 0.7733 | 0.4680 | 0.7941 | 0.7212 | 0.4955 | 0.3646 | 0.6896 |
| Gradient Boosting | 0.2603 | 0.4698 | 0.7982 | 0.6027 | 0.3506 | 0.5377 | 0.8100 |
| XGBoost | 0.7953 | 0.4759 | 0.7993 | 0.7306 | 0.5041 | 0.3690 | 0.6914 |

## Final Model Choice

**XGBoost** was selected as the final model because it achieved the highest cross-validated recall, which directly aligned with the project goal of early diabetes risk screening. Although Regularized Logistic Regression performed slightly better on some secondary metrics, XGBoost was preferred because recall was the primary KPI. The final model was evaluated on the holdout test set using threshold `0.50`.

| Model | Threshold | Recall | Precision | F1-score | PR-AUC | ROC-AUC | Balanced Accuracy | Accuracy |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| XGBoost | 0.50 | 0.8049 | 0.3737 | 0.5105 | 0.4513 | 0.7975 | 0.7369 | 0.6957 |

The final XGBoost model identified about **80.5% of diabetes cases** in the holdout test set.

## Interpretability

Model interpretation was performed using XGBoost feature importance and SHAP analysis. The most important predictors included age-related variables, BMI-related variables, waist circumference, blood-pressure-related variables, and metabolic risk features. These findings are clinically reasonable because diabetes risk is strongly associated with age, body size, waist circumference, and metabolic health indicators.

## Key Findings

The main findings were:

1. The dataset was imbalanced, so accuracy alone was not reliable for model selection.
2. Diabetes rate increased across older age groups.
3. BMI and waist circumference were strongly related to diabetes risk patterns.
4. XGBoost achieved the highest recall among the evaluated models.
5. The final XGBoost model identified about 80.5% of diabetes cases in the holdout test set.
6. The model produced false positives, which is acceptable for screening but limits diagnostic use.
7. SHAP analysis showed that age, BMI-related features, waist circumference, blood pressure, and metabolic risk features were important.
8. Calibration analysis showed that predicted probabilities should be interpreted as relative screening scores, not exact clinical risk probabilities.

## Limitations

This project has several limitations:

- False positives are present because recall was prioritized.
- Some diabetes cases may still be missed.
- The predicted probabilities are not perfectly calibrated.
- NHANES is cross-sectional, so the model does not predict future diabetes onset over time.
- External validation on another dataset is needed before any real-world use.
- Some subgroup differences in performance were observed and should be investigated further.

## Next Steps

Future work could include:

- Applying probability calibration to improve risk probability estimates.
- Performing more detailed subgroup fairness and robustness analysis.
- Testing additional feature engineering strategies.
- Validating the model on an external dataset.
- Comparing alternative threshold-selection strategies.
- Developing a simple screening demonstration interface.
- Exploring more interpretable screening models for comparison with XGBoost.

## Conclusion

This project developed a leakage-aware, recall-focused diabetes risk screening pipeline using NHANES data. XGBoost was selected as the final model because it achieved the strongest recall, identifying about 80.5% of diabetes cases in the holdout test set. The model is useful as a screening tool for identifying higher-risk participants, but it should not be used as a clinical diagnosis system. Further calibration, subgroup analysis, and external validation would be needed before practical deployment.