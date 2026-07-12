###### **Data Inventory**



This project uses raw NHANES 2017-March 2020 Pre-Pandemic data from the CDC. Multiple NHANES modules were downloaded as XPT files and merged using the participant identifier SEQN.



|Data File|NHANES Module|Main Variables Used|Purpose|
|-|-|-|-|
|P\_DEMO.XPT|Demographics|RIDAGEYR, RIAGENDR, RIDRETH3, DMDEDUC2, INDFMPIR|Age, sex, race/ethnicity, education, and income|
|P\_BMX.XPT|Body Measures|BMXBMI, BMXWAIST|BMI and waist circumference|
|P\_BPXO.XPT|Blood Pressure Exam|BPXOSY1, BPXOSY2, BPXOSY3, BPXODI1, BPXODI2, BPXODI3|Systolic and diastolic blood pressure measurements|
|P\_GHB.XPT|Glycohemoglobin|LBXGH|HbA1c used for diabetes target construction|
|P\_GLU.XPT|Fasting Glucose|LBXGLU|Fasting glucose used for diabetes target construction|
|P\_DIQ.XPT|Diabetes Questionnaire|DIQ010|Self-reported doctor diagnosis of diabetes|
|P\_BPQ.XPT|Blood Pressure/Cholesterol Questionnaire|BPQ020, BPQ080|Blood pressure and cholesterol history|
|P\_PAQ.XPT|Physical Activity|PAQ650, PAQ655, PAD660, PAQ665, PAQ670, PAD675, PAD680|Physical activity and sedentary behavior|
|P\_SMQ.XPT|Smoking|SMQ020|Smoking status|
|P\_ALQ.XPT|Alcohol Use|ALQ111, ALQ121, ALQ130|Alcohol behavior|
|P\_DBQ.XPT|Diet Behavior|DBQ700, DBD895, DBD900, DBD905, DBD910|General diet quality, fast-food meals, and processed-food behavior|





###### **Data Access**



The raw NHANES XPT files were downloaded from the CDC NHANES website. 

https://wwwn.cdc.gov/nchs/nhanes

The raw files are stored locally in data/raw/ and are not uploaded to GitHub.



###### **Merge Key**



All NHANES files are merged using the participant identifier: SEQN



###### **Data Cleaning Notes**



The analysis is restricted to adult participants aged 20 years and older. Special missing-value codes such as refused, don't know, and not applicable are converted to missing values during data cleaning.



###### **Target Construction**



The diabetes target is constructed using self-reported diabetes diagnosis, HbA1c, and fasting glucose. Variables used to construct the target are excluded from the predictor set to avoid target leakage.



###### **Limitations**



Some variables contain missing values because not all NHANES participants answered every questionnaire or completed every examination or laboratory component. This may affect feature availability and subgroup coverage.

