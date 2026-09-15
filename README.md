# SmartKart Customer Churn Analysis

## 📌 Project Overview

**SmartKart Customer Churn Analysis** is a customer analytics project focused on understanding factors associated with customer churn.

The dataset contains customer-level information including **age, monthly spending, complaint history, and churn status**. The file is intentionally **raw/dirty data**, making it suitable for practicing and demonstrating real-world data cleaning, exploratory data analysis (EDA), visualization, and customer retention analysis.

### Business Objective

The primary objective is to identify customer characteristics and behavioral patterns that may be associated with churn and to generate insights that can support:

* Customer retention strategies
* Churn-risk identification
* Customer segmentation
* Service-quality improvement
* Revenue protection
* Data-driven customer engagement

---

## 📂 Dataset

**File:** `SmartKart_dirty_100_rows(1).csv`

| Attribute       |           Value |
| --------------- | --------------: |
| Records         |             100 |
| Columns         |               5 |
| Target Variable |         `Churn` |
| Data Type       |   Tabular / CSV |
| Dataset Status  | Raw / Uncleaned |

---

## 📊 Data Dictionary

| Column          | Description                                              | Expected Type |
| --------------- | -------------------------------------------------------- | ------------- |
| `Customer_ID`   | Unique identifier assigned to each customer.             | String        |
| `Age`           | Age of the customer in years.                            | Numeric       |
| `Monthly_Spend` | Customer's recorded monthly spending with SmartKart.     | Numeric       |
| `Complaints`    | Number of customer complaints recorded.                  | Numeric       |
| `Churn`         | Customer churn indicator: `1` = churned, `0` = retained. | Binary        |

---

## 🎯 Target Variable

### `Churn`

The `Churn` column is the primary target variable for customer retention analysis.

* `1` → Customer has churned
* `0` → Customer has been retained

This variable can be used for:

* Churn-rate calculation
* Churn segmentation
* Statistical analysis
* Predictive modeling
* Customer-risk classification

---

## 🧹 Data Quality Assessment

This dataset intentionally contains several data-quality issues that should be addressed before production analysis.

### Missing Values

The dataset contains missing values in:

* `Age`
* `Monthly_Spend`
* `Complaints`

There are **6 records containing at least one missing value**, with:

* 3 missing `Age` values
* 2 missing `Monthly_Spend` values
* 2 missing `Complaints` values

> A single record may contain more than one missing field, so column-level missing-value counts should not be summed to determine the number of incomplete records.

### Invalid / Suspicious Values

Several values require validation before analysis:

#### `Age`

The observed age range includes values from **-5 to 150**, indicating invalid and potentially erroneous records.

Recommended validation rule:

```text
Age should be within a realistic customer age range.
```

#### `Monthly_Spend`

The dataset contains:

* Negative spending: `-1000`
* Extremely high spending: `99999`

These values should be investigated as potential data-entry errors or outliers before calculating average spend or customer value.

#### `Complaints`

The dataset contains a complaint count of **50**, which is substantially higher than the other observed complaint values and should be investigated as a potential outlier.

### Customer ID Formatting

Some customer IDs contain leading/trailing whitespace, for example:

```text
" C004 "
"C048 "
```

Customer IDs should be standardized using whitespace trimming before performing duplicate detection or joins.

### Duplicate Customer IDs

After normalizing whitespace, the dataset contains **5 duplicate customer IDs**.

The repeated IDs include:

```text
C005
C030
C050
C070
C080
```

These records should be reviewed to determine whether they represent:

* Duplicate customer records
* Multiple transactions belonging to the same customer
* Data-entry errors
* Legitimate repeated observations

---

## 🔧 Recommended Data Cleaning Process

Before performing business analysis, the following workflow is recommended:

### 1. Standardize Customer IDs

Remove leading and trailing whitespace.

```python
df["Customer_ID"] = df["Customer_ID"].str.strip()
```

### 2. Convert Numeric Columns

Convert the analytical fields to appropriate numeric types.

```python
numeric_cols = ["Age", "Monthly_Spend", "Complaints", "Churn"]

for col in numeric_cols:
    df[col] = pd.to_numeric(df[col], errors="coerce")
```

### 3. Handle Missing Values

Investigate missing values before deciding whether to:

* Impute
* Remove records
* Use model-based imputation
* Retain missingness as a separate category

The appropriate approach depends on the analytical objective.

### 4. Validate Age

Flag impossible or unrealistic ages.

```python
df.loc[(df["Age"] < 0) | (df["Age"] > 100), "Age"] = None
```

The exact upper bound should be determined according to SmartKart's business rules.

### 5. Validate Monthly Spend

Investigate negative and extreme spending values.

```python
df.loc[df["Monthly_Spend"] < 0, "Monthly_Spend"] = None
```

Extreme positive values should be investigated rather than automatically deleted.

### 6. Investigate Duplicate Customers

After cleaning the IDs:

```python
df[df["Customer_ID"].duplicated(keep=False)]
```

Duplicates should be resolved according to the business definition of a customer record.

### 7. Validate Complaint Counts

Check for unusually high complaint counts and determine whether they represent genuine customers or data-entry errors.

---

## 📈 Recommended KPIs

After cleaning the dataset, the following KPIs can be developed:

| KPI                          | Description                                       |
| ---------------------------- | ------------------------------------------------- |
| **Total Customers**          | Total number of unique customers.                 |
| **Churned Customers**        | Number of customers classified as churned.        |
| **Churn Rate**               | Percentage of customers who have churned.         |
| **Retention Rate**           | Percentage of customers who have remained active. |
| **Average Monthly Spend**    | Average monthly customer spending.                |
| **Average Complaints**       | Average number of complaints per customer.        |
| **Churn by Age Group**       | Churn rate across customer age segments.          |
| **Churn by Spend Segment**   | Churn rate across customer spending segments.     |
| **Churn by Complaint Level** | Relationship between complaints and churn.        |

### Churn Rate Formula

```text
Churn Rate =
Number of Churned Customers / Total Customers × 100
```

---

## 📊 Recommended Analysis

The project can be extended with the following analyses:

### Customer Demographics

Analyze churn across age groups to determine whether customer age is associated with retention.

### Spending Behavior

Compare monthly spending between:

* Churned customers
* Retained customers

This can help identify whether customer value is associated with churn.

### Complaint Analysis

Analyze whether customers with higher complaint volumes have greater churn rates.

### Multivariate Analysis

Evaluate the combined effect of:

```text
Age
+
Monthly Spend
+
Complaints
→
Churn
```

This provides a stronger understanding than analyzing each variable independently.

---

## 🤖 Potential Machine Learning Applications

After appropriate cleaning and validation, this dataset can be used as a simple demonstration dataset for:

* Logistic Regression
* Decision Trees
* Random Forest
* Gradient Boosting
* Classification modeling
* Customer churn prediction

### Example Prediction Objective

> Predict whether a customer is likely to churn based on their age, monthly spending, and complaint history.

Because the dataset contains only **100 records**, any predictive-modeling results should be treated as educational or exploratory rather than production-ready.

---

## 📉 Recommended Visualizations

A business dashboard could include:

1. **Overall Churn Rate** — KPI card
2. **Churned vs Retained Customers** — Donut/bar chart
3. **Churn by Age Group** — Column chart
4. **Monthly Spend vs Churn** — Box plot
5. **Complaints vs Churn** — Bar/box plot
6. **Customer Spend Distribution** — Histogram
7. **Complaint Distribution** — Histogram
8. **Churn by Customer Segment** — Stacked bar chart

---

## 🛠️ Suggested Technology Stack

The project can be implemented using:

* **Python**
* **Pandas** — data manipulation
* **NumPy** — numerical analysis
* **Matplotlib / Seaborn** — visualization
* **Scikit-learn** — machine learning
* **Jupyter Notebook** — exploratory analysis
* **Power BI / Tableau** — business dashboarding
* **GitHub** — project version control

---

## 📁 Suggested Project Structure

```text
SmartKart-Customer-Churn/
│
├── data/
│   └── SmartKart_dirty_100_rows(1).csv
│
├── notebooks/
│   ├── 01_data_quality_check.ipynb
│   ├── 02_data_cleaning.ipynb
│   ├── 03_exploratory_analysis.ipynb
│   └── 04_churn_model.ipynb
│
├── src/
│   ├── data_cleaning.py
│   ├── analysis.py
│   └── modeling.py
│
├── reports/
│   └── SmartKart_Churn_Analysis.pdf
│
├── README.md
└── requirements.txt
```

---

## ⚠️ Important Data Disclaimer

This is a **small, raw, and intentionally unclean dataset**. It contains missing values, duplicate customer IDs, invalid values, and potential outliers.

Therefore:

> **No business conclusions should be drawn from the raw dataset before data validation and cleaning.**

The dataset is best suited for demonstrating an end-to-end analytics workflow rather than representing a production customer database.

---

## 🚀 Project Workflow

```text
Raw Dataset
     ↓
Data Quality Assessment
     ↓
Data Cleaning & Validation
     ↓
Exploratory Data Analysis
     ↓
Customer Segmentation
     ↓
Churn Driver Analysis
     ↓
KPI Development
     ↓
Visualization / Dashboard
     ↓
Churn Prediction
     ↓
Business Recommendations
```

---

## 💡 Expected Business Outcome

The final analysis should help SmartKart answer questions such as:

* What percentage of customers are churning?
* Which customer segments have the highest churn?
* Does higher complaint volume correspond to higher churn?
* Is monthly spending associated with retention?
* Which customers represent potential high-value churn risks?
* What customer groups should be prioritized for retention campaigns?
* What data-quality improvements are required before deploying a churn model?

---

## 👤 Project Type

**Domain:** Customer Analytics / Retail Analytics
**Use Case:** Customer Churn & Retention
**Analysis Type:** Exploratory Data Analysis + Predictive Analytics
**Dataset Size:** 100 customer records
**Data Status:** Raw / Dirty Dataset
**Primary Outcome:** Churn and retention insights

---

## 📄 License & Usage

This dataset should be treated as a project/demo dataset unless a separate license or data-usage agreement is provided.

Do not use the data to make real-world customer decisions without appropriate validation, governance, privacy controls, and business approval.
