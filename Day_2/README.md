# Week 6 — EDA Precision Lab

## 📌 Overview

This project focuses on performing a **careful and reproducible Exploratory Data Analysis (EDA)** on learner engagement and course completion data.

The analysis goes beyond basic visualization by validating numerical claims, interpreting relationships correctly, distinguishing **association from causation**, and using statistical techniques such as **Pearson correlation** and **Bootstrap 95% Confidence Intervals**.

The goal is to produce findings that are not only visually clear but also **statistically supported and correctly interpreted**.

---

## 🎯 Objectives

The main objectives of this analysis are to:

* Explore learner engagement and completion patterns.
* Analyze the relationship between weekly login hours and completion.
* Compare completion rates across different course tracks.
* Examine the relationship between forum activity and completion.
* Calculate and verify the overall mean completion.
* Quantify the difference between Data Science and Web Dev completion rates.
* Use Bootstrap resampling to construct a **95% confidence interval**.
* Avoid unsupported causal claims from observational data.
* Validate all numerical findings using fresh calculations.

---

## 📊 Dataset

The analysis uses a learner-level dataset stored in the `learners` DataFrame.

Key variables include:

| Variable             | Description                                         |
| -------------------- | --------------------------------------------------- |
| `course_track`       | Learner's course track                              |
| `weekly_login_hours` | Average weekly time spent logging into the platform |
| `completion_pct`     | Course completion percentage                        |
| `forum_posts`        | Number of forum posts made by the learner           |

The dataset is analyzed using Python and pandas.

---

## 🔍 Analysis Performed

### 1. Weekly Login Hours vs. Completion

A scatter plot and Pearson correlation are used to examine whether learners who spend more time logging in tend to have higher completion percentages.

The analysis identifies a **positive association** between weekly login hours and completion.

However, because the data are observational, the result does **not establish causation**. Other factors, such as learner motivation, may influence both login activity and completion.

---

### 2. Completion by Course Track

Course tracks are categorical groups, so the comparison is interpreted as a **group-level comparison** rather than a relationship between two continuous variables.

The analysis compares completion across:

* Data Science
* Web Dev
* Design

Among the three tracks, **Data Science has the highest observed average completion**.

---

### 3. Forum Activity vs. Completion

Pearson correlation is used to evaluate the relationship between forum activity and completion.

The correlation is reported objectively, even when the relationship is weak or close to zero. This avoids selectively reporting only strong results.

---

### 4. Overall Completion

The overall mean completion is calculated directly from the `learners` DataFrame.

This ensures that the reported value is based on the actual dataset rather than relying on an approximate claim.

---

### 5. Data Science vs. Web Dev

The mean completion rates are compared directly:

* **Data Science:** 76.7756%
* **Web Dev:** 66.9564%
* **Mean gap:** 9.8192 percentage points

To assess the uncertainty around this difference, **10,000 bootstrap samples** are generated.

The resulting Bootstrap 95% Confidence Interval is:

**[6.3050, 13.2346]**

Since the confidence interval **does not include zero**, the observed difference provides statistical evidence that Data Science has a higher mean completion rate than Web Dev.

---

## 📈 Statistical Methods

### Pearson Correlation

Pearson correlation is used to measure the strength and direction of a linear association between numerical variables.

A positive value indicates that the variables tend to increase together, while a value close to zero indicates little linear association.

> Correlation indicates association, not causation.

### Bootstrap 95% Confidence Interval

Bootstrap resampling is used to estimate the uncertainty around the difference in mean completion between Data Science and Web Dev.

The procedure:

1. Randomly resample each course-track group **with replacement**.
2. Calculate the difference between their means.
3. Repeat the process **10,000 times**.
4. Use the 2.5th and 97.5th percentiles of the bootstrap distribution as the 95% confidence interval.
5. Check whether the interval contains zero.

---

## 💡 Key Findings

* Weekly login hours show a **positive association** with course completion.
* The login-hours result should **not be interpreted as proof of causation**.
* Course track is correctly treated as a **categorical comparison**.
* **Data Science has the highest observed average completion** among the three tracks.
* Forum activity is reported using its Pearson correlation, including weak or null results where applicable.
* The overall completion mean is calculated directly from the dataset.
* Data Science has an average completion rate approximately **9.82 percentage points higher** than Web Dev.
* The Bootstrap 95% CI for the Data Science–Web Dev mean difference is **[6.3050, 13.2346]**.
* The confidence interval does **not include zero**, supporting a statistically significant difference at the 5% level.

---

## 🧠 Correct Interpretation

The analysis demonstrates the importance of distinguishing **statistical association from causation**. Visual patterns and correlations can identify relationships in the data, but they do not by themselves establish causal effects.

The Data Science vs. Web Dev comparison is strengthened by a Bootstrap confidence interval rather than relying solely on visual differences. Numerical claims are also verified through fresh calculations from the dataset.

Overall, the analysis emphasizes **accuracy, reproducibility, statistical reasoning, and responsible interpretation of EDA results**.

---

## 🛠️ Tools & Technologies

* **Python**
* **Pandas** — Data manipulation and analysis
* **NumPy** — Numerical computations and bootstrap resampling
* **Matplotlib** — Data visualization
* **Jupyter Notebook** — Interactive analysis

---

## 📁 Project Structure

```text
Week6/
│
├── Day_2/
│   └── ...
│
├── Figures/
│   └── ...
│
├── README.md
├── requirements.txt
└── .gitignore
```

---

## ▶️ How to Run

### 1. Clone the repository

```bash
git clone <repository-url>
cd week6
```

### 2. Create and activate a virtual environment

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Launch Jupyter

```bash
jupyter notebook
```

Open the relevant notebook and run the cells from top to bottom.

---

## ✅ Conclusion

This project demonstrates a **precision-focused EDA workflow** where findings are supported by verified calculations and appropriate statistical methods. The analysis carefully separates association from causation, treats categorical variables correctly, and uses Bootstrap confidence intervals to quantify uncertainty in group differences.

The final results provide a more reliable and statistically responsible understanding of learner engagement and course completion.
