# Customer Churn Analysis & Customer Intelligence

## 📊 Project Overview

This project analyzes customer churn to identify **customer retention patterns, churn risk factors, revenue at risk, and customer support-related indicators**.

The analysis combines customer, subscription, and support data stored in a SQLite database and uses Python for data cleaning, feature engineering, exploratory data analysis, visualization, and KPI analysis.

The goal is to transform raw customer data into actionable insights that can help businesses understand **why customers leave and which customers may require retention efforts**.

---

## 🎯 Business Objectives

The project focuses on answering questions such as:

* What is the overall customer churn rate?
* What percentage of customers are retained?
* Which subscription plans have higher churn?
* How much monthly revenue is at risk from churned customers?
* What is the average revenue per user (ARPU)?
* How long do customers typically remain subscribed?
* How frequently do customers raise complaints?
* Is customer-support escalation associated with churn?
* How can customers be categorized based on churn risk?
* How does churn vary across different states and plans?

---

## 🗂️ Dataset Structure

The project works with three main tables:

### 1. Customer Table — `db_customer`

Contains customer demographic information:

* `customerid`
* `name`
* `country`
* `state`
* `gender`
* `dob`
* `interests`
* `pincode`

### 2. Subscription Table — `db_subscription`

Contains subscription and financial information:

* `customerid`
* `subscription_start_date`
* `subscription_type`
* `renewal_date`
* `plan_type`
* `contract_type`
* `cancellation_date`
* `cancellation_reason`
* `monthly_charges`
* `cltv`
* `churn_score`

### 3. Support Table — `db_support`

Contains customer support information:

* `customerid`
* `complaint_date`
* `escalations`
* `csat_score`
* `comment`

---

## 🛠️ Technologies Used

* **Python**
* **Pandas** — data manipulation and analysis
* **NumPy** — numerical operations and feature engineering
* **Matplotlib** — data visualization
* **Seaborn** — statistical visualization
* **SQLite** — database management
* **Jupyter Notebook** — analysis environment

---

## 🔄 Project Workflow

```text
Raw Customer Data
       ↓
SQLite Database
       ↓
Data Import
       ↓
Data Cleaning
       ↓
Feature Engineering
       ↓
Merge Customer + Subscription + Support Data
       ↓
KPI & Churn Analysis
       ↓
Visualization
       ↓
Correlation Analysis
       ↓
Business Insights
```

---

## 🧹 Data Cleaning

The project performs several data-cleaning steps:

### Customer Data

* Renamed `name` to `customer_name`
* Removed `interests` and `pincode`
* Converted `dob` into datetime format
* Standardized gender values such as `Men` and `Women` to `Male` and `Female`
* Filled missing country values using the corresponding state-to-country mapping

### Subscription Data

* Converted subscription, renewal, and cancellation dates into datetime format

### Support Data

* Removed unused columns
* Converted `complaint_date` into datetime format
* Identified duplicate customer support records
* Created a complaint count for each customer
* Retained the latest support record for each customer before merging

---

## ⚙️ Feature Engineering

### Churn Flag

A `churn_flag` was created using the cancellation date:

```python
df['churn_flag'] = np.where(
    df['cancellation_date'].notna(),
    1,
    0
)
```

Where:

* `1` = Customer churned
* `0` = Customer retained

### Customer Tenure

Customer tenure was calculated in days.

For churned customers:

```text
Cancellation Date − Subscription Start Date
```

For active customers:

```text
Current Date − Subscription Start Date
```

### Churn Risk

Customers were categorized using their existing `churn_score`:

| Churn Score | Risk Level |
| ----------- | ---------- |
| < 50        | Low        |
| 50–69       | Medium     |
| ≥ 70        | High       |

### Complaint Count

The number of support complaints was calculated for each customer.

---

## 📈 Key Business Metrics

The notebook calculates several important customer-retention KPIs.

### Churn Rate

**28.57%**

### Retention Rate

**71.43%**

### Average Revenue Per User

**18.85**

### Average Customer Tenure

Approximately **1,452 days** based on the notebook's calculation.

### Revenue at Risk

Approximately **73.94K** in monthly charges associated with churned customers.

### Escalation Rate

**19.05%**

### Average Complaints per User

**0.43**

### Escalation vs Churn Correlation

The analysis reports a correlation of approximately:

**0.77**

This indicates a strong positive association between support escalations and churn within this dataset. Correlation should not be interpreted as proof that escalations directly cause churn.

---

## 📊 Churn by Plan

The analysis calculates churn rates by subscription plan:

| Plan     | Churn Rate |
| -------- | ---------: |
| Basic    |     60.00% |
| Standard |     22.22% |
| Premium  |     14.29% |

The Basic plan shows the highest churn rate in the analyzed dataset.

---

## 📊 Data Visualization

The project includes multiple visualizations using Matplotlib and Seaborn.

### Visualizations include:

* Monthly churn trend
* Churn rate by plan type
* Churn rate by state
* Correlation heatmap
* Pair plots
* Customer plan and monthly-charge comparisons
* Churn-risk comparisons
* Pivot-table based analysis

---

## 📋 Pivot Table Analysis

Pivot tables are used to summarize churn and customer metrics by plan.

Example metrics include:

* Total monthly charges
* Unique customer count
* Average churn rate

This provides a compact business view of customer performance across different plans.

---

## 💡 Key Insights

Based on the analysis performed in the notebook:

1. **Overall churn is 28.57%**, meaning more than one-quarter of the analyzed customers have churned.

2. **Basic-plan customers show the highest churn rate at 60%**, substantially higher than Standard and Premium customers.

3. **Premium customers have the lowest churn rate at 14.29%** among the analyzed plans.

4. Approximately **73.94K in monthly charges is associated with churned customers**, representing revenue that may be recoverable through effective retention strategies.

5. The analysis shows a **0.77 correlation between support escalations and churn**, suggesting that customers experiencing escalated support interactions may represent an important retention segment.

6. The churn-risk feature provides a simple way to segment customers into **low, medium, and high-risk groups** based on the existing churn score.

---

## 🚀 Business Recommendations

Based on these findings, a business could consider:

### 1. Focus on Basic-Plan Customers

Since the Basic plan has the highest observed churn rate, investigate:

* Pricing concerns
* Product limitations
* Customer experience
* Upgrade incentives

### 2. Prioritize High-Risk Customers

Use the churn-risk classification to identify customers who may require proactive retention efforts.

### 3. Improve Support Experience

Because support escalation has a strong association with churn in this dataset, businesses could monitor escalated complaints and prioritize faster resolution.

### 4. Protect Revenue at Risk

Identify churned or high-risk customers with higher monthly charges and CLTV and prioritize them for retention campaigns.

### 5. Monitor Churn Trends

Monthly churn trends can help identify periods where customer cancellations increase and allow teams to investigate potential causes.

---

## 📁 Project Files

Recommended repository structure:

```text
customer-churn-analysis/
│
├── churn_analysis.ipynb
├── customer_churn.db
├── customer_churn_data_raw.xlsx
├── exported_churn_data.csv
├── README.md
└── requirements.txt
```

---

## ▶️ How to Run the Project

### 1. Clone the repository

```bash
git clone <your-repository-url>
cd customer-churn-analysis
```

### 2. Install dependencies

```bash
pip install pandas numpy matplotlib seaborn openpyxl
```

### 3. Open the notebook

```bash
jupyter notebook churn_analysis.ipynb
```

### 4. Run the notebook

Run the cells sequentially to:

* Import the database
* Clean the datasets
* Engineer features
* Merge the tables
* Calculate churn KPIs
* Generate visualizations
* Perform correlation analysis
* Create pivot tables

---

## 📌 Skills Demonstrated

This project demonstrates practical skills in:

* Data Cleaning
* Exploratory Data Analysis (EDA)
* SQL/SQLite
* Python
* Pandas
* NumPy
* Data Visualization
* Feature Engineering
* Customer Churn Analysis
* KPI Development
* Correlation Analysis
* Business Intelligence
* Pivot Tables
* Business Recommendations

---

## 👨‍💻 Project Purpose

This project was developed as a **Data Analytics portfolio project** to demonstrate the ability to transform raw customer data into meaningful business insights using Python, SQL/SQLite, and data visualization.

---

## ⭐ Conclusion

The project demonstrates an end-to-end customer churn analysis workflow, starting from raw relational data and progressing through cleaning, feature engineering, analysis, visualization, and business interpretation.

The findings highlight **plan-level churn differences, revenue exposure, customer tenure, support escalations, and churn-risk segmentation**, providing a foundation for data-driven customer retention strategies.
