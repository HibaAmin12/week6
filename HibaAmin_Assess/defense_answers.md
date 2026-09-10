# Week 06 · Assessment — Written Defense

## 1. Your random forest's accuracy is probably close to, or below, your logistic regression's. Does that mean logistic regression should always be the answer for a problem shaped like this? Defend your actual final model choice.

No. Logistic Regression should not always be selected just because it performs well on one metric. Model selection should depend on the problem objective and the overall evaluation results.

In my results, Logistic Regression achieved an accuracy of **0.7500** and ROC-AUC of **0.8453**, while Random Forest achieved a higher accuracy of **0.7708**, recall of **0.8403**, and F1-score of **0.8148**. I selected Random Forest because it provided the strongest overall classification performance for this loan-default problem, especially in recall and F1-score. 

---

## 2. A colleague suggests computing the `credit_score` fill value from the entire dataset before splitting, since "it's just imputation, not modeling." Explain specifically why that's wrong, using your own train-only vs. full-dataset numbers.

This is wrong because using the entire dataset for imputation allows information from the test set to influence the preprocessing step. This is a form of **data leakage**, even though imputation itself is not a modeling algorithm.

In my pipeline, the `credit_score` median calculated from the training set was **649.0000**, while the median calculated from the full dataset was **650.0000**. The correct approach is to calculate the fill value using only `X_train` and then apply that same value to both the training and test sets. This keeps the test set completely unseen during the training process.

---

## 3. For a loan-default decision, is overall accuracy or recall on the "will default" class more important? Defend your answer, and explain what goes wrong in practice if you optimized for the wrong one.

For a loan-default decision, I would consider **recall on the "will default" class more important** because missing an actual defaulter can result in financial loss for the lender.

If the model is optimized only for accuracy, it may favor the majority class and still miss a significant number of actual default cases. These missed default cases are **false negatives**. In practice, this could mean approving loans for applicants who are actually likely to default.

In my results, Random Forest achieved a recall of **0.8403**, meaning it correctly identified a large proportion of the actual default cases in the test set. Therefore, recall is an important metric alongside accuracy and the other evaluation measures.

---

## 4. Walk through your calibration curve — would you trust this model's predicted probabilities directly to set a risk-based interest rate? Why or why not?

I would **not directly trust the Random Forest predicted probabilities to set a risk-based interest rate** without further calibration and validation.

The calibration curve compares the predicted probabilities with the actual fraction of positive cases. A perfectly calibrated model would have its curve close to the diagonal reference line. This tells us whether a predicted probability such as 0.7 actually corresponds to approximately 70% positive outcomes.

Even if the model has good classification performance, its probabilities may not be perfectly calibrated. Since interest rates are financial decisions, I would first validate the probability estimates on representative data and apply calibration if necessary before using them for risk-based pricing.

---

## 5. Name one real-world factor this pipeline doesn't have that you'd want before this model made actual lending decisions, and explain concretely why its absence should limit how much the model is trusted.

One important missing factor is the applicant's existing debt obligations or debt-to-income ratio (DTI).

The pipeline includes applicant income and loan amount, but it does not tell us how much debt the applicant already has. Two applicants with the same income and loan amount could have very different financial situations if one already has significant outstanding debt.

Without this information, the model may underestimate the default risk of applicants who are already heavily indebted. Therefore, the model should not be fully trusted for actual lending decisions until existing debt obligations and other measures of financial capacity are included.

Other factor missing from this pipeline is the applicant's **previous repayment history**.

Repayment history provides information about whether an applicant has previously paid loans on time or missed payments. Without this information, the model may give similar predictions to applicants who have very different histories of managing credit.

Because repayment behavior is directly relevant to default risk, its absence means that the model does not capture an important part of an applicant's financial profile. Therefore, the model should not be fully trusted for actual lending decisions without additional credit-history and financial features.
