# **Metabolic Syndrome Prediction**

**Executive Summary**
This project builds a machine learning classifier to predict metabolic syndrome risk based on demographic and clinical measurements. Using data from 2,401 individuals, we identified that triglycerides and blood glucose are the strongest predictors, followed by waist circumference and BMI. The Random Forest model achieved 88% accuracy on test data with balanced precision-recall performance.

--------------------------------------------------
## **Problem Statement**
Predict whether an individual has metabolic syndrome based on clinical and demographic risk factors. 
Early identification enables preventive interventions.
**Problem Type**: Binary Classification with Class Imbalance

## **Dataset**
- **Source:** NHANES (National Health and Nutrition Examination Survey)
- **Size:** 2,401 observations, 14 features
- **Target:** Metabolic Syndrome (Yes/No)
- **Class Distribution:** 66% No, 34% Yes

## **Features**
**Clinical Metrics:** Waist circumference, BMI, HDL cholesterol, triglycerides, blood glucose, uric acid, albuminuria
**Demographic:** Age, sex, race, income, marital status

## **Methodology**
1. Data cleaning & preprocessing (handle missing values, scaling)
2. Classification model training & evaluation
3. Performance metrics (accuracy, precision, recall, F1)
------------------------------------------
## **Results**
### **Exploratory Data Analysis (EDA)**
### **Target Variable Distribution:** Class Imbalance Assessment
   
Finding: Moderately imbalanced dataset (65.8% healthy, 34.2% disease).

Implication: A naive model predicting only "No MetSyn" would achieve 65.8% accuracy but miss all disease cases
Addressed by: Random Forest with class-weighted loss penalizing MetSyn misclassification

For Stakeholders: This imbalance is realistic. Real-world screening must prioritize sensitivity (catching disease) over specificity (avoiding false alarms).

<img width="789" height="490" alt="تنزيل (7)" src="https://github.com/user-attachments/assets/1e503666-f02f-4495-9bdf-d40660da6abf" />
-----------------------------------------------------------------------------

### **Key Exploratory Findings**

#### **Relationship: WaistCirc vs Triglycerides**

<img width="861" height="547" alt="تنزيل (9)" src="https://github.com/user-attachments/assets/708ab2c7-6806-4740-8550-892e91017c9a" />

Green cluster (No MetSyn): Waist < 100 cm, Triglycerides < 200 mg/dL
Red cluster (MetSyn): Waist > 100 cm, Triglycerides > 200 mg/dL
Pattern: Clear separation with minimal overlap → Strong predictive signal
Clinical Insight: Visceral obesity (abdominal fat) drives metabolic dysfunction and lipid dysregulation

#### **Age Distribution: Metabolic Syndrome Increases with Age**

<img width="1389" height="490" alt="تنزيل (10)" src="https://github.com/user-attachments/assets/d9458fee-d64b-401f-a3d5-fbcd9046f1d4" />

No MetSyn median age: 43 years
MetSyn median age: 57 years (14-year difference)
Pattern: MetSyn prevalence rises sharply after age 45
Clinical Insight: Metabolic dysfunction is age-dependent; insulin sensitivity declines naturally with aging

#### **Demographics: Sex Parity, Racial Disparities**

<img width="1389" height="490" alt="تنزيل (11)" src="https://github.com/user-attachments/assets/95bedd51-8248-491e-811e-182e31f28233" />

Sex: Equal prevalence in males (65.2% healthy) vs females (65.9% healthy) → No gender bias
Race: White and Black populations show highest prevalence; possible drivers: socioeconomic factors, healthcare access, genetic predisposition
Caveat: NHANES sampling may not represent all demographic groups equally

___________________________________________________
____________________________________________________

## **Model Development & Preprocessing**

### **Data Preprocessing Pipeline**

Raw Data (2,401 × 15)

   ↓
    
[Missing Value Imputation]

   ↓
   
[Feature Engineering: AgeGroup binning for Income imputation]

   ↓
   
[Drop ID columns (seqn)]

   ↓
   
[Train-Test Split: 75% train / 25% test]

   ↓
    
[Column Transformer:
  - Categorical: OneHotEncoder (Sex, Marital, Race)
  - Numeric: StandardScaler (Age, Income, WaistCirc, BMI, etc.)]
    
   ↓
    
Random Forest Classifier (preprocessed X, target y)

----------------------------------------
---------------------------------------

**Test Data Performance (Generalization)**

<img width="623" height="258" alt="تنزيل (12)" src="https://github.com/user-attachments/assets/ce158aad-376f-43a5-a777-8c458d24fcea" />

## **Breakdown:**

- 88% Overall Accuracy: Model correctly classifies 88 out of 100 test individuals
- MetSyn Detection (Recall): 78% of actual MetSyn cases are correctly identified → 22% missed diagnoses
- MetSyn Precision: 85% of predicted MetSyn cases are correct → 15% false alarms
- No MetSyn Recall: 93% of healthy individuals correctly identified → Few healthy people misclassified as disease

 ### Clinical Interpretation
 
Trade-off: Sensitivity vs Specificity
- High recall (78%) for MetSyn: Catches most disease cases, but misses some
- High recall (93%) for No MetSyn: Avoids unnecessary treatment of healthy people
- 
===========================================

## **Feature Importance Analysis**

### **Triglycerides** – The #1 Predictor

<img width="1089" height="690" alt="تنزيل (5)" src="https://github.com/user-attachments/assets/a6c42b77-1ebc-4a88-af14-f699baa77299" />

Finding: People with MetSyn have median triglycerides of ~220 mg/dL vs. ~110 mg/dL in healthy individuals.
Why It Matters:
Triglycerides >150 mg/dL is a diagnostic criterion for metabolic syndrome
Elevated triglycerides indicate dyslipidemia (abnormal lipid metabolism)
Directly related to cardiovascular risk and insulin resistance

For Decision-Makers: If triglycerides are high, metabolic syndrome risk is very high. This is a high-confidence screening metric. Medication (statins, fibrates) can reduce triglycerides and lower metabolic syndrome risk.

-------------------

### **Waist Circumference – Physical Indicator of Abdominal Obesity**

<img width="1089" height="690" alt="تنزيل (6)" src="https://github.com/user-attachments/assets/ad371b58-000a-4080-939c-edff0f49d7a9" />

Finding: People with MetSyn have a median waist circumference of ~105 cm vs. ~92 cm in healthy individuals.
Why It Matters:

Waist circumference >40" in men, >35" in women is a diagnostic criterion
Visceral fat (belly fat) is metabolically active and pro-inflammatory
Less expensive screening than blood tests; can be measured in any setting

For Decision-Makers: Waist circumference is an easy, cost-effective screening tool. Combined with triglyceride testing, provides a comprehensive risk assessment.

------------------------------------------
### **Age Confounding & Blood Glucose Relationship**

<img width="1189" height="690" alt="تنزيل (3)" src="https://github.com/user-attachments/assets/6bfa0399-9f4e-4f44-b12f-d317e60315a4" />

**Why It Matters:**
- A 35-year-old with MetSyn has higher glucose risk than a healthy 35-year-old
- Age and MetSyn status are **independent risk factors**, not confounded
- This validates BloodGlucose as a strong MetSyn predictor

**Key Finding:**
MetSyn patients have elevated glucose at every age—age doesn't explain the glucose elevation. Metabolic dysfunction drives the signal.

MetSyn isn't just an age problem. Young people with metabolic dysfunction need screening too.
That's the level of conciseness you need for your notebook. The original was verbose. 

________________________________________________________
________________________________________________________
## **Conclusions & Recommendations**

### Key Findings

1. Metabolic markers dominate: BloodGlucose and Triglycerides are the strongest predictors (23% of total importance), validating clinical diagnostic criteria.
2. Body composition matters: WaistCirc and BMI together account for 15% of importance; waist circumference superior to BMI.
3. Demographics are weak: Age, Income, Race, Sex collectively account for <6% of importance; model correctly learned that metabolic dysfunction is driven by biology, not demographics.
4. Reasonable generalization: 88% test accuracy with balanced precision-recall suggests useful screening performance.
5. Age is relevant but not deterministic: Age-dependent but younger people can have MetSyn if other risk factors present.
   
======================================================================
______________________________________________________________

### Actionable Next Steps
1. **Feature Reduction:** Drop low-importance features (Marital, UricAcid) 
   to simplify the model and reduce noise
   
2. **Model Improvement:** Try permutation-importance-based feature selection 
   or regularization (L1) to reduce overfitting
   
3. **Clinical Validation:** Present Triglycerides + WaistCirc model to clinicians 
   to validate that it aligns with practice
   
4. **Class Imbalance Handling:** If MetSyn class is minority, consider 
   SMOTE, class weights, or threshold tuning
=========================================================================
