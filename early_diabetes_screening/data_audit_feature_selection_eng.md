#### **Data Audit, Feature Selection and Engineering Summary**



##### **Project Context**



This project uses NHANES data to build an early diabetes risk screening model. The target variable is a binary diabetes indicator created using doctor-diagnosed diabetes status, HbA1c, and fasting glucose information.



The goal of the data audit was to understand the structure of the dataset, identify missingness and possible data-quality issues, remove leakage or low-information variables, and create clinically meaningful engineered features for modeling.





##### **Data Audit Summary**



###### **Dataset Size**



After cleaning and applying the adult age filter, the final cleaned dataset contained:



* 9,232 adult participants
* 7,412 non-diabetes participants
* 1,820 diabetes participants



This shows that the dataset is imbalanced, with many more non-diabetes participants than diabetes participants.



###### **Target Distribution**



The diabetes target distribution was:



|Diabetes Status|Count|
|-|-|
|Non-diabetes|7,412|
|Diabetes|1,820|



The diabetes prevalence in the cleaned dataset was approximately 19.7%.



Because of this class imbalance, accuracy alone is not an appropriate primary evaluation metric. Recall, PR-AUC, ROC-AUC, balanced accuracy, F1-score, and precision will be used to evaluate models more carefully.



##### **Key Distributional Facts**



###### **Age**



Age was an important predictor in the dataset. The cleaned dataset included adult participants aged 20 years and older.



Summary statistics for age:



|Statistic| Value|
|-|-|
|Mean|51.14|
|Standard deviation|17.69|
|Minimum|20|
|Median |52|
|Maximum |80|



The diabetes rate increased across older age groups. This supports the use of age and age-related interaction features in the modeling stage.



###### **Gender**



The dataset included both gender groups:



|Gender code |Count |
|-|-|
|1.0|4,479|
|2.0|4,753|



Diabetes cases were present in both groups, so gender was retained as a demographic predictor.



###### **Race/Ethnicity**



Race/ethnicity was represented using NHANES race/ethnicity codes. The distribution showed that all major race/ethnicity groups had both diabetes and non-diabetes cases. Therefore, race/ethnicity was retained as a demographic predictor for subgroup analysis and model evaluation.



###### **Body-Size Measures**



BMI and waist circumference were important clinical variables. Exploratory plots showed a strong positive relationship between BMI and waist circumference. Diabetes cases appeared more frequently among participants with higher BMI and higher waist circumference.



These findings supported the use of BMI, waist circumference, and body-size interaction features.



###### **Blood Pressure**



Blood-pressure variables were also reviewed. Instead of using only one blood-pressure reading, average systolic and average diastolic blood pressure features were created using available repeated measurements. This helped summarize blood-pressure information more clearly.





##### **Missingness Summary**



Missingness was reviewed before modeling. Some variables had noticeable missing values, especially lifestyle, alcohol, diet, waist, and blood-pressure-related variables.



Important variables with missingness included:



* Alcohol-related variables such as `ALQ121` and `ALQ130`
* Diet-related variables such as `DBD900`
* Blood-pressure-related variables
* Waist circumference
* Income-to-poverty ratio



Missing values were handled inside the modeling pipeline using imputation. Numeric features were imputed using the median, and categorical features were imputed using the most frequent category. For some important predictors, missingness indicator features were also added. These indicators preserve information about whether a value was originally missing before imputation.



##### **Correlation and Relationship Summary**



Several important relationships were identified during EDA.



###### **BMI and Waist Circumference**



BMI and waist circumference showed a strong positive relationship. Participants with higher BMI generally also had higher waist circumference. This relationship supports the use of both body-size variables, while also motivating interaction features such as age-BMI and age-waist interactions.



###### **Age and Diabetes Risk**



Diabetes rate increased across older age groups. This supports the use of age as an important predictor.



###### **Body Size and Diabetes Status**



Diabetes cases appeared more frequently among participants with higher BMI and higher waist circumference. This supports the use of BMI, waist circumference, obesity indicators, high-waist indicators, and metabolic risk features.



##### **Dropped Features and Justification**



Several features were removed before modeling for leakage, redundancy, missingness, or low relevance.



###### **Leakage Features**



The following variables were removed because they directly define or strongly reveal the target variable:

|Dropped Feature|Reason|
|-|-|
|diabetes|Target variable, not a predictor|
|doctor\_diabetes|Used to construct the target|
|DIQ010|Doctor diagnosis variable used to construct the target|
|LBXGH|HbA1c value used to construct the target|
|LBXGLU|Fasting glucose value used to construct the target|



These variables were removed to avoid target leakage. Keeping them would make the prediction task unrealistic because the model would have direct access to diagnostic information.



###### **Identifier Feature**



|Dropped Feature|Reason|
|-|-|
|SEQN|Participant identifier; not clinically meaningful for prediction|



SEQN was removed because it is only an ID number and does not contain useful predictive information.



###### **Redundant Blood-Pressure Features**



The original repeated blood-pressure readings were summarized into average blood-pressure features.

|Original Features|Replacement Feature|
|-|-|
|BPXOSY1, BPXOSY2, BPXOSY3|avg\_systolic\_bp|
|BPXODI1, BPXODI2, BPXODI3|avg\_diastolic\_bp|



The repeated readings were not all used separately in the final modeling dataset because average systolic and average diastolic blood pressure provide cleaner summary measures.



###### **High-Missingness or Low-Information Physical Activity Features**



Some physical activity duration variables were removed because they had high missingness and were less reliable for modeling.

Dropped Feature    Reason

\---------------    ------------------

PAD660             High missingness 

PAD675             High missingness 

PAQ655             High missingness 

PAQ670             High missingness



Instead of relying on these detailed duration variables, simpler physical activity indicators were retained or engineered.



##### **Engineered Features and Rationale**



Several engineered features were created using domain knowledge related to diabetes risk.



###### **Blood-Pressure Features**



Engineered Feature      Rationale

\-------------------     -------------------------------------------------------

avg\_systolic\_bp         Average systolic blood pressure across available readings

avg\_diastolic\_bp        Average diastolic blood pressure across available readings

pulse\_pressure          Difference between systolic and diastolic blood pressure

high\_bp\_exam            Indicator for elevated measured blood pressure



These features summarize blood-pressure information and help capture cardiovascular risk factors related to diabetes.



###### **Body-Size Features**



Engineered Feature      Rationale

\-------------------     -------------------------------------------------------

obese                   Indicator for BMI-based obesity

bmi\_category            Categorizes BMI into clinically interpretable groups

high\_waist              Indicator for high waist circumference

age\_bmi                 Interaction between age and BMI

age\_waist               Interaction between age and waist circumference



These features were created because obesity, central adiposity, and age-related body-size risk are clinically meaningful for diabetes screening.



###### **Lifestyle and Health Behavior Features**



Engineered Feature          Rationale

\-------------------         ----------------------------------------------------------

physically\_active           Captures whether the participant reported physical activity

smoking\_history             Captures smoking-related risk information

ever\_regular\_alcohol        Captures alcohol-use history

fair\_poor\_diet              Captures lower self-reported diet quality

fair\_or\_poor\_diet           Captures lower self-reported diet quality

frequent\_fast\_food          Captures frequent fast-food consumption behavior



These features were included because lifestyle and health behavior factors may contribute to diabetes risk.



###### **Interaction and Risk Score Features**



Engineered Feature      Rationale

\-------------------     -------------------------------------------------------------------------------

age\_systolic\_bp         Interaction between age and systolic blood pressure

metabolic\_risk\_score    Simple combined score based on obesity, waist, blood pressure, smoking, and diet-related indicators



The metabolic risk score was created to summarize multiple diabetes-related risk indicators into one interpretable feature.



###### **Missingness Indicator Features**



Missingness indicators were added for selected important predictors.



Engineered Feature          Rationale

\-------------------         ------------------------------------------------------------

avg\_systolic\_bp\_missing     Indicates whether systolic BP information was missing

avg\_diastolic\_bp\_missing    Indicates whether diastolic BP information was missing

BMXWAIST\_missing            Indicates whether waist circumference was missing

ALQ121\_missing              Indicates whether alcohol frequency information was missing

ALQ130\_missing              Indicates whether alcohol quantity information was missing



These features were added because missingness itself may contain useful information. The model can learn whether missing values are associated with different risk patterns.



##### **Summary**



The data audit showed that the dataset is imbalanced, with a lower number of diabetes cases than non-diabetes cases. EDA showed important relationships between diabetes status and age, BMI, waist circumference, and blood-pressure-related variables.



Features were removed when they caused target leakage, were only identifiers, had high missingness, or were redundant with engineered summary features. New engineered features were created using diabetes-related domain knowledge, including body-size indicators, blood-pressure summaries, lifestyle indicators, interaction terms, a metabolic risk score, and missingness indicators.



These steps helped create a cleaner and more meaningful feature set for the modeling stage.

