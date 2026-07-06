# Customer Behavior Analytics Pipeline

<p align="center">
  <img src="assets/Screenshot%202026-07-06%20191807.png" alt="Customer Behavior Power BI Dashboard" width="100%" />
</p>

<p align="center">
  <b>End-to-end customer shopping behavior analytics project using Python, PostgreSQL, SQL, and Power BI.</b>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-Pandas-blue?style=for-the-badge&logo=python" />
  <img src="https://img.shields.io/badge/PostgreSQL-Database-316192?style=for-the-badge&logo=postgresql" />
  <img src="https://img.shields.io/badge/SQL-DBeaver-orange?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?style=for-the-badge&logo=powerbi&logoColor=black" />
</p>

---

## Project Overview

This project analyzes customer shopping behavior to identify trends across revenue, product categories, customer demographics, subscription status, review ratings, purchase frequency, and sales performance.

The project follows a complete analytics engineering workflow:

```text
Raw CSV Dataset → Python + Pandas → PostgreSQL → DBeaver SQL Analysis → Power BI Dashboard → Business Insights
```

The goal was not only to analyze a CSV file, but to build a practical portfolio-grade data pipeline where cleaned data is stored in a database, analyzed using SQL, and visualized through an interactive BI dashboard.

---

## Business Objective

The business objective was to convert raw customer shopping data into decision-ready insights.

Key business questions answered:

- Which product categories generate the highest revenue?
- Which age groups contribute the most sales?
- How do subscribers and non-subscribers behave differently?
- Which products are the top revenue contributors?
- What is the average customer purchase value?
- Which customer segments should be prioritized for marketing and retention?

---

## Solution Architecture

<p align="center">
  <img src="assets/Element%20Data%20Processing-2026-07-06-135032.png" alt="Customer Behavior Analytics Pipeline Architecture" width="100%" />
</p>

The pipeline was designed as a structured analytics workflow:

1. **Raw CSV Dataset**  
   Customer shopping behavior data was used as the source file.

2. **Python + Pandas Data Preparation**  
   Data was loaded, cleaned, transformed, and feature-engineered using Pandas.

3. **PostgreSQL Database Layer**  
   The cleaned Pandas DataFrame was pushed into PostgreSQL using SQLAlchemy.

4. **DBeaver SQL Analysis**  
   SQL queries were written for revenue analysis, product analysis, subscription behavior, and customer segmentation.

5. **Power BI Dashboard**  
   Power BI was connected to PostgreSQL to create an interactive dashboard with KPI cards, charts, and slicers.

---

## Tech Stack

| Layer | Technology | Purpose |
|---|---|---|
| Data Source | CSV | Raw customer shopping behavior dataset |
| Data Processing | Python, Pandas | Data cleaning and feature engineering |
| Database Connectivity | SQLAlchemy, psycopg2 | Python-to-PostgreSQL connection |
| Database | PostgreSQL | Structured analytics storage |
| SQL IDE | DBeaver | SQL exploration and business analysis |
| Visualization | Power BI Desktop | Interactive dashboard and reporting |
| Documentation | PDF, PPT | Portfolio report and presentation |

---

## Repository Structure

```text
customer-behavior-analytics-pipeline/
│
├── assets/
│   ├── Screenshot 2026-07-06 191807.png
│   └── Element Data Processing-2026-07-06-135032.png
│
├── data/
│   └── customer_shopping_behavior.csv
│
├── notebook/
│   └── data_cleaning_pipeline.ipynb
│
├── sql/
│   └── analysis_queries.sql
│
├── reports/
│   └── Customer_Shopping_Behavior_Portfolio_Report_Final.pdf
│
├── presentation/
│   └── Customer-Shopping-Behavior-Analytics-Pipeline.pptx
│
├── requirements.txt
├── .gitignore
└── README.md
```

---

## Data Engineering Pipeline

### 1. Data Loading

The raw CSV file was loaded into a Pandas DataFrame for cleaning and transformation.

```python
import pandas as pd

df = pd.read_csv("data/customer_shopping_behavior.csv")
```

### 2. Column Name Standardization

Raw column names were standardized into lowercase `snake_case` format to make the dataset easier to use in Python, SQL, and Power BI.

| Original Column | Cleaned Column |
|---|---|
| Customer ID | customer_id |
| Purchase Amount (USD) | purchase_amount_usd |
| Review Rating | review_rating |
| Subscription Status | subscription_status |
| Frequency of Purchases | frequency_of_purchases |

Example logic:

```python
df.columns = (
    df.columns
    .str.strip()
    .str.lower()
    .str.replace(r"[^a-z0-9]+", "_", regex=True)
    .str.strip("_")
)
```

### 3. Missing Value Handling

Missing values in `review_rating` were handled using category-wise median imputation.

```python
df["review_rating"] = df.groupby("category")["review_rating"].transform(
    lambda x: x.fillna(x.median())
)
```

This method is stronger than filling all missing values with one global median because it preserves product-category behavior.

### 4. Feature Engineering

New analytical columns were created to improve SQL and Power BI analysis.

| Feature | Description |
|---|---|
| `age_group` | Groups customers into Young Adult, Adult, Middle-aged, and Senior |
| `purchase_frequency_days` | Converts Weekly, Monthly, Quarterly, etc. into numeric day values |
| `review_rating` | Missing values filled using category-wise median |

Purchase frequency mapping:

```python
frequency_mapping = {
    "Weekly": 7,
    "Fortnightly": 14,
    "Bi-Weekly": 14,
    "Monthly": 30,
    "Quarterly": 90,
    "Every 3 Months": 90,
    "Annually": 365
}

df["purchase_frequency_days"] = df["frequency_of_purchases"].map(frequency_mapping)
```

---

## PostgreSQL Database Layer

After cleaning and feature engineering, the transformed Pandas DataFrame was loaded into a local PostgreSQL database.

The database layer converted the project from simple file-based analysis into a structured analytics workflow.

```text
Cleaned Pandas DataFrame → SQLAlchemy Engine → PostgreSQL Table → SQL + Power BI
```

Key implementation points:

- Used SQLAlchemy for database connectivity.
- Used PostgreSQL as the central analytics database.
- Loaded the cleaned DataFrame directly into a PostgreSQL table.
- Connected both DBeaver and Power BI to the same PostgreSQL database.

---

## SQL Analysis

SQL analysis was performed in DBeaver on top of the PostgreSQL database.

The SQL layer was used to validate dashboard metrics and generate deeper business insights.

### SQL Techniques Used

- Aggregations
- `GROUP BY` analysis
- Common Table Expressions
- Window functions
- Ranking queries
- Subscriber vs non-subscriber comparison
- Product and category performance analysis

### Business Questions Answered Using SQL

- Total revenue by gender
- Customers using discounts who spent more than average
- Top 5 products by average review rating
- Standard vs Express shipping purchase comparison
- Subscriber vs non-subscriber revenue comparison
- Top products by discount usage

SQL file included in the repository:

```text
sql/analysis_queries.sql
```

---

## Power BI Dashboard

The final dashboard was built in Power BI Desktop by connecting directly to the PostgreSQL database.

<p align="center">
  <img src="assets/Screenshot%202026-07-06%20191807.png" alt="Customer Behavior Dashboard" width="100%" />
</p>

### Dashboard Components

- **KPI Cards**
  - Total Customers
  - Average Purchase Amount
  - Average Review Rating

- **Donut Chart**
  - Subscription Status split

- **Bar and Column Charts**
  - Revenue by Category
  - Sales by Category
  - Revenue by Age Group
  - Sales by Age Group

- **Interactive Slicers**
  - Subscription Status
  - Gender
  - Category
  - Shipping Type

---

## Key Business Insights

### Executive KPIs

| Metric | Value |
|---|---:|
| Total Customers | 3.9K |
| Average Purchase Amount | $59.76 |
| Average Review Rating | 3.75 |

### Subscription Status

| Subscription Status | Share |
|---|---:|
| Non-Subscribers | 73% |
| Subscribers | 27% |

**Insight:** Non-subscribers represent the majority of the customer base, showing a clear opportunity for subscription conversion campaigns.

### Revenue by Category

| Category | Revenue | Sales Volume |
|---|---:|---:|
| Clothing | $104,264 | 1,737 |
| Accessories | $74,200 | 1,240 |
| Footwear | $36,093 | 599 |
| Outerwear | $18,524 | 324 |

**Insight:** Clothing is the strongest category by both revenue and sales volume, making it a priority segment for marketing, inventory planning, and campaign targeting.

### Age Group Revenue

| Age Group | Revenue |
|---|---:|
| Young Adult | $62,143 |
| Middle-aged | $59,197 |
| Adult | $55,978 |
| Senior | $55,763 |

**Insight:** Young Adults generated the highest revenue and can be treated as a high-value customer segment.

### Seasonal Revenue

| Season | Revenue |
|---|---:|
| Fall | $60,018 |
| Spring | $58,679 |
| Winter | $58,607 |
| Summer | $55,777 |

**Insight:** Fall generated the highest seasonal revenue, making it an important period for campaign planning.

### Top Product Revenue

| Product | Revenue |
|---|---:|
| Blouse | $10,410 |
| Shirt | $10,332 |
| Dress | $10,320 |
| Pants | $10,090 |
| Jewelry | $10,010 |

**Insight:** These products can be treated as hero products for promotions and inventory prioritization.

---

## How to Run This Project Locally

### 1. Clone the Repository

```bash
git clone https://github.com/bansalsamarth1/customer-behavior-analytics-pipeline.git
cd customer-behavior-analytics-pipeline
```

### 2. Create a Virtual Environment

```bash
python -m venv .venv
```

Activate it:

```bash
# Windows
.venv\Scripts\activate

# macOS/Linux
source .venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Set Up PostgreSQL

Create a local PostgreSQL database, for example:

```text
customer_behavior_db
```

Update the PostgreSQL connection details in the notebook or use environment variables for better security.

### 5. Run the Notebook

Open and run:

```text
notebook/data_cleaning_pipeline.ipynb
```

This will:

- Load the CSV file
- Clean and transform the data
- Create engineered columns
- Push the cleaned DataFrame into PostgreSQL

### 6. Run SQL Analysis

Open the SQL file in DBeaver:

```text
sql/analysis_queries.sql
```

Run the queries against the PostgreSQL table.

### 7. View the Dashboard

Open the Power BI dashboard/report file if available locally, or use the dashboard screenshot and project report included in this repository.

---

## Deliverables

| Deliverable | Location |
|---|---|
| Raw Dataset | `data/customer_shopping_behavior.csv` |
| Data Cleaning Notebook | `notebook/data_cleaning_pipeline.ipynb` |
| SQL Queries | `sql/analysis_queries.sql` |
| Dashboard Preview | `assets/Screenshot 2026-07-06 191807.png` |
| Pipeline Architecture | `assets/Element Data Processing-2026-07-06-135032.png` |
| PDF Report | `reports/Customer_Shopping_Behavior_Portfolio_Report_Final.pdf` |
| Presentation | `presentation/Customer-Shopping-Behavior-Analytics-Pipeline.pptx` |

---

## Skills Demonstrated

### Data Engineering

- CSV ingestion
- Data cleaning pipeline
- Feature engineering
- PostgreSQL data loading
- Database-backed analytics workflow

### Python & Pandas

- DataFrame manipulation
- Missing value handling
- Group-based transformations
- Column normalization
- Categorical mapping

### SQL

- Aggregations
- CTEs
- Window functions
- Ranking
- Business analysis queries

### Business Intelligence

- Power BI dashboard design
- KPI creation
- Slicer-based interactivity
- Executive-level data storytelling
- Revenue and customer segmentation analysis

---

## Project Learning Outcome

This project demonstrates the ability to build a full analytics workflow from raw data to business-ready insights.

It reflects practical skills required for roles such as:

- Data Analyst
- Business Intelligence Analyst
- Data Engineer
- Analytics Engineer
- SQL Developer
- Power BI Developer

---

## Author

**Samarth Bansal**  
GitHub: [bansalsamarth1](https://github.com/bansalsamarth1)

---

## Final Note

This project is designed as a portfolio case study to show practical capability in data cleaning, database design, SQL analysis, and dashboard storytelling.

```text
Raw Data → Clean Data → Database → SQL Insights → BI Dashboard → Business Decisions
```
