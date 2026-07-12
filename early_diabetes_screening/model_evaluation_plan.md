## **Model Evaluation Plan**



The evaluation plan was created before final modeling to define a fair and leakage-free modeling workflow. This step is important because model performance can be misleading if the data split, metrics, leakage checks, and test-set rules are not defined in advance. For this checkpoint, baseline models were evaluated using the planned framework. More advanced modeling and hyperparameter tuning will be performed later using the same evaluation structure.



#### **Evaluation Framework**

* Split the data
* Train models using cross-validation
* Compare recall, PR-AUC, ROC-AUC, balanced accuracy, F1
* Select a model
* Evaluate it on final holdout test set
* Show classification report
* Show confusion matrix
* Show ROC and PR curves
* Run shuffled-target check
* Run subgroup performance check



#### **Unit of Analysis**



Each row in the cleaned dataset represents one adult NHANES participant. Therefore, the unit of analysis is the individual participant. The model is designed to predict diabetes risk at the participant level using available demographic, clinical, and lifestyle variables.



#### **Data Split Strategy**



A stratified splitting strategy will be used because the diabetes outcome is moderately imbalanced. Stratification helps preserve the proportion of diabetes and non-diabetes cases in both the training and test sets.



The evaluation workflow is:



1\. Split the data into a training set and a final holdout test set using an 80/20 stratified split.

2\. Use only the training set for model comparison and cross-validation.

3\. Perform 5-fold stratified cross-validation on the training set.

4\. Select the final model based on cross-validation performance.

5\. Evaluate the selected final model only once on the untouched final test set.



The final test set will not be used for feature selection, hyperparameter tuning, or repeated model comparison.



#### **Leakage Prevention**



Target leakage is a major concern in this project because the diabetes target was constructed using self-reported diagnosis and laboratory thresholds.



The following variables are excluded from the predictor set:

| Feature         |Reason|
|-|-|
|diabetes|Target variable.|
|DIQ010|Self-reported diabetes diagnosis; directly related to target.|
|doctor\_diabetes|Derived from DIQ010.|
|LBXGH|HbA1c value used to define the target.|
|LBXGLU|Fasting glucose value used to define the target.|
|SEQN|Participant ID; not a meaningful predictor.|





All preprocessing steps, including imputation, scaling, and one-hot encoding, will be performed inside scikit-learn pipelines. This ensures that preprocessing is fit only on the training data during cross-validation and prevents train-test contamination.



#### **Split Leakage Considerations**



The dataset is cross-sectional, with one row per adult NHANES participant. Therefore:



1. Temporal leakage is not a major concern because the data are not time-series observations.
2. Geographic leakage is not a major concern because the prediction task is not based on spatial units.
3. Group-level leakage is not a major concern because the data do not contain repeated observations for the same participant in the modeling dataset.



The main leakage concern for this project is feature leakage, which is addressed by removing variables used to construct the diabetes target.



#### **Evaluation Metrics**



Accuracy alone is not sufficient because the dataset is imbalanced and the project goal is diabetes screening. The primary metric is "Recall" for the diabetes class. Recall is emphasized because missing a participant with diabetes risk is more serious in a screening setting.



Secondary metrics include:



1. PR-AUC
2. ROC-AUC
3. Balanced accuracy
4. F1-score
5. Precision
6. Accuracy
7. Confusion matrix
8. Classification report



PR-AUC is especially useful because the positive diabetes class is less frequent than the non-diabetes class.



#### **Baseline Models**



The following baseline models will be evaluated:



|Model|Purpose|
|-|-|
|Dummy Classifier|Baseline reference model|
|Logistic Regression|Simple interpretable linear model|
|Decision Tree|Simple nonlinear model|



Three baseline models will be evaluated: Dummy Classifier, Logistic Regression, and Decision Tree. The Dummy Classifier provides a majority-class reference model. Logistic Regression serves as a simple and interpretable linear baseline. The Decision Tree provides a simple nonlinear baseline that can capture basic feature interactions.



More complex models such as Random Forest, Regularized Logistic Regression, Gradient Boosting and XGBoost will be evaluated later in the modeling experiments notebook and will be justified only if they improve the cross-validated KPIs.



#### **Feature Sets to Compare**



Two feature sets may be compared:



1\. Selected original features after leakage removal, low-information feature removal, and redundancy handling.

2\. Selected features plus engineered features.



Engineered features include clinically interpretable variables such as average blood pressure, pulse pressure, obesity indicator, high waist indicator, high blood pressure indicator, physical activity indicator, smoking history, diet behavior indicators, interaction terms, and metabolic risk score.



The final feature set will be chosen based on cross-validation performance and interpretability.



#### **Calibration Plan**



Because the model may be used for risk screening, predicted probabilities should be checked for reliability. Calibration analysis may be performed using calibration curves and calibrated classifiers. Calibration will be considered after baseline model evaluation.



#### **Robustness and Stress Testing**



The following stress tests are planned:



##### **Subgroup Performance**



Model performance will be evaluated across important demographic groups, including:



1. Gender
2. Race/ethnicity
3. Age groups
4. Education groups



This will help assess whether the model performs consistently across different participant subgroups.



##### **Shuffled-Target Leakage Check**



A shuffled-target experiment will be used as an adversarial leakage check. In this test, the target labels are randomly shuffled and the model is re-evaluated. If the model performs unusually well on shuffled labels, this may indicate leakage or overfitting.



##### **Outlier Sensitivity**



Outlier sensitivity will be reviewed for clinical variables such as BMI, waist circumference, and blood pressure. Extreme values will not be removed automatically because they may represent real diabetes-related risk signals.



#### **Result Storage**



Evaluation results will be saved in the "results" folder.



Planned output files include:



1. baseline\_cv\_results\_stratified\_DATE.csv
2. final\_test\_results\_stratified\_DATE.csv
3. shuffled\_target\_results\_stratified\_DATE.csv
4. subgroup\_results\_stratified\_DATE.csv



Each file will include the split strategy and date in the filename.



#### **Summary**



The evaluation framework is designed to provide a fair and leakage-free assessment of model performance. Stratified splitting and cross-validation will be used to handle class imbalance and reduce dependence on a single split. Recall and PR-AUC will be emphasized because the project is focused on early diabetes risk screening. The final test set will be kept untouched until the final model evaluation.



