# Week 06 — ML Pipeline Evaluation Report

## 1. Task Overview

The objective of this assessment was to build and evaluate a complete machine learning pipeline for predicting loan default.

The dataset contains applicant information including credit score, applicant income, loan amount, employment type, and the target variable `default`.

The pipeline included:

* Train/test splitting with stratification
* One-hot encoding of the categorical feature
* Train-only median imputation for missing credit scores
* A majority-class baseline
* Logistic Regression
* Decision Tree
* Random Forest
* Multiple classification evaluation metrics
* Error analysis
* Calibration analysis

The dataset was split into training and testing sets using an 80/20 stratified split. Missing `credit_score` values were imputed using the median calculated from the training set only.

---

## 2. Baseline Model

A `DummyClassifier` using the `most_frequent` strategy was used as the baseline.

The baseline achieved:

| Metric    |  Score |
| --------- | -----: |
| Accuracy  | 0.6000 |
| Precision | 0.6000 |
| Recall    | 1.0000 |
| F1-score  | 0.7500 |
| ROC-AUC   | 0.5000 |

Although the baseline achieved a recall of 1.0000, this does not indicate useful predictive performance. The model simply predicts the majority class for every test case. Its ROC-AUC of 0.5000 confirms that it has no ability to distinguish between the two classes.

Therefore, the trained machine learning models provide a more meaningful comparison.

---

## 3. Model Comparison

Three machine learning models were trained using the same train/test split and feature set.

| Model               | Accuracy | Precision | Recall | F1-score | ROC-AUC |
| ------------------- | -------: | --------: | -----: | -------: | ------: |
| Baseline            |   0.6000 |    0.6000 | 1.0000 |   0.7500 |  0.5000 |
| Logistic Regression |   0.7500 |    0.7692 | 0.8333 |   0.8000 |  0.8453 |
| Decision Tree       |   0.7500 |    0.7917 | 0.7917 |   0.7917 |  0.8205 |
| Random Forest       |   0.7708 |    0.7908 | 0.8403 |   0.8148 |  0.8198 |

All three trained models substantially improve upon the baseline in accuracy and ROC-AUC.

Random Forest achieved the highest accuracy, recall, and F1-score among the trained models. Its accuracy was 0.7708 and its recall for the default class was 0.8403.

Logistic Regression achieved the highest ROC-AUC of 0.8453, which indicates stronger ranking/discrimination performance. However, Random Forest provided a stronger overall balance of accuracy, recall, and F1-score for the selected classification objective.

---

## 4. Final Model Selection

Random Forest was selected as the final model.

The main reason for this selection is its overall classification performance. It achieved the highest accuracy of 0.7708, the highest recall of 0.8403, and the highest F1-score of 0.8148 among the trained models.

Recall is particularly important in this loan-default problem because missing an actual default case can have practical consequences for lending decisions.

The choice also acknowledges an important trade-off: Logistic Regression achieved a higher ROC-AUC of 0.8453 compared with Random Forest's 0.8198. Therefore, Random Forest is not considered the best model on every metric, but it provides the strongest overall performance for the selected classification-focused objective.

---

## 5. Error Analysis

After selecting Random Forest as the final model, its predictions on the test set were compared with the actual default labels.

The misclassified test cases were examined to identify possible patterns in credit score, applicant income, loan amount, and employment type.

The analysis shows that model errors occur because the available applicant features do not completely separate default and non-default cases. Some applicants have feature values that make them difficult to classify correctly, resulting in both false positives and false negatives.

This indicates that the model should not be treated as a perfect decision-making system. In a real lending environment, additional applicant information and domain-specific checks would be required before making a final decision.

---

## 6. Calibration Analysis

A calibration curve was created for the final Random Forest model to evaluate whether its predicted probabilities correspond to the observed frequency of default cases.

The diagonal reference line represents perfect calibration. A model whose calibration curve is close to this line produces probabilities that are more consistent with the actual frequency of positive cases.

The calibration curve should therefore be considered alongside classification metrics rather than assuming that the predicted probabilities are automatically reliable.

The Random Forest probabilities should not be directly interpreted as guaranteed real-world default probabilities without additional calibration validation on representative data.

---

## 7. Limitation

One important limitation is that the dataset contains only a small set of applicant characteristics: credit score, applicant income, loan amount, and employment type.

Real-world credit risk can also depend on factors such as existing debt obligations, repayment history, outstanding loans, credit utilization, and other financial information.

Because these factors are not included, the model's predictions may not fully represent real-world credit risk. Therefore, the model should be considered an educational predictive model rather than a complete lending decision system.

---

## 8. Conclusion

The assessment demonstrated a complete machine learning classification pipeline from preprocessing through model evaluation and calibration.

The baseline established a reference point, while Logistic Regression, Decision Tree, and Random Forest provided substantially better predictive performance.

Random Forest was selected as the final model because it achieved the strongest combination of accuracy, recall, and F1-score. However, Logistic Regression achieved the highest ROC-AUC, highlighting the importance of considering multiple evaluation metrics rather than relying on a single score.

The error analysis and calibration analysis further show that model performance and predicted probabilities should be interpreted carefully. Additional real-world features, validation, and calibration would be required before using such a model for actual lending decisions.
