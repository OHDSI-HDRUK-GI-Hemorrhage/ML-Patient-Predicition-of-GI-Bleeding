# ML Patient Prediction of GI Bleeding

A reproducible **R-based machine learning project** demonstrating the development, evaluation, and deployment of a predictive model for gastrointestinal (GI) bleeding risk stratification using the OHDSI OMOP Common Data Model and Eunomia synthetic data.

---

## Background

Gastrointestinal (GI) haemorrhage represents an important acute-care complication, particularly among patients receiving antithrombotic therapy. Early recognition and risk stratification can be clinically challenging yet critical for patient outcomes.

### Clinical Context

- **Risk Factors**: Anticoagulant and antiplatelet agents are associated with increased GI bleeding risk and can complicate management of patients who present with or are at risk of haemorrhage
- **Multifactorial Nature**: A systematic review and meta-analysis identified older age, concomitant aspirin or other antiplatelet therapy, renal disease, and other patient-level characteristics among factors associated with anticoagulant-related GI haemorrhage
- **Clinical Need**: Objective, data-driven risk stratification tools are needed to identify high-risk patients requiring enhanced monitoring or preventive interventions

### Study Rationale

This investigation applies patient-level prediction approaches that combine:
- **Demographic features** (age, sex, race)
- **Medication history** (aspirin, NSAID use, celecoxib)
- **Clinical conditions** (peptic ulcer disease, colitis, esophagitis, diverticular disease)

**Note**: This study is framed as a **risk-prediction investigation** rather than a pharmacovigilance intervention. The model estimates the likelihood of a GI haemorrhage outcome and does not, on its own, establish that a specific medication caused the event.

---

## Project Overview

### Objectives

1. **Develop** a machine learning model for patient-level GI bleeding risk prediction
2. **Evaluate** model performance using rigorous cross-validation and held-out test sets
3. **Compare** multiple modeling approaches (logistic regression, elastic net, random forest, GBM, SVM, XGBoost)
4. **Deploy** a production-ready prediction model with interpretable risk scores
5. **Demonstrate** reproducible, transparent machine learning workflows

### Key Features

✅ **Reproducible Pipeline**: Complete end-to-end workflow from data extraction to model deployment  
✅ **Multiple Algorithms**: Comparison of 6 distinct machine learning approaches  
✅ **Rigorous Evaluation**: 5-fold stratified cross-validation with AUROC and AUPRC metrics  
✅ **OMOP Standard**: Uses OHDSI Common Data Model for real-world applicability  
✅ **Transparent**: Publication-ready documentation and open-source code  

---

## Data & Methodology

### Data Source

**Eunomia Synthetic Dataset**
- OHDSI OMOP Common Data Model standard
- Realistic patient characteristics and clinical events
- Suitable for demonstration and validation without privacy concerns

### Study Population

- **N = 2,694** unique patients
- **Outcome**: GI bleeding (concept ID: 192671)
- **Prevalence**: ~12.5% (335 patients with GI bleeding events)
- **Train/Test Split**: 80/20 stratified by outcome

### Features

**Demographic Variables**
- Age (calculated at reference date: 2019-07-01)
- Sex (Male/Female)
- Race (White, Black/African American, Asian, Other, Unknown)

**Medication Exposure History**
- Prior aspirin use
- Prior naproxen use
- Prior celecoxib use

**Clinical Conditions**
- Prior peptic ulcer disease
- Prior ulcerative colitis
- Prior esophagitis
- Prior diverticular disease

### Target Variable

**Binary Outcome**: Presence of ≥1 GI bleeding event (ICD-10 equivalent concept 192671) during observation period

---

## Models Evaluated

### 1. **Logistic Regression**
Classical linear model for binary classification; serves as interpretable baseline

### 2. **Elastic Net**
Regularized regression combining L1 (LASSO) and L2 (Ridge) penalties; handles feature selection and multicollinearity

### 3. **Random Forest**
Ensemble of decision trees; captures non-linear relationships and interactions

### 4. **Gradient Boosting Machine (GBM)**
Sequential ensemble method; fits residuals iteratively for improved predictions

### 5. **Support Vector Machine (SVM)**
Radial basis function kernel; effective for high-dimensional classification

### 6. **XGBoost**
Advanced gradient boosting framework; optimized for speed, performance, and interpretability

---

## Results

### Cross-Validation Performance (5-Fold)

| Model | Mean AUPRC | SD AUPRC | Mean AUROC | SD AUROC |
|-------|-----------|----------|-----------|----------|
| **XGBoost** | **0.412** | 0.048 | **0.750** | 0.032 |
| GBM | 0.398 | 0.062 | 0.742 | 0.041 |
| Random Forest | 0.385 | 0.055 | 0.731 | 0.038 |
| SVM | 0.371 | 0.070 | 0.718 | 0.051 |
| Elastic Net | 0.352 | 0.068 | 0.698 | 0.058 |
| Logistic Regression | 0.341 | 0.075 | 0.684 | 0.062 |

**Primary Metric**: AUPRC (Area Under Precision-Recall Curve) — preferred for imbalanced data  
**Secondary Metric**: AUROC (Area Under Receiver Operating Characteristic Curve)

### Final Test-Set Performance (XGBoost)

**Threshold = 0.205** (optimized for sensitivity/specificity balance)

| Metric | Value |
|--------|-------|
| **Sensitivity** | 0.78 |
| **Specificity** | 0.68 |
| **Positive Predictive Value** | 0.22 |
| **Negative Predictive Value** | 0.96 |
| **F1-Score** | 0.34 |
| **Accuracy** | 0.69 |
| **AUROC** | 0.75 (95% CI: 0.71–0.79) |
| **AUPRC** | 0.41 |

**Clinical Interpretation**:
- Model identifies 78% of patients who experience GI bleeding (high sensitivity)
- 96% of low-risk predictions are correct (high NPV)
- Suitable for rule-out scenarios and identifying high-risk populations requiring enhanced monitoring

### Feature Importance (XGBoost)

Top predictive features ranked by importance:
1. Age
2. Prior esophagitis
3. Prior peptic ulcer disease
4. Sex
5. Prior diverticular disease
6. Race
7. Prior ulcerative colitis
8. Prior aspirin use
9. Prior naproxen use
10. Prior celecoxib use

---

## Technical Stack

### Languages & Frameworks
- **R 4.x**: Primary language for all analyses
- **XGBoost**: Final deployed model framework

### Key R Packages

**Data Management**
- `dplyr`, `dbplyr`, `tidyr`, `tibble`
- `DBI`, `DatabaseConnector` (OMOP CDM connectivity)
- `readr` (CSV I/O)

**OHDSI Packages**
- `Eunomia` (Synthetic OMOP data)
- `CommonDataModel` (CDM definitions)

**Machine Learning**
- `caret` (unified ML interface)
- `glmnet` (elastic net & logistic regression)
- `randomForest` (random forest)
- `gbm` (gradient boosting)
- `xgboost` (XGBoost)
- `kernlab` (SVM)

**Model Evaluation**
- `pROC` (AUROC calculation)
- `PRROC` (AUPRC calculation)
- `recipes` (feature preprocessing)

**Visualization & Reporting**
- `ggplot2` (statistical graphics)
- `knitr`, `kableExtra` (R Markdown reports)

---

## Project Structure
