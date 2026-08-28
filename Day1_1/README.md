# Week 06 — Day 1_1

## EDA Precision Lab

### Overview

This task focused on performing a precise and reproducible Exploratory Data Analysis (EDA) workflow. The analysis used a student performance dataset containing study hours, sleep hours, attendance, class sections, and exam scores.

The main goal was not only to create visualizations, but also to ensure that each chart type matched the analytical question, numerical findings were verified, and statistical conclusions were interpreted carefully.

### Dataset

A synthetic dataset of **600 students** was generated using NumPy with a fixed random seed for reproducibility.

The dataset contains the following columns:

* `student_id`
* `class_section`
* `study_hours_per_week`
* `sleep_hours_per_night`
* `attendance_pct`
* `exam_score`

Dataset shape:

* **600 rows**
* **6 columns**

No missing values or duplicate rows were found.

### Topics Covered

#### 1. Genuine Relationship — Study Hours vs Exam Score

A scatter plot and Pearson correlation were used to examine the relationship between weekly study hours and exam scores.

* Pearson correlation: **0.6895**
* The result shows a moderate positive linear association.

#### 2. Null Relationship — Sleep Hours vs Exam Score

A scatter plot and Pearson correlation were used to investigate whether sleep hours were meaningfully associated with exam scores.

* Pearson correlation: **-0.0217**
* The result indicates a negligible linear relationship.

#### 3. Genuine Comparison — Exam Score by Class Section

Average exam scores were compared across Sections A, B, and C using a bar chart.

* Section A: **87.61**
* Section B: **87.03**
* Section C: **91.36**

Section C had the highest average exam score.

#### 4. Line-Chart Trap

A line chart was intentionally used with unordered class-section categories to demonstrate how connecting categorical values can create a misleading impression of continuity or trend.

The category order was also shuffled to show that changing the order changes the visual pattern even though the underlying values remain unchanged.

#### 5. Visual QA

A layout collision was deliberately introduced into a chart and then fixed.

The saved PNG files were checked to ensure that the final figures were readable and did not contain clipping or overlapping elements.

#### 6. Color and Category Order

Arbitrary colors were compared with a consistent color treatment.

The final comparison kept the natural category order:

**A → B → C**

rather than sorting sections according to their average scores.

#### 7. Labels and Readability

A poorly labeled chart was compared with an improved version using:

* descriptive titles
* meaningful axis labels
* appropriate figure size
* improved layout

#### 8. Correlation vs Causation

The positive relationship between study hours and exam scores was interpreted as an **association**, not a causal relationship.

Student motivation was identified as a plausible confounding variable.

#### 9. Pearson vs Spearman

Both correlation methods were calculated for study hours and exam score.

* Pearson: **0.6895**
* Spearman: **0.7010**
* Absolute difference: **0.0115**

The two correlations are close, indicating that the positive relationship is broadly consistent under both linear and rank-based measures.

#### 10. Bootstrap Confidence Interval

A bootstrap analysis was performed to quantify uncertainty in the mean exam-score difference between Sections A and C.

* Observed difference (A − C): **-3.7541**
* 95% CI lower bound: **-5.7234**
* 95% CI upper bound: **-1.8329**
* Confidence interval includes zero: **False**

Because the confidence interval does not include zero, the observed difference is supported as being different from zero at the 95% confidence level.

#### 11. Small Multiples

Study hours versus exam score was visualized separately for Sections A, B, and C using shared axes.

Section-wise Pearson correlations were:

* Section A: **0.6828**
* Section B: **0.7053**
* Section C: **0.6897**

All three sections showed a positive association.

#### 12. Numerical Self-Audit

Key numerical findings were independently recomputed from the dataset and compared with the values used in the written analysis.

The final audit returned:

```text
Do all audited numerical values match? True
```

This confirms that the audited numerical findings are internally consistent with the dataset.

### Figures

The notebook generated and saved multiple figures covering:

* Study hours vs exam score
* Sleep hours vs exam score
* Average exam score by class section
* Line-chart trap
* Original vs shuffled category order
* Deliberately broken layout
* Fixed layout
* Arbitrary vs consistent colors
* Poor vs improved readability
* Small multiples by class section

### Key Learning Outcomes

By completing this task, I practiced:

* Selecting appropriate chart types for relationships and comparisons
* Interpreting Pearson and Spearman correlations
* Distinguishing correlation from causation
* Identifying potential confounding variables
* Understanding misleading line charts for categorical data
* Performing visual QA on saved figures
* Improving chart readability and labeling
* Using bootstrap resampling to estimate confidence intervals
* Comparing relationships across groups using small multiples
* Independently verifying numerical findings
* Using f-strings for reproducible numerical reporting
* Re-running the notebook from a clean kernel for reproducibility

### Reproducibility

The dataset was generated using a fixed random seed, and statistical procedures that involve randomness also use a fixed `random_state`.

The notebook should be tested using:

**Restart Kernel → Run All**

to confirm that all cells execute successfully from the beginning without relying on previously stored variables or outputs.
