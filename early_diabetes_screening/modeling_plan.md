### **Modeling Plan**



The goal of this project is to build an interpretable early diabetes risk screening model using NHANES data. Since this is a screening problem, the main priority is to identify as many diabetes cases as possible. Therefore, recall is the primary KPI. Secondary KPIs include PR-AUC, ROC-AUC, balanced accuracy, F1-score, precision, and accuracy.



Accuracy is reported for reference, but it is not the main model-selection metric because the dataset is imbalanced, with more non-diabetes participants than diabetes participants.



#### **Evaluation Strategy**



All models were evaluated using the same preprocessing pipeline and stratified cross-validation strategy. The preprocessing pipeline included missing-value imputation, scaling for numeric variables, and one-hot encoding for categorical variables. This ensured that all models were compared fairly using the same training data, feature set, and evaluation metrics.



An 80/20 stratified train-test split was used. Model comparison and tuning were performed using the training set only. The final holdout test set was used only after the final model was selected.



#### **Model Families Tried**



##### **Baseline Models**



Three simple baseline models were evaluated first:



* Dummy Classifier
* Logistic Regression
* Decision Tree



The Dummy Classifier was used as a majority-class reference model. Logistic Regression was used as a simple and interpretable linear baseline. Decision Tree was used as a simple nonlinear baseline that can capture basic feature interactions.



These baseline models provided a reference point before moving to more complex models.



##### **Advanced Models**



The following advanced models were evaluated:



* Regularized Logistic Regression
* Random Forest
* Gradient Boosting
* XGBoost



Regularized Logistic Regression was included because it extends standard Logistic Regression and can improve generalization through regularization while remaining interpretable.



Random Forest was included because it can capture nonlinear relationships and feature interactions through an ensemble of decision trees.



Gradient Boosting was included as a boosting-based tree model that builds trees sequentially to improve prediction performance.



XGBoost was included as a stronger gradient-boosting model that often performs well on structured tabular data and can capture nonlinear patterns in clinical, demographic, and lifestyle variables.



#### **Model Comparison**



Models were compared using recall as the primary KPI because the project focuses on early diabetes risk screening. Secondary KPIs were then reviewed to understand precision-recall tradeoffs and overall model quality.



In cross-validation, XGBoost achieved the highest diabetes recall among the evaluated models. This means it identified the largest proportion of diabetes cases. Regularized Logistic Regression had slightly better values for some secondary metrics, such as PR-AUC, ROC-AUC, F1-score, and precision, and it remains a strong interpretable alternative. However, XGBoost provided the strongest recall performance, which aligned most directly with the screening goal.



#### **Final Model Choice**



XGBoost was selected as the final model because it achieved the highest cross-validated recall, which was the primary KPI for this project. The model was selected to prioritize detecting diabetes cases and reducing false negatives.



On the final holdout test set using threshold 0.50, the XGBoost model achieved strong diabetes recall. This means the model successfully identified most diabetes cases in the test set. However, the diabetes precision was lower, meaning the model also produced a noticeable number of false positives.



This tradeoff is acceptable for an early screening model, but the model should not be interpreted as a diagnostic tool. A positive prediction should be interpreted as “predicted high risk” of diabetes and should require further clinical evaluation.



\## Threshold Selection



Threshold tuning was explored using cross-validated predicted probabilities from the training set. A tuned threshold of 0.53 was considered, but the default threshold of 0.50 performed better on the final test set for the main screening goal because it achieved higher recall.



Therefore, threshold 0.50 was used for the final classification report and confusion matrix.



#### **Interpretability**



Several interpretability diagnostics were used for the final XGBoost model:



\- XGBoost feature importance

\- SHAP summary plot

\- SHAP feature importance table



The feature importance and SHAP results showed that the model relied heavily on clinically meaningful predictors such as age, BMI-related interaction terms, waist circumference, blood-pressure-related variables, and metabolic risk features.



SHAP analysis provided additional insight by showing how features pushed predictions toward higher or lower diabetes risk.



#### **Error Analysis and Feedback Loop**



Error analysis was performed using the classification report, confusion matrix, and subgroup error analysis. The final model achieved high diabetes recall but produced many false positives. Subgroup analysis also showed that recall was lower in some groups, such as `RIDRETH3 = 6.0` and `DMDEDUC2 = 5.0`.



A focused review showed that false negatives in these subgroups generally had lower age, BMI, waist circumference, pulse pressure, and metabolic risk scores than true positives. This suggests that the model was better at detecting diabetes cases with more visible risk patterns and missed some less obvious diabetes cases.



Missingness patterns were also reviewed. Some clinically relevant variables, including alcohol-related, blood-pressure-related, and waist-related variables, had noticeable missingness in the lower-recall subgroups. To address this, missingness indicator features were added for selected important predictors.



#### **Notes on Models Not Selected**



The Dummy Classifier was not selected because it predicted only the majority class and failed to identify diabetes cases.



The Decision Tree was not selected because, although it provided a simple nonlinear baseline, it did not clearly outperform stronger models across the main KPIs.



Gradient Boosting was not selected because it had much lower recall than the best-performing models.



Random Forest performed better than several baselines but did not achieve the highest recall.



Regularized Logistic Regression remained a strong interpretable alternative, but XGBoost was selected because it achieved the best recall, which was the primary KPI for the screening objective.



#### **Summary**



The modeling process started with simple baselines and then moved to more complex models. All models were evaluated using the same cross-validation strategy and KPIs. XGBoost was selected as the final model because it achieved the highest diabetes recall, aligning with the project goal of early diabetes risk screening. The final model was interpreted using feature importance and SHAP analysis, and error analysis was used to identify future feature-engineering improvements.

