# Week 06 — ML Pipeline Practical + Evaluation Report

## 📌 Overview

This project implements a complete classical Machine Learning pipeline for **loan-default prediction**.

The objective is to predict whether a loan applicant is likely to default based on:

* Credit score
* Applicant income
* Loan amount
* Employment type

The assessment focuses not only on model performance, but also on **proper preprocessing, prevention of data leakage, model comparison, error analysis, and probability calibration**.

---

## 🎯 Objectives

The main objectives of this assessment were to:

1. Build features from the provided loan dataset.
2. Split the data before handling missing values.
3. Perform train-only median imputation for missing credit scores.
4. Establish a majority-class baseline.
5. Train and evaluate three classical ML models.
6. Compare models using multiple classification metrics.
7. Perform error analysis on the final model.
8. Evaluate the calibration of predicted probabilities.
9. Document the methodology, findings, limitations, and model-selection decisions.

---

## 📊 Dataset

The dataset contains **1,200 loan applications** and the following variables:

| Feature            | Description                             |
| ------------------ | --------------------------------------- |
| `applicant_id`     | Unique identifier for each applicant    |
| `employment_type`  | Applicant's employment category         |
| `credit_score`     | Applicant's credit score                |
| `applicant_income` | Applicant's income                      |
| `loan_amount`      | Requested loan amount                   |
| `default`          | Target variable indicating loan default |

The `credit_score` feature contains **90 missing values**, representing incomplete credit-bureau information.

The dataset was generated using the exact specification provided in the assessment and was not modified.

---

## 🔄 Machine Learning Pipeline

The pipeline follows the required sequence:

```text
Raw Dataset
     │
     ▼
Feature Selection
     │
     ▼
One-Hot Encoding
     │
     ▼
Train/Test Split
     │
     ▼
Train-Only Median Imputation
     │
     ├───────────────┐
     ▼               ▼
Training Set      Test Set
     │               │
     └───────┬───────┘
             ▼
     Model Training
             │
             ▼
   Model Evaluation
             │
             ▼
     Final Model
             │
       ┌─────┴─────┐
       ▼           ▼
 Error Analysis  Calibration
```

### Important preprocessing decision

The dataset was split **before** imputation.

The median for `credit_score` was calculated using only the training set:

```text
Training-set median = 649.0000
```

The same value was then applied to both training and test data.

This prevents information from the test set from influencing the preprocessing step.

---

## 🧹 Feature Engineering

The following features were selected:

```python
[
    "credit_score",
    "applicant_income",
    "loan_amount",
    "employment_type"
]
```

`employment_type` was converted into numerical features using one-hot encoding with `drop_first=True`.

This allows the categorical employment information to be used by the classical ML models while avoiding redundant dummy variables.

---

## 🤖 Models

Four models were evaluated:

### 1. Baseline

A `DummyClassifier` using:

```python
strategy="most_frequent"
```

was used to establish a reference performance level.

### 2. Logistic Regression

A linear classification model used as a strong and interpretable baseline among the trained models.

### 3. Decision Tree

A tree-based model capable of learning non-linear decision boundaries.

### 4. Random Forest

An ensemble of decision trees designed to improve generalization and provide stronger classification performance.

All trained models used the **same train/test split and feature set**.

---

## 📈 Evaluation Metrics

The following metrics were calculated for every model:

* **Accuracy** — overall proportion of correct predictions.
* **Precision** — proportion of predicted defaults that were actually defaults.
* **Recall** — proportion of actual defaults correctly identified.
* **F1-score** — balance between precision and recall.
* **ROC-AUC** — ability to distinguish between the two classes across classification thresholds.

For loan-default prediction, recall is particularly important because a false negative means an actual defaulter was classified as a non-defaulter.

---

## 📊 Model Performance

| Model               | Accuracy | Precision | Recall | F1-score | ROC-AUC |
| ------------------- | -------: | --------: | -----: | -------: | ------: |
| Baseline            |   0.6000 |    0.6000 | 1.0000 |   0.7500 |  0.5000 |
| Logistic Regression |   0.7500 |    0.7692 | 0.8333 |   0.8000 |  0.8453 |
| Decision Tree       |   0.7500 |    0.7917 | 0.7917 |   0.7917 |  0.8205 |
| Random Forest       |   0.7708 |    0.7908 | 0.8403 |   0.8148 |  0.8198 |

### Key observation

All three trained models substantially outperform the baseline on overall predictive performance.

Random Forest achieved the highest:

* **Accuracy:** 0.7708
* **Recall:** 0.8403
* **F1-score:** 0.8148

Logistic Regression achieved the highest **ROC-AUC of 0.8453**.

Therefore, ROC-AUC alone was not used to select the final model.

---

## 🏆 Final Model

### Random Forest

Random Forest was selected as the final model because it provided the strongest overall classification performance for the selected objective.

It achieved:

```text
Accuracy  = 0.7708
Recall    = 0.8403
F1-score  = 0.8148
```

Recall is particularly relevant for this problem because failing to identify an actual defaulter can result in financial loss.

However, Logistic Regression achieved a higher ROC-AUC:

```text
Logistic Regression ROC-AUC = 0.8453
Random Forest ROC-AUC        = 0.8198
```

This trade-off was considered explicitly rather than assuming that Random Forest was best on every metric.

---

## 🔍 Error Analysis

After selecting Random Forest, the test-set predictions were compared with the actual default labels.

The misclassified cases were examined using:

* Credit score
* Applicant income
* Loan amount
* Employment type

The analysis indicates that the available features do not completely separate default and non-default applicants.

Both **false positives and false negatives** occur, showing that some applicants have similar characteristics despite having different outcomes.

This means that the model should not be treated as a perfect lending decision system. Additional financial information would be useful for reducing uncertainty in difficult cases.

---

## 📉 Calibration Analysis

A calibration curve was created using the Random Forest's predicted probabilities.

The calibration curve compares:

```text
Mean Predicted Probability
          vs.
Observed Fraction of Positives
```

The diagonal reference line represents perfect calibration.

Calibration is important because a model can achieve good classification performance while still producing probabilities that are not reliable enough for direct financial decision-making.

Therefore, the Random Forest probabilities should **not be directly used to set risk-based interest rates without additional calibration and validation**.

---

## ⚠️ Limitation

One important limitation is that the dataset does not include the applicant's **existing debt obligations or debt-to-income ratio (DTI)**.

The pipeline contains applicant income and loan amount, but it does not indicate how much debt the applicant already has.

For example, two applicants may have the same income and requested loan amount, but one may already have substantial outstanding debt.

Without this information, the model may underestimate the default risk of financially overextended applicants.

Therefore, additional financial-capacity features would be required before using this model for real-world lending decisions.

---

## 📁 Project Outputs

The assessment produces the following required files:

```text
├── model_metrics.json
├── chart_model_comparison.png
├── chart_calibration.png
├── evaluation_report.md
├── defense_answers.md
└── test_friday_sample.py
```

The completed Jupyter notebook contains the complete reproducible pipeline.

---

## 🧪 Self-Check

The provided structural self-check can be executed using:

```bash
pytest test_friday_sample.py
```

The sample test verifies that:

* All four model results exist in `model_metrics.json`.
* Required evaluation metrics are present.
* The imputation value and final model are recorded.
* Both required charts exist.
* The evaluation report exists.
* The defense answers exist.

The sample test checks submission structure only; it does not replace methodological or manual evaluation.

---

## 🔁 Reproducibility

The dataset generation uses:

```python
np.random.default_rng(seed=55)
```

and the train/test split uses:

```python
train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42,
    stratify=y
)
```

These fixed settings ensure that the dataset and evaluation split remain reproducible.

---

## 💡 Key Takeaways

This assessment demonstrates several important Machine Learning practices:

* Always split data before learning preprocessing statistics.
* Use only training data to calculate imputation values.
* Establish a baseline before evaluating complex models.
* Compare models using multiple metrics.
* Select a model according to the actual problem objective.
* Analyze errors instead of relying only on aggregate metrics.
* Do not assume classification probabilities are automatically calibrated.
* Consider real-world limitations before deploying a model.

---

## 👤 Assessment

**Week:** 06
**Topic:** ML Pipeline Practical + Evaluation Report + Defense
**Task:** Loan Default Prediction
**Final Model:** Random Forest
