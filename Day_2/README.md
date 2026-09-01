# 📊 Data Cleaning, Preprocessing & Exploratory Data Analysis

## 📌 Overview

This project contains my **Day 2 practical work for Week 6**, focused on understanding and implementing important concepts of **Data Cleaning, Data Preprocessing, Feature Transformation, and Exploratory Data Analysis (EDA)** using Python.

Today, I studied the concepts and then **practically implemented them on a sample dataset** to understand how raw data is prepared, transformed, analyzed, and visualized.

The practical work covers:

* Data Cleaning
* Handling Missing Values
* Removing Duplicate Records
* Handling Inconsistent Data
* Categorical Data Encoding
* One-Hot Encoding
* Label Encoding
* Feature Scaling
* Min-Max Scaling
* Standardization
* Z-Score
* Normalization vs Standardization
* Exploratory Data Analysis (EDA)
* Data Visualization
* Correlation Analysis
* Visualization Best Practices

---

## 📁 Project Structure

The project is organized as follows:

```text
Day2/
│
├── Data_Cleaning_Preprocessing_EDA_Practical.ipynb
│
├── Figures/
│   ├── 01_salary_distribution.png
│   ├── 02_employees_by_department.png
│   ├── 03_experience_vs_salary.png
│   ├── 04_salary_boxplot.png
│   └── 05_correlation_heatmap.png
│
├── README.md
│
└── requirements.txt
```

### 📄 Files & Directories

| File / Directory                                  | Description                                                                                        |
| ------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| `Data_Cleaning_Preprocessing_EDA_Practical.ipynb` | Main Jupyter Notebook containing explanations, Python code, practical implementation, and analysis |
| `Figures/`                                        | Contains all visualizations generated during the practical work                                    |
| `README.md`                                       | Project documentation and overview                                                                 |
| `requirements.txt`                                | Contains the Python dependencies required to run the notebook                                      |

---

# 🎯 Objectives

The main objectives of this practical session were to:

1. Understand the importance of **data cleaning**.
2. Identify and handle **missing values**.
3. Detect and remove **duplicate records**.
4. Handle **inconsistent categorical values**.
5. Understand why categorical data needs to be encoded.
6. Implement **One-Hot Encoding**.
7. Implement **Label Encoding**.
8. Understand the need for **feature scaling**.
9. Implement **Min-Max Scaling**.
10. Implement **Standardization and Z-score transformation**.
11. Understand the difference between **Normalization and Standardization**.
12. Perform **Exploratory Data Analysis (EDA)**.
13. Analyze relationships between numerical features using **correlation**.
14. Create and interpret different types of visualizations.
15. Save generated figures in a separate `Figures` directory.

---

# 🧹 1. Data Cleaning

Data cleaning is the process of identifying and fixing problems in raw data before performing analysis.

In this practical, I worked with common data-quality issues such as:

* Missing values
* Duplicate records
* Inconsistent categorical values

### Missing Values

Missing values occur when information is not available for a particular record.

For numerical features, I practiced filling missing values using the **median**.

### Duplicate Records

Duplicate rows can affect calculations and analysis by counting the same record more than once.

I checked for duplicate records and removed them where necessary.

### Inconsistent Values

The dataset contained inconsistent capitalization such as:

```text
Lahore
lahore
```

These values represent the same city, so I standardized them into a consistent format.

---

# 🔤 2. Categorical Data Encoding

Categorical data contains values such as:

```text
IT
HR
Finance
```

Many machine learning algorithms require numerical input, so categorical values may need to be converted into numerical representations.

## One-Hot Encoding

One-Hot Encoding creates separate binary columns for each category.

For example:

| Department | Finance | HR | IT |
| ---------- | ------: | -: | -: |
| IT         |       0 |  0 |  1 |
| HR         |       0 |  1 |  0 |
| Finance    |       1 |  0 |  0 |

A value of `1` indicates that the record belongs to that category.

## Label Encoding

Label Encoding assigns a numerical label to each category.

For example:

```text
Finance → 0
HR      → 1
IT      → 2
```

These numbers are labels and do not necessarily indicate an actual ranking between categories.

---

# ⚖️ 3. Feature Scaling

Different numerical features can have very different ranges.

For example:

```text
Age          → 20–40
Experience   → 1–10
Salary       → 45,000–120,000
```

Feature scaling helps bring numerical features onto comparable scales.

Two techniques were practically implemented.

## Min-Max Scaling

Min-Max Scaling transforms values into a fixed range, commonly:

```text
0 to 1
```

The smallest value becomes `0`, while the largest value becomes `1`.

## Standardization

Standardization transforms values based on their:

* Mean
* Standard deviation

After standardization:

```text
Mean ≈ 0
Standard Deviation ≈ 1
```

The resulting transformed value is called a **Z-score**.

---

# 🔎 4. Exploratory Data Analysis (EDA)

Exploratory Data Analysis is used to understand the characteristics, patterns, distributions, and relationships within a dataset.

In this practical, I used both **statistical analysis and visualization**.

The notebook includes:

### 📊 Summary Statistics

Used to understand:

* Mean
* Standard deviation
* Minimum
* Maximum
* Quartiles
* Count

### 📈 Histogram

Used to understand the distribution of numerical values.

The notebook uses a histogram to visualize:

**Salary Distribution**

### 📊 Bar Chart

Used to compare categories.

The notebook visualizes:

**Number of Employees by Department**

### 🔵 Scatter Plot

Used to examine the relationship between two numerical variables.

The notebook visualizes:

**Experience vs Salary**

A trend line is also included to show the overall direction of the relationship.

### 📦 Box Plot

Used to understand:

* Data spread
* Central tendency
* Potential outliers

The notebook uses a box plot to analyze:

**Salary Distribution**

### 🔥 Correlation Heatmap

Used to visually examine correlations between numerical features.

The heatmap includes:

* Age
* Experience
* Salary

---

# 🖼️ 5. Generated Figures

All figures generated during the practical session are automatically saved inside the `Figures/` directory.

The figures are saved at **300 DPI** so they can also be used in reports and documentation.

| Figure                           | Purpose                                              |
| -------------------------------- | ---------------------------------------------------- |
| `01_salary_distribution.png`     | Shows the distribution of employee salaries          |
| `02_employees_by_department.png` | Compares employees across departments                |
| `03_experience_vs_salary.png`    | Shows the relationship between experience and salary |
| `04_salary_boxplot.png`          | Shows salary spread and potential outliers           |
| `05_correlation_heatmap.png`     | Shows correlations between numerical features        |

---

# 🛠️ Technologies & Libraries

The practical work was implemented using Python and the following libraries:

* **Python**
* **Pandas** — data manipulation and analysis
* **NumPy** — numerical operations
* **Matplotlib** — data visualization
* **SciPy** — scientific and numerical computing
* **Scikit-learn** — preprocessing and feature transformation
* **Jupyter Notebook** — interactive practical implementation

---

# ⚙️ Installation & Setup

## 1. Create a Virtual Environment

It is recommended to use a virtual environment for the project.

```bash
python3 -m venv .venv
```

## 2. Activate the Virtual Environment

### Linux / macOS

```bash
source .venv/bin/activate
```

### Windows

```bash
.venv\Scripts\activate
```

## 3. Install Dependencies

After activating the virtual environment:

```bash
pip install -r requirements.txt
```

---

# ▶️ Running the Notebook

Start Jupyter Notebook using:

```bash
jupyter notebook
```

Then open:

```text
Data_Cleaning_Preprocessing_EDA_Practical.ipynb
```

Run the notebook **from top to bottom** so that each step is executed in the correct order.

---

# 📋 Requirements

The required Python packages are listed in:

```text
requirements.txt
```

Current dependencies include:

```text
numpy==2.2.6
pandas==2.3.3
matplotlib==3.10.9
scipy==1.15.3
scikit-learn
jupyter
```

---

# 🧠 Key Learning

The main learning from this practical session was that **data analysis starts with understanding and preparing the data**.

A typical workflow can be summarized as:

```text
Raw Data
   ↓
Inspect Data
   ↓
Clean Data
   ↓
Handle Missing Values
   ↓
Remove Duplicates
   ↓
Fix Inconsistencies
   ↓
Encode Categorical Data
   ↓
Scale / Transform Features
   ↓
Perform EDA
   ↓
Visualize Data
   ↓
Interpret Results
```

The goal was not only to execute Python code, but also to understand:


