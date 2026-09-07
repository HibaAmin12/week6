# Week 06 — Machine Learning Model Evaluation

A complete machine learning evaluation pipeline covering **regression, classification, model comparison, overfitting analysis, feature importance, error analysis, and probability calibration** using a student performance dataset.

---

## 📌 Project Overview

This project evaluates different machine learning models to predict student exam performance and classify students based on their exam scores.

### Main Objectives

The project encompasses two core machine learning tasks:

- **Regression Task**: Predict the student's `exam_score` (continuous value)
- **Classification Task**: Classify whether a student's `exam_score` is ≥ 85 or < 85

Multiple models are trained and evaluated using a fixed train-test split, with comprehensive analysis of overfitting, feature importance, and error patterns.

---

## 🎯 Key Learning Objectives

✓ Establish baseline models for regression and classification  
✓ Train and compare different machine learning algorithms  
✓ Identify and analyze overfitting in complex models  
✓ Control model complexity using hyperparameters  
✓ Perform feature importance analysis  
✓ Conduct comprehensive error and residual analysis  
✓ Evaluate probability calibration  
✓ Select best models based on objective metrics  
✓ Maintain reproducibility using fixed random seeds  

---

## 📂 Dataset

### Dataset Specifications

- **Size**: 600 student records
- **Columns**: 6 features + 1 target
- **Format**: Structured tabular data

### Features

| Feature | Type | Description |
|---------|------|-------------|
| `student_id` | Integer | Unique identifier for each student |
| `class_section` | Categorical | Student's class section (A, B, C) |
| `study_hours_per_week` | Numeric | Hours spent studying per week |
| `sleep_hours_per_night` | Numeric | Average hours of sleep per night |
| `attendance_pct` | Numeric | Student attendance percentage |
| `exam_score` | Numeric | Student's exam score (target) |

### Target Variables

**Regression Task**
- **Target**: `exam_score` (continuous)
- **Use Case**: Direct prediction of numerical exam scores

**Classification Task**
- **Target**: Binary classification (0 or 1)
  - `1` = exam_score ≥ 85 (Distinction)
  - `0` = exam_score < 85 (No distinction)

---

## ⚙️ Data Preprocessing

### Categorical Encoding

The `class_section` feature was converted to numerical format using one-hot encoding with first category dropped:

```python
X = pd.get_dummies(
    students[
        [
            "class_section",
            "study_hours_per_week",
            "sleep_hours_per_night",
            "attendance_pct",
        ]
    ],
    columns=["class_section"],
    drop_first=True,
    dtype=int,
)
```

### Final Features

- `class_section_B` (binary)
- `class_section_C` (binary)
- `study_hours_per_week` (numeric)
- `sleep_hours_per_night` (numeric)
- `attendance_pct` (numeric)

### Train-Test Split

```python
X_train, X_test, y_train, y_test = train_test_split(
    X, y,
    test_size=0.2,
    random_state=42
)
```

- **Train Set**: 480 samples (80%)
- **Test Set**: 120 samples (20%)
- **Random Seed**: 42 (for reproducibility)

---

## 📈 Regression Models

### Models Trained

1. **Dummy Mean Baseline** - Predicts mean training value
2. **Linear Regression** - Simple linear model
3. **Decision Tree Regressor** - Unconstrained tree (overfitting analysis)
4. **Decision Tree Regressor (max_depth=3)** - Complexity-controlled tree
5. **Random Forest Regressor** - Ensemble with 200 trees

### Evaluation Metrics

**RMSE (Root Mean Squared Error)**
- Measures average magnitude of prediction errors
- Lower is better
- Formula: √(Σ(predicted - actual)² / n)

**R² Score**
- Measures proportion of variance explained by the model
- Range: 0 to 1 (higher is better)
- Negative values indicate worse than baseline

### Regression Results

| Model | Test RMSE | Test R² | Notes |
|-------|-----------|---------|-------|
| Dummy Mean Baseline | 10.2978 | -0.0096 | Baseline performance |
| Linear Regression | **7.0584** | **0.5257** | **Best Model** ✓ |
| Decision Tree (Unconstrained) | 10.2120 | 0.0072 | Severe overfitting |
| Decision Tree (max_depth=3) | 7.1031 | 0.5197 | Near-best performance |
| Random Forest (200 trees) | 7.3518 | 0.4854 | Higher complexity, lower performance |

### 🏆 Best Regression Model

**Linear Regression** emerged as the best-performing model:

- ✓ Lowest test RMSE: **7.0584**
- ✓ Highest test R²: **0.5257**
- ✓ Excellent generalization to unseen data
- ✓ Interpretable and efficient

---

## 🌳 Overfitting Analysis

### Unconstrained Decision Tree

An unconstrained Decision Tree Regressor was trained to demonstrate overfitting:

```
Train RMSE = 0.0000 (Perfect fit on training data)
Test RMSE  = 10.2120 (Poor performance on test data)
Test R²    = 0.0072

Train-Test RMSE Gap = 10.2120
```

**Findings:**
- The model memorized training observations perfectly
- Failed to generalize to unseen data
- Train-test gap of ~10.21 indicates **severe overfitting**

### Controlling Complexity with max_depth

Restricting tree depth significantly improved generalization:

```python
DecisionTreeRegressor(max_depth=3, random_state=42)
```

**Results:**
```
Train RMSE = 7.0019
Test RMSE  = 7.1031
Test R²    = 0.5197

Train-Test RMSE Gap = 0.1012 (Much smaller!)
```

**Improvement:**
- Train-test gap reduced by **99%**
- Test performance remained strong
- Model learned generalizable patterns instead of memorizing

---

## 🌲 Random Forest Analysis

### Random Forest Regressor (200 trees)

```python
RandomForestRegressor(n_estimators=200, random_state=42)
```

**Performance:**
```
Train RMSE = 2.8716
Test RMSE  = 7.3518
Test R²    = 0.4854
```

**Key Insight:**
- Lower training error does not guarantee better test performance
- Ensemble methods can still underperform simpler models
- Model complexity ≠ Better generalization

### Feature Importance

Random Forest identified the following feature contributions:

| Feature | Importance | % of Total |
|---------|-----------|-----------|
| study_hours_per_week | 0.6424 | **64.24%** ⭐ |
| attendance_pct | 0.1680 | 16.80% |
| sleep_hours_per_night | 0.1391 | 13.91% |
| class_section_C | 0.0340 | 3.40% |
| class_section_B | 0.0165 | 1.65% |

**Insight:** Study hours is by far the most predictive feature for exam scores.

---

## 🔀 Classification Models

### Models Trained

1. **Dummy Most-Frequent Baseline** - Always predicts majority class
2. **Logistic Regression** - Linear probability model
3. **Decision Tree Classifier** - Tree-based classifier
4. **Random Forest Classifier** - Ensemble with 200 trees

### Evaluation Metrics

**Accuracy**
- Proportion of correct predictions
- Formula: (TP + TN) / Total

**Precision**
- Proportion of positive predictions that were correct
- Formula: TP / (TP + FP)
- Important when false positives are costly

**Recall**
- Proportion of actual positives identified correctly
- Formula: TP / (TP + FN)
- Important when false negatives are costly

**F1-Score**
- Harmonic mean of precision and recall
- Formula: 2 × (Precision × Recall) / (Precision + Recall)
- Balanced metric for imbalanced classes

### Classification Results

| Model | Accuracy | Precision | Recall | F1-Score |
|-------|----------|-----------|--------|----------|
| Dummy Most-Frequent | 0.6500 | 0.6500 | 1.0000 | 0.7879 |
| **Logistic Regression** | **0.7667** | **0.8125** | **0.8333** | **0.8228** ✓ |
| Decision Tree | 0.7167 | 0.7973 | 0.7564 | 0.7763 |
| Random Forest (200 trees) | 0.7417 | 0.7831 | 0.8333 | 0.8075 |

### 🏆 Best Classification Model

**Logistic Regression** achieved the best overall classification performance:

- ✓ Highest Accuracy: **0.7667**
- ✓ Highest Precision: **0.8125**
- ✓ Highest F1-Score: **0.8228**
- ✓ Tied Recall with Random Forest: **0.8333**
- ✓ Simpler and more interpretable than complex models

---

## ❌ Error Analysis

### Regression Error Analysis

The selected Linear Regression model was analyzed for its largest prediction errors:

**Largest Error - Student 576:**
```
Actual Score     = 65.3
Predicted Score  = 84.1137
Absolute Error   = 18.8137
```

**Second Largest - Student 516:**
```
Actual Score     = 76.0
Predicted Score  = 94.2987
Absolute Error   = 18.2987
```

**Key Findings:**
- Large errors observed in students with unusual feature combinations
- No single feature explains all major errors
- Suggests missing factors affecting exam performance
- Model performs well on typical cases

### Classification Error Analysis

Logistic Regression produced both false positives and false negatives:

**False Negatives (Predicted 0, Actually 1):**
- Example: Student 220
  - Study Hours: 3.6
  - Attendance: 97.4%
  - Model underestimated exam score

**False Positives (Predicted 1, Actually 0):**
- Example: Student 218
  - Study Hours: 13.0
  - Attendance: 94.0%
  - Predicted Probability: 0.9689
  - Other factors prevented high exam score

**Insight:** Study hours and attendance are strong predictors but not deterministic.

---

## 📐 Probability Calibration

### Calibration Curve Analysis

Logistic Regression probability estimates were evaluated for calibration:

**High Probability Range (Excellent Calibration):**
```
Mean Predicted Probability = 0.9636
Actual Fraction Positive   = 0.9630
Calibration Gap            = 0.0007 (Near perfect!)
```

**Middle Probability Range (Largest Gap):**
```
Mean Predicted Probability = 0.5500
Actual Fraction Positive   = 0.7143
Calibration Gap            = 0.1643
```

**Interpretation:**
- ✓ High-confidence predictions are highly reliable
- ⚠ Medium-confidence predictions may be underestimated
- ✓ Overall reasonable calibration

---

## 📋 Final Model Selection

### Selected Models

| Task | Model | Rationale |
|------|-------|-----------|
| **Regression** | Linear Regression | Lowest RMSE (7.0584), Highest R² (0.5257) |
| **Classification** | Logistic Regression | Highest Accuracy & F1-Score, Simplicity |

### Selection Criteria

Models were selected based on:
- Test-set performance metrics
- Generalization ability
- Interpretability
- Computational efficiency
- Not model complexity alone

---

## 💡 Key Findings

### Regression Insights

✓ Linear Regression achieved best overall performance  
✓ Unconstrained Decision Trees exhibited severe overfitting  
✓ Restricting tree depth dramatically reduced overfitting  
✓ Complexity doesn't guarantee better test performance  
✓ Study hours was the most important feature (64.24%)  

### Classification Insights

✓ Logistic Regression achieved best overall performance  
✓ Dummy baseline had high recall but poor overall metrics  
✓ Random Forest achieved same recall but lower precision  
✓ Simpler models sometimes outperform complex ones  
✓ Study hours was most important feature (52.77%)  

### General Insights

✓ Fixed seeds enable reproducible results  
✓ Evaluation metrics provide objective model comparison  
✓ Test performance is the true measure of model quality  
✓ Probability calibration matters for confidence assessment  
✓ Error analysis reveals model limitations and data gaps  

---

## 🔁 Reproducibility

All experiments use a fixed random seed to ensure reproducible results:

```python
RANDOM_SEED = 42

# Train-Test Split
train_test_split(X, y, test_size=0.2, random_state=RANDOM_SEED)

# Random Forest Regressor
RandomForestRegressor(n_estimators=200, random_state=RANDOM_SEED)

# Random Forest Classifier
RandomForestClassifier(n_estimators=200, random_state=RANDOM_SEED)
```

This ensures experiments can be reproduced consistently across different runs and environments.

---

## 🛠️ Technologies Used

- **Python 3.x** - Programming language
- **Pandas** - Data manipulation and preprocessing
- **NumPy** - Numerical computations
- **Scikit-learn** - Machine learning models and metrics
- **Matplotlib** - Data visualization
- **Jupyter Notebook** - Interactive development environment

---

## 📁 Project Structure

```
Week6/
│
├── Day1/
│   └── Week6-Day1.ipynb
│
├── Day2/
│   └── Week6-Day2.ipynb
│
├── Day3/
│   └── Week6-Day3.ipynb
│
├── Day4/
│   ├── Week6_Day4_Review_Complete_ML_Pipeline.ipynb
│   ├── Week6_Day4_Evaluation_Report.md
│   ├── README.md
│   └── Figures/
│       ├── calibration_curve.png
│       ├── regression_comparison.png
│       └── classification_comparison.png
│
└── README.md (this file)
```

---

## 📊 Quick Reference: Evaluation Summary

### Regression Performance

| Metric | Linear Regression |
|--------|------------------|
| Test RMSE | 7.0584 |
| Test R² | 0.5257 |
| Train-Test Gap | Minimal |
| Best Feature | study_hours_per_week |

### Classification Performance

| Metric | Logistic Regression |
|--------|-------------------|
| Accuracy | 0.7667 |
| Precision | 0.8125 |
| Recall | 0.8333 |
| F1-Score | 0.8228 |
| Best Feature | study_hours_per_week |

---

## 🎓 Learning Outcomes

After completing this project, you will understand:

1. **Model Evaluation** - How to properly evaluate ML models using appropriate metrics
2. **Train-Test Paradigm** - Why test performance is the true measure of model quality
3. **Overfitting Detection** - How to identify and measure overfitting
4. **Hyperparameter Tuning** - How to control model complexity
5. **Feature Importance** - Which features drive model predictions
6. **Error Analysis** - How to understand and learn from model mistakes
7. **Probability Calibration** - How to interpret prediction confidence
8. **Model Comparison** - How to systematically compare different algorithms
9. **Reproducibility** - Best practices for reproducible machine learning

---

## ⭐ Final Takeaway

> **The best model is not necessarily the most complex model — it is the model that provides the strongest evidence of good generalization on unseen data.**

---

## 📝 Notes

- All random seeds are fixed for reproducibility
- Test set evaluation is paramount for model selection
- Simpler models often generalize better than complex ones
- Feature importance helps explain model behavior
- Error analysis guides feature engineering efforts
- Probability calibration enables confident decision-making

---

