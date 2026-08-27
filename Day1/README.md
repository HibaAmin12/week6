# Week 06 — EDA Precision Lab

## Real Relationships vs. Real Comparisons, and Verified Findings

This project focuses on improving visualization accuracy and analytical reporting through a focused EDA precision lab. The main goal is to distinguish genuine relationships between continuous variables from categorical comparisons, compute and report Pearson correlations honestly, identify misleading visualizations, perform visual quality assurance, and verify numerical findings before reporting them.

---

## Learning Objectives

By completing this lab, the following skills were practiced:

* Distinguishing a genuine relationship chart from a categorical comparison chart.
* Computing Pearson correlation coefficients using numerical data.
* Reporting near-zero correlations as valid findings instead of ignoring them.
* Building scatter plots with fitted trend lines for continuous-variable relationships.
* Building bar charts for categorical comparisons.
* Identifying misleading line charts created from unordered categories.
* Performing visual QA on actual saved PNG files.
* Detecting and fixing layout collisions and overlapping chart elements.
* Using colors deliberately rather than assigning arbitrary colors to categories.
* Maintaining meaningful category order such as A → B → C.
* Verifying every numerical finding against a fresh computation.
* Performing a self-audit before considering the analysis complete.

---

## Dataset

The dataset was generated using the exact specification provided for the lab.

It contains **600 student records** and the following variables:

| Column                  | Description                         |
| ----------------------- | ----------------------------------- |
| `student_id`            | Unique identifier for each student  |
| `class_section`         | Student's class section: A, B, or C |
| `study_hours_per_week`  | Weekly study hours                  |
| `sleep_hours_per_night` | Average nightly sleep hours         |
| `attendance_pct`        | Attendance percentage               |
| `exam_score`            | Student's exam score                |

The dataset was generated with a fixed random seed so that the results can be reproduced consistently.

---

## Analysis Tasks

### 1. Genuine Relationship — Study Hours vs. Exam Score

A scatter plot was created to examine the relationship between weekly study hours and exam score.

A fitted trend line was included to make the overall direction of the relationship easier to interpret.

The Pearson correlation coefficient was also calculated so that the relationship could be reported numerically rather than judged only from the visual appearance of the scatter plot.

---

### 2. Null Relationship — Sleep Hours vs. Exam Score

A second scatter plot was created using sleep hours and exam score.

The Pearson correlation was calculated and reported even if the resulting value was close to zero.

This demonstrates that a weak or null relationship is still a valid analytical finding and should not be omitted simply because it is not visually interesting.

---

### 3. Categorical Comparison — Exam Score by Class Section

Exam scores were compared across class sections A, B, and C using a categorical comparison chart.

This is a comparison rather than a relationship because `class_section` is a categorical variable rather than a continuous numeric variable.

The shuffle test was used to support this distinction: if the category order were changed, the comparison would still represent the same groups.

---

### 4. Line-Chart Trap

A connected line chart of average exam scores for sections A, B, and C was intentionally created.

This demonstrates why connecting unordered categories with a line can be misleading. The line visually suggests a continuous trend or movement from one category to the next, even though there is no meaningful numerical progression between class sections.

The bar chart is therefore the more appropriate visualization for this categorical comparison.

---

## Visual Quality Assurance

Visual QA was performed as an explicit part of the workflow rather than relying only on the notebook's inline output.

### Layout Collision

A chart was intentionally given a poor layout by placing an annotation where it could overlap another chart element.

The resulting chart was saved as a PNG and inspected at its actual file size.

The layout was then corrected using repositioning and/or `tight_layout()`.

Both the problematic and corrected versions were retained to demonstrate the QA process.

### Color and Category Order

The comparison chart was rebuilt using two different color approaches:

1. An intentionally arbitrary color assignment.
2. A consistent and more appropriate color choice.

The comparison also maintained the natural category order:

**A → B → C**

This preserves the meaningful structure of the categories instead of sorting them only by their numerical values.

### Labels and Readability

Chart readability was also evaluated by comparing a deliberately poor version with an improved version.

The improved chart used:

* A descriptive title.
* Meaningful x-axis and y-axis labels.
* A larger figure size.
* Appropriate spacing using `tight_layout()`.

These changes make the chart easier to interpret without requiring additional explanation.

---

## Self-Audit

A final numerical self-audit was performed to verify the written findings.

Three reported values were checked:

1. Pearson correlation between study hours and exam score.
2. Pearson correlation between sleep hours and exam score.
3. Mean exam score for one selected class section.

Each value was independently recomputed and compared against the number reported in the findings.

The purpose of this step was to ensure that numerical statements were based on freshly computed results rather than memory or manually assumed values.

All reported values were checked for consistency before finalizing the analysis.

---

## Key Lessons

This lab demonstrated that creating a visualization is only one part of a reliable EDA workflow.

A complete workflow should follow:

**Generate → Analyze → Visualize → Verify → Inspect → Fix → Report**

Important lessons from the lab include:

* A scatter plot is appropriate for investigating relationships between continuous variables.
* A bar or box plot is appropriate for comparing a continuous variable across categories.
* Correlation should be calculated rather than estimated only from visual appearance.
* A near-zero correlation is still a meaningful result.
* Connecting unordered categories with a line can create a false impression of a trend.
* Saved PNG files should be visually inspected because notebook previews can hide layout problems.
* Colors should not introduce unintended meaning.
* Natural category order should be preserved when it carries useful structure.
* Every numerical claim in the findings should be verified against a fresh computation.

---

## Final Visual QA Checklist

* [x] Dataset generated from the exact specification.
* [x] Genuine relationship chart created.
* [x] Pearson correlation calculated and reported.
* [x] Null relationship chart created and reported.
* [x] Genuine categorical comparison created.
* [x] Shuffle test used to distinguish comparison from relationship.
* [x] Line-chart trap intentionally created and analyzed.
* [x] Layout collision intentionally created.
* [x] Actual saved PNG inspected for the collision.
* [x] Layout corrected and final PNG saved.
* [x] Arbitrary and deliberate color versions compared.
* [x] Categories maintained in natural A → B → C order.
* [x] Labels and readability improved.
* [x] Final visual QA completed.
* [x] Self-audit performed using freshly recomputed values.

---

## Conclusion

The Week 06 EDA Precision Lab strengthened the distinction between relationships and categorical comparisons while emphasizing accurate numerical reporting and visual quality assurance.

The final workflow demonstrates that a reliable visualization should not only use the correct chart type but should also communicate clearly, avoid misleading visual cues, survive inspection as a saved file, and contain findings that have been independently verified against the underlying data.
