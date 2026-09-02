# Week 6 — Day 1: Machine Learning Foundations

## Overview

This project introduces the basic workflow of **supervised machine learning** using a synthetic student performance dataset.

The analysis focuses on two machine learning tasks:

* **Regression:** Predict students' exam scores.
* **Classification:** Predict whether a student achieves a distinction (`exam_score >= 85`).

The notebook covers:

* Data preparation
* Feature engineering
* Train/test splitting
* Baseline models
* Machine learning models
* Evaluation metrics
* Model comparison
* Confusion matrix analysis
* Visual analysis

---

## Dataset

The dataset contains **600 student records** with the following features:

| Feature                 | Description                          |
| ----------------------- | ------------------------------------ |
| `student_id`            | Unique identifier for each student   |
| `class_section`         | Student's class section (A, B, or C) |
| `study_hours_per_week`  | Weekly study hours                   |
| `sleep_hours_per_night` | Average nightly sleep hours          |
| `attendance_pct`        | Attendance percentage                |
| `exam_score`            | Final exam score                     |

The dataset was **synthetically generated using a fixed random seed** to ensure reproducibility.

---

## Data Quality

Initial data quality checks confirmed that:

* The dataset contains **600 rows and 6 columns**.
* There are **no missing values**.
* There are **no duplicate records**.
* Numerical features were examined using descriptive statistics.
* Relationships between variables were explored using Pearson correlation.

---

## Exploratory Findings

The exploratory analysis produced the following observations:

* The average exam score is approximately **88.61**.
* The average weekly study time is approximately **10.09 hours**.
* The average sleep duration is approximately **7.05 hours per night**.
* Study hours have a **moderate positive correlation** with exam score (`r = 0.6895`).
* Sleep hours show **almost no linear correlation** with exam score (`r = -0.0217`).
* Attendance has a **weak positive correlation** with exam score (`r = 0.1066`).
* Section C has the highest average exam score at approximately **91.36**.

These findings suggest that **study time is more strongly associated with exam performance** than sleep duration or attendance in this synthetic dataset.

---

## Feature Engineering

The categorical `class_section` feature was converted into numerical dummy variables using **one-hot encoding**.

`class_section_A` was used as the reference category, while the following binary features were included:

* `class_section_B`
* `class_section_C`

The final feature matrix contains **5 input features**:

```text
study_hours_per_week
sleep_hours_per_night
attendance_pct
class_section_B
class_section_C
```

---

# Regression Task

## Objective

The objective of the regression task is to predict the continuous `exam_score` using student-related features.

---

## Train/Test Split

The dataset was divided into training and testing sets:

* **Training set:** 480 students (80%)
* **Test set:** 120 students (20%)

A fixed random state was used to ensure that the split is **reproducible**.

---

## Baseline Model

A `DummyRegressor` using the **mean strategy** was used as the regression baseline.

| Metric | Baseline |
| ------ | -------: |
| RMSE   |  10.2978 |
| R²     |  -0.0096 |

The baseline provides a simple reference point for determining whether the trained regression model provides meaningful predictive value.

---

## Linear Regression

A **Linear Regression** model was trained using the engineered features.

| Metric | Linear Regression |
| ------ | ----------------: |
| RMSE   |            7.0584 |
| R²     |            0.5257 |

### Comparison with Baseline

Compared with the baseline:

* RMSE improved by approximately **3.2394 points**.
* R² improved by approximately **0.5353**.

This indicates that **Linear Regression provides substantially better predictions** than simply predicting the average exam score for every student.

An R² of **0.5257** means that the model explains approximately **52.57% of the variation** in exam scores on the test data.

---

## Regression Coefficients

The learned regression coefficients were:

| Feature                 | Coefficient |
| ----------------------- | ----------: |
| `class_section_C`       |      3.1304 |
| `study_hours_per_week`  |      2.0330 |
| `class_section_B`       |      0.1248 |
| `attendance_pct`        |      0.0887 |
| `sleep_hours_per_night` |     -0.0107 |

The largest absolute coefficient is associated with `class_section_C`.

### Interpretation

The coefficient for `study_hours_per_week` indicates that, **holding the other features constant**, an additional hour of weekly study is associated with an increase of approximately **2.03 points** in the predicted exam score.

The coefficient for `class_section_C` is approximately **3.13**, meaning that, relative to the reference category (Section A), being in Section C is associated with an approximately **3.13-point higher predicted exam score**, while keeping the other features constant.

The coefficient for sleep hours is very close to zero, indicating that sleep duration has **little linear contribution** to the prediction in this dataset.

---

# Classification Task

## Objective

The classification task predicts whether a student achieves a **Distinction** based on their exam score.

The target variable was defined as:

```text
Distinction = 1  → exam_score >= 85
Distinction = 0  → exam_score < 85
```

---

## Class Distribution

Out of the 600 students:

* **392 students (65.33%)** achieved distinction.
* **208 students (34.67%)** did not achieve distinction.

This shows that the dataset has a **moderate class imbalance**, with Distinction being the majority class.

---

## Train/Test Split

A **stratified train/test split** was used to preserve a similar distinction ratio in both datasets.

* **Training set:** 480 students
* **Test set:** 120 students

Stratification helps ensure that both the training and testing sets contain a representative proportion of Distinction and No Distinction students.

---

# Classification Baseline

A `DummyClassifier` using the **most frequent class** strategy was used as the classification baseline.

| Metric    | Baseline |
| --------- | -------: |
| Accuracy  |   0.6500 |
| Precision |   0.6500 |
| Recall    |   1.0000 |
| F1-score  |   0.7879 |

The baseline predicts every student as the majority class.

Although this produces a relatively high accuracy, it does not represent a useful classification strategy because it does not distinguish between Distinction and No Distinction students.

Therefore, the baseline is primarily used as a **reference point** for evaluating the Logistic Regression model.

---

# Logistic Regression

A **Logistic Regression** model was trained to classify students into:

* Distinction
* No Distinction

The model achieved the following results:

| Metric    | Logistic Regression |
| --------- | ------------------: |
| Accuracy  |              0.7667 |
| Precision |              0.8125 |
| Recall    |              0.8333 |
| F1-score  |              0.8228 |

---

## Comparison with Baseline

Compared with the baseline:

* Accuracy improved by **0.1167**.
* Precision improved by **0.1625**.
* F1-score improved by approximately **0.0349**.
* Recall decreased because the baseline predicted every student as Distinction.

Overall, Logistic Regression provides a **more meaningful and balanced classification model** than the majority-class baseline.

The improvement in accuracy and precision indicates that the model is able to use the student features to make more informative predictions rather than simply selecting the majority class.

---

# Confusion Matrix

The Logistic Regression confusion matrix provides a detailed view of the model's correct and incorrect predictions.

The model produced:

* **27** No Distinction students correctly classified.
* **65** Distinction students correctly classified.
* **15** No Distinction students incorrectly classified as Distinction.
* **13** Distinction students incorrectly classified as No Distinction.

The results indicate that the model correctly identifies **Distinction students more effectively** than No Distinction students.

The confusion matrix is useful because accuracy alone does not show which classes are being predicted correctly or incorrectly.

---

# Visualizations

The notebook includes visualizations to support model evaluation and interpretation.

## Regression Visualization

### Actual vs. Predicted Exam Scores

The **Actual vs. Predicted Exam Scores** scatter plot compares the true exam scores with the scores predicted by the Linear Regression model.

This visualization helps assess how closely the predictions follow the actual values.

Predictions that are closer to the ideal diagonal relationship indicate better model performance.

---

## Classification Visualization

### Logistic Regression Confusion Matrix

The **Logistic Regression Confusion Matrix** visualizes the number of correct and incorrect predictions for:

* Distinction
* No Distinction

It provides an intuitive way to understand the classification performance of the model.

---

# Key Findings

1. **Study hours are the strongest numerical predictor** of exam score among the continuous features.

2. **Sleep hours show almost no linear relationship** with exam score in this synthetic dataset.

3. **Linear Regression substantially outperforms the regression baseline**, achieving an R² of **0.5257** and an RMSE of **7.0584**.

4. **Logistic Regression outperforms the classification baseline** in accuracy, precision, and F1-score.

5. The classification model performs better at identifying **Distinction students** than **No Distinction students**.

6. The results demonstrate the importance of using **baseline models** before evaluating more advanced machine learning models.

7. Feature engineering, particularly **one-hot encoding**, allows categorical information such as class section to be incorporated into machine learning models.

---

# Tools & Libraries

The project uses the following tools and libraries:

* **Python**
* **NumPy**
* **Pandas**
* **Matplotlib**
* **Scikit-learn**
* **Jupyter Notebook**

---

# Project Structure

```text
Day3
│
├── Week6_Day3_First_ML_Pipeline.ipynb
├── README.md
├── requirements.txt
├── .gitignore
│
└── Figures/
    ├── linear_actual_vs_predicted.png
    └── logistic_confusion_matrix.png
```

---

# Reproducibility

The dataset and machine learning experiments use fixed random states where applicable.

This ensures that:

* The synthetic dataset can be regenerated consistently.
* The train/test split remains reproducible.
* Model results can be compared reliably.

---

# Conclusion

This project demonstrates a complete introductory **supervised machine learning workflow**, starting from data preparation and feature engineering and progressing through model training, evaluation, comparison, and visualization.

Two different machine learning tasks were explored:

* **Regression** for predicting continuous exam scores.
* **Classification** for predicting whether a student achieves distinction.

The Linear Regression model significantly outperformed the regression baseline, while Logistic Regression also provided better and more informative predictions than the majority-class classification baseline.

The analysis demonstrates the importance of:

* Preparing data correctly
* Encoding categorical features
* Creating appropriate train/test splits
* Establishing baseline models
* Selecting suitable evaluation metrics
* Interpreting model coefficients
* Using visualizations to understand model behavior

Overall, the project provides a practical foundation for understanding how machine learning models are developed, evaluated, and interpreted using Python and Scikit-learn.
