##### **KPI Definitions**



This file defines the key performance indicators used to evaluate the early diabetes risk screening models.



##### **Project Goal**



The goal of this project is to identify individuals who may be at higher risk of diabetes using demographic, clinical, and lifestyle variables from NHANES data.



Because this is a screening problem, the model should prioritize identifying diabetes cases and reducing false negatives. Accuracy alone is not sufficient because the dataset is moderately imbalanced, with more non-diabetes cases than diabetes cases.



##### **Primary KPI**



###### **Recall for the Diabetes Class**



Recall measures how many actual diabetes cases are correctly identified by the model. Recall is important because missing a high-risk individual may delay lifestyle changes, medical monitoring, or further clinical consultation.



In this project, higher recall is better.



##### **Secondary KPIs**



###### **PR-AUC**



PR-AUC summarizes the tradeoff between precision and recall across different thresholds. PR-AUC is useful because the diabetes class is less common than the non-diabetes class.



Higher PR-AUC is better.



###### **ROC-AUC**



ROC-AUC measures how well the model separates diabetes and non-diabetes participants across classification thresholds.



Higher ROC-AUC is better.



###### **Balanced Accuracy**



Balanced accuracy accounts for performance on both diabetes and non-diabetes classes. This is useful when the outcome classes are imbalanced.



Higher balanced accuracy is better.



###### **F1-Score**



F1-score combines precision and recall into one metric. It is useful when both identifying diabetes cases and avoiding too many false positives are important.



Higher F1-score is better.



###### **Precision**



Precision measures how many participants predicted as diabetes cases are actually diabetes cases. Higher precision is better, but in this screening project, recall is more important than precision.



###### **Confusion Matrix**



The confusion matrix shows:



* True positives
* True negatives
* False positives
* False negatives



False negatives are especially important because they represent diabetes cases missed by the model.



###### **Model Comparison Plan**



The project will compare the following models:



* Dummy Classifier
* Logistic Regression
* Random Forest
* XGBoost



The Dummy Classifier is used as a baseline. Logistic Regression, Random Forest, and XGBoost should perform better than the Dummy Classifier to show that the dataset contains learnable signal.



###### **Improvement Direction**



* Higher recall is better.
* Higher PR-AUC is better.
* Higher ROC-AUC is better.
* Higher balanced accuracy is better.
* Higher F1-score is better.
* Lower false negative count is better.



