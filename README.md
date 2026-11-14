# 🛍️ Customer Shopping Behavior Analytics
### End-to-End Retail Analysis using **Python · PostgreSQL · Power BI**

![Python](https://img.shields.io/badge/Python-Pandas%20%7C%20Jupyter-3776AB?logo=python&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-PostgreSQL-336791?logo=postgresql&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?logo=powerbi&logoColor=black)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

> **One-line summary:** I turned raw retail transaction data into a decision-ready dashboard that shows *who* buys, *what* they buy, *how* discounts and subscriptions affect spending, and *where* the business should focus to grow revenue and retention.

---

## 📌 Table of Contents
1. [Business Problem](#-business-problem)
2. [Project Highlights](#-project-highlights)
3. [Workflow](#-workflow)
4. [Dataset](#-dataset)
5. [Data Cleaning & Feature Engineering (Python)](#-data-cleaning--feature-engineering-python)
6. [Business Questions Answered (SQL)](#-business-questions-answered-sql)
7. [Power BI Dashboard](#-power-bi-dashboard)
8. [Key Insights & Recommendations](#-key-insights--recommendations)
9. [Repository Structure](#-repository-structure)
10. [How to Run](#-how-to-run)
11. [Skills Demonstrated](#-skills-demonstrated)
12. [About Me](#-about-me)

---

## 🎯 Business Problem

A retail company wants to understand its customers' shopping behavior in order to improve **revenue, customer loyalty and marketing efficiency**. Leadership needs clear answers to questions such as:

- Which customer groups (gender, age, loyalty tier) drive the most revenue?
- Do **discounts** actually help, or do they just reduce margin?
- Are **subscribers** more valuable than non-subscribers?
- Which products and categories perform best, and which are rated highest?
- Does shipping type influence how much customers spend?

📄 Full problem statement: [`Business Problem Document.pdf`](Business%20Problem%20%20Document.pdf)

---

## ✨ Project Highlights

| | |
|---|---|
| 🔄 **Full pipeline** | Raw CSV → Python cleaning → SQL analysis → Power BI dashboard → business report |
| 🧮 **10 business questions** | Solved in SQL using aggregations, subqueries, CTEs, `CASE` logic and **window functions** |
| 🧹 **Reproducible cleaning** | Feature engineering done in a Jupyter notebook |
| 📊 **Interactive dashboard** | KPIs, segment breakdowns and drill-downs |
| 💡 **Actionable recommendations** | Written for non-technical stakeholders |

---

## 🔁 Workflow

```
 customer_shopping_behavior.csv
              │
              ▼
   🐍 Python (Pandas, Jupyter)
   Data profiling · cleaning · feature engineering
              │
              ▼
   🐘 PostgreSQL
   10 business queries (CTEs, window functions, segmentation)
              │
              ▼
   📊 Power BI
   Interactive dashboard & KPI storytelling
              │
              ▼
   📝 Business report with insights & recommendations
```

---

## 📂 Dataset

**File:** `customer_shopping_behavior.csv`

Each row is a customer purchase. Fields used in the analysis include:

| Column | Description |
|---|---|
| `customer_id` | Unique customer identifier |
| `age` / `age_group` | Customer age and derived age bucket |
| `gender` | Customer gender |
| `item_purchased` / `category` | Product and its category |
| `purchase_amount` | Transaction value (USD) |
| `review_rating` | Customer rating of the product |
| `discount_applied` | Whether a discount was used (Yes/No) |
| `subscription_status` | Subscriber vs non-subscriber |
| `shipping_type` | Standard, Express, etc. |
| `previous_purchases` | Number of prior purchases (loyalty proxy) |

---

## 🧹 Data Cleaning & Feature Engineering (Python)

Notebook: [`Customer_Shopping_Behavior_Analysis.ipynb`](Customer_Shopping_Behavior_Analysis.ipynb)

- Profiled the dataset: structure, data types, summary statistics, missing values and duplicates
- Handled missing values and standardized column names for SQL compatibility
- Engineered analytical features such as **`age_group`** buckets and customer **loyalty segments**
- Loaded the cleaned data into **PostgreSQL** for analysis

---

## 🧠 Business Questions Answered (SQL)

All queries live in [`customer_behavior_sql_queries.sql`](customer_behavior_sql_queries.sql).

| # | Business Question | SQL Concepts Used |
|---|---|---|
| 1 | Total revenue from male vs. female customers | `GROUP BY`, `SUM` |
| 2 | Customers who used a discount but still spent above the average | Subquery, `AVG` |
| 3 | Top 5 products by average review rating | `ROUND`, `AVG`, `ORDER BY`, `LIMIT` |
| 4 | Average spend: Standard vs Express shipping | Filtering, aggregation |
| 5 | Do subscribers spend more? (customers, avg spend, revenue) | Multi-metric aggregation |
| 6 | Top 5 products with the highest discount dependency | Conditional aggregation (`CASE WHEN`) |
| 7 | Segment customers into **New / Returning / Loyal** | **CTE**, `CASE` |
| 8 | Top 3 most purchased products **within each category** | **CTE + `ROW_NUMBER() OVER (PARTITION BY …)`** |
| 9 | Are repeat buyers (5+ purchases) more likely to subscribe? | Cohort filtering |
| 10 | Revenue contribution by age group | Aggregation, sorting |

**Sample: top 3 products in every category (window function)**
```sql
WITH item_counts AS (
  SELECT category,
         item_purchased,
         COUNT(customer_id) AS total_orders,
         ROW_NUMBER() OVER (PARTITION BY category
                            ORDER BY COUNT(customer_id) DESC) AS item_rank
  FROM customer
  GROUP BY category, item_purchased
)
SELECT item_rank, category, item_purchased, total_orders
FROM item_counts
WHERE item_rank <= 3;
```

---

## 📊 Power BI Dashboard

File: [`customer_behavior_dashboard.pbix`](customer_behavior_dashboard.pbix)

The dashboard gives executives a single-page view of customer behavior:

- **KPI cards:** total revenue, average purchase value, customer count, average rating
- **Segment analysis:** revenue by gender, age group, category and loyalty tier
- **Discount and subscription impact** on spending
- **Shipping-type comparison**
- **Interactive slicers** to filter by category, age group, subscription status and more

<!-- Add your screenshot below (upload to an /images folder), then remove the comment markers -->
<!-- ![Dashboard Preview](images/dashboard.png) -->

📄 Full written analysis: [`Customer Shopping Behavior Analysis.pdf`](Customer%20Shopping%20Behavior%20Analysis.pdf)

---

## 💡 Key Insights & Recommendations

| Area | Insight | Recommendation |
|---|---|---|
| **Revenue drivers** | [Gender / age group X] contributes [X%] of total revenue | Target campaigns at the highest-value segment |
| **Discounts** | [X%] of discounted purchases still exceeded the average spend; some products depend heavily on discounts | Limit discounts on products that sell well without them to protect margin |
| **Subscriptions** | Subscribers vs non-subscribers: [avg spend / revenue comparison] | Promote subscription to loyal customers; build exclusive perks |
| **Loyalty** | [X%] of customers are in the *Loyal* segment | Launch a rewards program to move *Returning* customers up to *Loyal* |
| **Products** | Top-rated and top-selling products identified in each category | Prioritize inventory and marketing for proven winners |
| **Shipping** | Avg spend differs between Standard and Express: [values] | Use shipping thresholds or offers to increase basket size |

---

## 🗂️ Repository Structure

```
├── customer_shopping_behavior.csv            # Raw dataset
├── Customer_Shopping_Behavior_Analysis.ipynb # Python: cleaning & feature engineering
├── customer_behavior_sql_queries.sql         # 10 business queries (PostgreSQL)
├── customer_behavior_dashboard.pbix          # Power BI dashboard
├── Business Problem  Document.pdf            # Problem statement & objectives
├── Customer Shopping Behavior Analysis.pdf   # Final analysis report
└── README.md
```

---

## ▶️ How to Run

1. **Clone the repo**
   ```bash
   git clone https://github.com/Rkamal0806/Customer-Behavior-dashboard-data-analysis-SQL-Python-PowerBI.git
   cd Customer-Behavior-dashboard-data-analysis-SQL-Python-PowerBI
   ```
2. **Python cleaning**
   ```bash
   pip install pandas jupyter sqlalchemy psycopg2-binary
   jupyter notebook Customer_Shopping_Behavior_Analysis.ipynb
   ```
3. **SQL:** load the cleaned data into a PostgreSQL table named `customer`, then run `customer_behavior_sql_queries.sql`.
4. **Dashboard:** open `customer_behavior_dashboard.pbix` in **Power BI Desktop**.

---

## 🧰 Skills Demonstrated

- **Python:** Pandas, data cleaning, feature engineering, EDA, Jupyter
- **SQL (PostgreSQL):** subqueries, CTEs, window functions, conditional aggregation
- **Power BI:** data modeling, KPI design, interactive dashboards, visual storytelling
- **Business analytics:** customer segmentation, discount and subscription analysis, turning data into recommendations
- **Communication:** documented problem statement, report and stakeholder-friendly insights

---

## 👤 About Me

**Kamal**, aspiring Data Analyst passionate about turning data into decisions.

🔗 [GitHub](https://github.com/Rkamal0806) · 💼 LinkedIn: *add your link* · 📧 Email: *add your email*

⭐ *If you found this project useful, please consider giving it a star!*
