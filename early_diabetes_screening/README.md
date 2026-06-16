##### &#x20;           **Early Diabetes Risk Screening Using Interpretable Machine Learning**





###### **Problem Statement**

Diabetes is a major public health concern, and early identification of high-risk individuals can help people take precautionary actions before the condition becomes severe. The goal of this project is to develop a practical and interpretable machine learning framework for early diabetes risk screening.



This project uses raw NHANES 2017-March 2020 Pre-Pandemic data collected from multiple CDC files, including demographics, dietary, examination, laboratory, and questionnaire modules. These files are merged using the participant identifier SEQN. Special missing-value codes are cleaned, and a diabetes target variable is constructed using self-reported doctor diagnosis and clinical laboratory thresholds.



The project explores important demographic, clinical, and lifestyle factors related to diabetes risk, including age, BMI, waist circumference, blood pressure, cholesterol history, physical activity, smoking status, diet behavior, alcohol behavior, education, and income. The final goal is to build an explainable early diabetes screening model that can identify individuals at higher risk and highlight important risk factors so that lifestyle changes, regular monitoring, and further medical consultation can be encouraged early.



###### **Diabetes Target Definition**



The diabetes target is constructed using the following criteria:



Self-reported doctor diagnosis of diabetes

HbA1c greater than or equal to 6.5

Fasting glucose greater than or equal to 126



A participant is labeled as diabetic if at least one of these conditions is satisfied.



Variables used to define the target, including self-reported diabetes diagnosis, HbA1c, and fasting glucose, are excluded from the predictor variables to avoid target leakage.



###### **Modeling Plan**



This project will compare several machine learning models for early diabetes risk screening, including:



\-Logistic Regression

\-Random Forest

\-XGBoost



Because this is a screening problem, model evaluation will focus not only on accuracy but also on recall, PR-AUC, ROC-AUC, F1-score, and false negative reduction.



###### **Project Notebooks**

notebooks/data\_collection.ipynb: Loads and merges raw NHANES data files.

notebooks/data\_cleaning.ipynb: Cleans missing-value codes, filters adult participants, and creates the diabetes target.

notebooks/data\_assessment.ipynb: Assesses data volume, coverage, missingness, representativeness, and potential bias.

notebooks/data\_learnability.ipynb: Runs baseline learnability tests using trivial, linear, and tree-based models.



