# Standalone Evaluation Report — Week 06 ML Pipeline

## 1. Overview

This report evaluates the regression and classification models developed during the Week 06 machine learning pipeline. The objective was to compare baseline models with Linear Regression, Decision Tree, Random Forest, and Logistic Regression models and select the best-performing models based on test-set evidence.

The evaluation also includes overfitting analysis, feature importance, error analysis, and probability calibration.

---

## 2. Dataset

The dataset contains **600 student records** with the following features:

- `student_id`
- `class_section`
- `study_hours_per_week`
- `sleep_hours_per_night`
- `attendance_pct`
- `exam_score`

### Regression Target

The regression task predicts:

`exam_score`

### Classification Target

For classification, students were divided into two classes:

- **1:** `exam_score >= 85`
- **0:** `exam_score < 85`

Categorical `class_section` was converted into numerical dummy variables using one-hot encoding.

All train/test splits used a fixed `random_state=42` to ensure reproducibility.

---

# 3. Regression Evaluation

The regression models were evaluated using:

- **RMSE:** Lower values are better.
- **R²:** Higher values are better.

## Regression Results

| Model | Test RMSE | Test R² |
|---|---:|---:|
| Dummy Mean Baseline | 10.2978 | -0.0096 |
| Linear Regression | **7.0584** | **0.5257** |
| Decision Tree | 10.2120 | 0.0072 |
| Decision Tree (`max_depth=3`) | 7.1031 | 0.5197 |
| Random Forest (200 trees) | 7.3518 | 0.4854 |

## Regression Model Selection

**Linear Regression** was selected as the best regression model.

It achieved the **lowest test RMSE (7.0584)** and the **highest test R² (0.5257)** among the evaluated models.

The depth-limited Decision Tree performed very similarly, with a test RMSE of **7.1031** and R² of **0.5197**, but it was still slightly worse than Linear Regression.

The Random Forest Regressor achieved a test RMSE of **7.3518** and R² of **0.4854**, so its additional complexity did not provide better predictive performance.

Therefore, the results show that a more complex model is not necessarily better for this dataset.

---

# 4. Decision Tree Overfitting Analysis

The unconstrained Decision Tree achieved:

- Train RMSE: **0.0000**
- Test RMSE: **10.2120**
- Test R²: **0.0072**

The train-test RMSE gap was approximately **10.2120**.

This large gap indicates severe **overfitting**. The tree essentially memorized the training observations but failed to generalize well to unseen test data.

After restricting the tree using `max_depth=3`:

- Train RMSE: **7.0019**
- Test RMSE: **7.1031**
- Test R²: **0.5197**

The train-test RMSE gap decreased to approximately **0.1012**.

This demonstrates that limiting tree complexity substantially improved generalization and reduced overfitting.

---

# 5. Random Forest Regression

The Random Forest Regressor used:

- `n_estimators=200`
- `random_state=42`

It achieved:

- Train RMSE: **2.8716**
- Test RMSE: **7.3518**
- Test R²: **0.4854**

## Feature Importance

| Feature | Importance |
|---|---:|
| `study_hours_per_week` | 0.6424 |
| `attendance_pct` | 0.1680 |
| `sleep_hours_per_night` | 0.1391 |
| `class_section_C` | 0.0340 |
| `class_section_B` | 0.0165 |

The most important feature was **`study_hours_per_week`**, with an importance of approximately **0.6424**.

This is consistent with the earlier correlation analysis, where study hours showed a moderate positive relationship with exam score.

However, the most important Random Forest feature was not the same as the feature with the largest absolute Linear Regression coefficient. Linear Regression gave the largest absolute coefficient to **`class_section_C`**.

This difference occurs because linear regression coefficients and tree-based feature importances measure feature contribution in different ways.

---

# 6. Classification Evaluation

The classification models were evaluated using:

- Accuracy
- Precision
- Recall
- F1-score

Higher values indicate better performance.

## Classification Results

| Model | Accuracy | Precision | Recall | F1 |
|---|---:|---:|---:|---:|
| Dummy Most-Frequent Baseline | 0.6500 | 0.6500 | 1.0000 | 0.7879 |
| Logistic Regression | **0.7667** | **0.8125** | 0.8333 | **0.8228** |
| Decision Tree | 0.7167 | 0.7973 | 0.7564 | 0.7763 |
| Random Forest (200 trees) | 0.7417 | 0.7831 | 0.8333 | 0.8075 |

## Classification Model Selection

**Logistic Regression** was selected as the best classification model.

It achieved the highest:

- Accuracy: **0.7667**
- Precision: **0.8125**
- F1-score: **0.8228**

Its recall of **0.8333** was also equal to the Random Forest Classifier.

Although Random Forest is more complex, it did not outperform Logistic Regression on the main evaluation metrics. Therefore, Logistic Regression was selected based on the actual test-set evidence rather than model complexity.

The Dummy Baseline achieved a recall of **1.0000**, but this does not indicate strong overall performance because it simply predicts the majority class.

---

# 7. Regression Error Analysis

The selected regression model, **Linear Regression**, was analyzed using its five largest absolute residuals.

The largest error occurred for student **576**, where:

- Actual score = **65.3**
- Predicted score = **84.1137**
- Absolute error = **18.8137**

The second-largest error occurred for student **516**:

- Actual score = **76.0**
- Predicted score = **94.2987**
- Absolute error = **18.2987**

Other large errors were observed for students **71, 507, and 258**, with absolute errors between approximately **16.63 and 17.50 points**.

Some large errors involved relatively low attendance or unusual combinations of study hours, sleep hours, and attendance.

However, there was no single feature pattern that explained all five errors. This suggests that some factors affecting exam performance are not fully represented by the available features.

Overall, Linear Regression performs well on average but can produce substantial errors for individual observations.

---

# 8. Classification Error Analysis

The selected classification model, **Logistic Regression**, was analyzed by examining its misclassified test observations.

The model made both:

- **False Negative** predictions
- **False Positive** predictions

Several false negatives had relatively low study hours or attendance. For example, student **220** had only **3.6 study hours per week**, while student **570** had **7.2 study hours** and **76.4% attendance**.

Several false positives had relatively high study hours or attendance. For example, student **218** had **13.0 study hours** and **94.0% attendance**, but the actual class was negative while the model predicted positive with a probability of approximately **0.9689**.

These observations indicate that study hours and attendance strongly influence classification, but they do not perfectly determine whether a student belongs to the positive class.

The misclassified observations therefore represent borderline or unusual combinations that are difficult to classify using the available features.

---

# 9. Probability Calibration

The Logistic Regression model's predicted probabilities were evaluated using a calibration curve.

A perfectly calibrated model would have predicted probabilities close to the actual fraction of positive observations in each probability bin.

The calibration results show that the model is reasonably well calibrated overall.

The strongest calibration was observed at the higher probability range. For example, the highest-probability bin had:

- Mean predicted probability: **0.9636**
- Actual fraction positive: **0.9630**
- Calibration gap: **0.0007**

The largest calibration gap occurred around the middle probability range:

- Mean predicted probability: **0.5500**
- Actual fraction positive: **0.7143**
- Calibration gap: **0.1643**

This indicates that the model **underestimates the probability of positive cases** in this region.

Therefore, the predicted probabilities are more trustworthy for high-confidence predictions, while probabilities around the middle range should be interpreted with more caution.

---

# 10. Overall Findings

The evaluation produced the following main findings:

1. **Linear Regression** was the best regression model based on the lowest test RMSE and highest test R².
2. The unconstrained Decision Tree showed clear **overfitting**, with a train RMSE of 0.0000 compared with a test RMSE of 10.2120.
3. Limiting the Decision Tree to `max_depth=3` greatly reduced overfitting and improved generalization.
4. **Study hours per week** was the most important feature in the Random Forest Regressor.
5. **Logistic Regression** was the best classification model, achieving the highest accuracy and F1-score.
6. Random Forest Classification achieved the same recall as Logistic Regression but had lower accuracy, precision, and F1-score.
7. Error analysis showed that some students have unusual feature combinations that are difficult to predict accurately.
8. Logistic Regression probabilities were generally well calibrated at high-confidence ranges but showed weaker calibration around the middle probability range.

---

# 11. Final Model Selection

| Task | Selected Model | Main Evidence |
|---|---|---|
| Regression | **Linear Regression** | Lowest RMSE and highest R² |
| Classification | **Logistic Regression** | Highest accuracy and F1-score |

The final model choices were made using **test-set performance and error analysis**, rather than selecting the most complex model.

For this dataset, the simpler Linear Regression and Logistic Regression models provided the strongest overall results and better supported the principle of choosing a model based on evidence.

---

# 12. Reproducibility

The complete pipeline uses fixed random seeds and a consistent train/test split to make the results reproducible.

Key settings include:

```python
RANDOM_SEED = 42

train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=RANDOM_SEED
)# Standalone Evaluation Report — Week 06 ML Pipeline

## 1. Overview

This report evaluates the regression and classification models developed during the Week 06 machine learning pipeline. The objective was to compare baseline models with Linear Regression, Decision Tree, Random Forest, and Logistic Regression models and select the best-performing models based on test-set evidence.

The evaluation also includes overfitting analysis, feature importance, error analysis, and probability calibration.

---

## 2. Dataset

The dataset contains **600 student records** with the following features:

- `student_id`
- `class_section`
- `study_hours_per_week`
- `sleep_hours_per_night`
- `attendance_pct`
- `exam_score`

### Regression Target

The regression task predicts:

`exam_score`

### Classification Target

For classification, students were divided into two classes:

- **1:** `exam_score >= 85`
- **0:** `exam_score < 85`

Categorical `class_section` was converted into numerical dummy variables using one-hot encoding.

All train/test splits used a fixed `random_state=42` to ensure reproducibility.

---

# 3. Regression Evaluation

The regression models were evaluated using:

- **RMSE:** Lower values are better.
- **R²:** Higher values are better.

## Regression Results

| Model | Test RMSE | Test R² |
|---|---:|---:|
| Dummy Mean Baseline | 10.2978 | -0.0096 |
| Linear Regression | **7.0584** | **0.5257** |
| Decision Tree | 10.2120 | 0.0072 |
| Decision Tree (`max_depth=3`) | 7.1031 | 0.5197 |
| Random Forest (200 trees) | 7.3518 | 0.4854 |

## Regression Model Selection

**Linear Regression** was selected as the best regression model.

It achieved the **lowest test RMSE (7.0584)** and the **highest test R² (0.5257)** among the evaluated models.

The depth-limited Decision Tree performed very similarly, with a test RMSE of **7.1031** and R² of **0.5197**, but it was still slightly worse than Linear Regression.

The Random Forest Regressor achieved a test RMSE of **7.3518** and R² of **0.4854**, so its additional complexity did not provide better predictive performance.

Therefore, the results show that a more complex model is not necessarily better for this dataset.

---

# 4. Decision Tree Overfitting Analysis

The unconstrained Decision Tree achieved:

- Train RMSE: **0.0000**
- Test RMSE: **10.2120**
- Test R²: **0.0072**

The train-test RMSE gap was approximately **10.2120**.

This large gap indicates severe **overfitting**. The tree essentially memorized the training observations but failed to generalize well to unseen test data.

After restricting the tree using `max_depth=3`:

- Train RMSE: **7.0019**
- Test RMSE: **7.1031**
- Test R²: **0.5197**

The train-test RMSE gap decreased to approximately **0.1012**.

This demonstrates that limiting tree complexity substantially improved generalization and reduced overfitting.

---

# 5. Random Forest Regression

The Random Forest Regressor used:

- `n_estimators=200`
- `random_state=42`

It achieved:

- Train RMSE: **2.8716**
- Test RMSE: **7.3518**
- Test R²: **0.4854**

## Feature Importance

| Feature | Importance |
|---|---:|
| `study_hours_per_week` | 0.6424 |
| `attendance_pct` | 0.1680 |
| `sleep_hours_per_night` | 0.1391 |
| `class_section_C` | 0.0340 |
| `class_section_B` | 0.0165 |

The most important feature was **`study_hours_per_week`**, with an importance of approximately **0.6424**.

This is consistent with the earlier correlation analysis, where study hours showed a moderate positive relationship with exam score.

However, the most important Random Forest feature was not the same as the feature with the largest absolute Linear Regression coefficient. Linear Regression gave the largest absolute coefficient to **`class_section_C`**.

This difference occurs because linear regression coefficients and tree-based feature importances measure feature contribution in different ways.

---

# 6. Classification Evaluation

The classification models were evaluated using:

- Accuracy
- Precision
- Recall
- F1-score

Higher values indicate better performance.

## Classification Results

| Model | Accuracy | Precision | Recall | F1 |
|---|---:|---:|---:|---:|
| Dummy Most-Frequent Baseline | 0.6500 | 0.6500 | 1.0000 | 0.7879 |
| Logistic Regression | **0.7667** | **0.8125** | 0.8333 | **0.8228** |
| Decision Tree | 0.7167 | 0.7973 | 0.7564 | 0.7763 |
| Random Forest (200 trees) | 0.7417 | 0.7831 | 0.8333 | 0.8075 |

## Classification Model Selection

**Logistic Regression** was selected as the best classification model.

It achieved the highest:

- Accuracy: **0.7667**
- Precision: **0.8125**
- F1-score: **0.8228**

Its recall of **0.8333** was also equal to the Random Forest Classifier.

Although Random Forest is more complex, it did not outperform Logistic Regression on the main evaluation metrics. Therefore, Logistic Regression was selected based on the actual test-set evidence rather than model complexity.

The Dummy Baseline achieved a recall of **1.0000**, but this does not indicate strong overall performance because it simply predicts the majority class.

---

# 7. Regression Error Analysis

The selected regression model, **Linear Regression**, was analyzed using its five largest absolute residuals.

The largest error occurred for student **576**, where:

- Actual score = **65.3**
- Predicted score = **84.1137**
- Absolute error = **18.8137**

The second-largest error occurred for student **516**:

- Actual score = **76.0**
- Predicted score = **94.2987**
- Absolute error = **18.2987**

Other large errors were observed for students **71, 507, and 258**, with absolute errors between approximately **16.63 and 17.50 points**.

Some large errors involved relatively low attendance or unusual combinations of study hours, sleep hours, and attendance.

However, there was no single feature pattern that explained all five errors. This suggests that some factors affecting exam performance are not fully represented by the available features.

Overall, Linear Regression performs well on average but can produce substantial errors for individual observations.

---

# 8. Classification Error Analysis

The selected classification model, **Logistic Regression**, was analyzed by examining its misclassified test observations.

The model made both:

- **False Negative** predictions
- **False Positive** predictions

Several false negatives had relatively low study hours or attendance. For example, student **220** had only **3.6 study hours per week**, while student **570** had **7.2 study hours** and **76.4% attendance**.

Several false positives had relatively high study hours or attendance. For example, student **218** had **13.0 study hours** and **94.0% attendance**, but the actual class was negative while the model predicted positive with a probability of approximately **0.9689**.

These observations indicate that study hours and attendance strongly influence classification, but they do not perfectly determine whether a student belongs to the positive class.

The misclassified observations therefore represent borderline or unusual combinations that are difficult to classify using the available features.

---

# 9. Probability Calibration

The Logistic Regression model's predicted probabilities were evaluated using a calibration curve.

A perfectly calibrated model would have predicted probabilities close to the actual fraction of positive observations in each probability bin.

The calibration results show that the model is reasonably well calibrated overall.

The strongest calibration was observed at the higher probability range. For example, the highest-probability bin had:

- Mean predicted probability: **0.9636**
- Actual fraction positive: **0.9630**
- Calibration gap: **0.0007**

The largest calibration gap occurred around the middle probability range:

- Mean predicted probability: **0.5500**
- Actual fraction positive: **0.7143**
- Calibration gap: **0.1643**

This indicates that the model **underestimates the probability of positive cases** in this region.

Therefore, the predicted probabilities are more trustworthy for high-confidence predictions, while probabilities around the middle range should be interpreted with more caution.

---

# 10. Overall Findings

The evaluation produced the following main findings:

1. **Linear Regression** was the best regression model based on the lowest test RMSE and highest test R².
2. The unconstrained Decision Tree showed clear **overfitting**, with a train RMSE of 0.0000 compared with a test RMSE of 10.2120.
3. Limiting the Decision Tree to `max_depth=3` greatly reduced overfitting and improved generalization.
4. **Study hours per week** was the most important feature in the Random Forest Regressor.
5. **Logistic Regression** was the best classification model, achieving the highest accuracy and F1-score.
6. Random Forest Classification achieved the same recall as Logistic Regression but had lower accuracy, precision, and F1-score.
7. Error analysis showed that some students have unusual feature combinations that are difficult to predict accurately.
8. Logistic Regression probabilities were generally well calibrated at high-confidence ranges but showed weaker calibration around the middle probability range.

---

# 11. Final Model Selection

| Task | Selected Model | Main Evidence |
|---|---|---|
| Regression | **Linear Regression** | Lowest RMSE and highest R² |
| Classification | **Logistic Regression** | Highest accuracy and F1-score |

The final model choices were made using **test-set performance and error analysis**, rather than selecting the most complex model.

For this dataset, the simpler Linear Regression and Logistic Regression models provided the strongest overall results and better supported the principle of choosing a model based on evidence.

---

# 12. Reproducibility

The complete pipeline uses fixed random seeds and a consistent train/test split to make the results reproducible.

Key settings include:

```python
RANDOM_SEED = 42

train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=RANDOM_SEED
)