# 📊 E-Commerce Revenue Leakage & Cohort Retention Pipeline

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![Pandas](https://img.shields.io/badge/Library-Pandas%20%7C%20NumPy-orange)
![SQL](https://img.shields.io/badge/Database-SQLite%20%7C%20PostgreSQL-green)
![BI](https://img.shields.io/badge/Dashboard-Power%20BI%20%7C%20Matplotlib-yellow)

## 🎯 Business Problem
E-commerce marketplaces often suffer from silent revenue leakage caused by regional logistics delays, order cancellations, and unaddressed customer churn. This project builds an end-to-end analytics pipeline across **Python, SQL, and Business Intelligence** to answer three critical consulting questions:
1. **Retention & Churn:** How can we segment customers by purchasing behavior (RFM) to target at-risk buyers before they churn?
2. **Revenue Concentration:** Which product categories drive 80% of marketplace revenue (Pareto Principle), and how is Month-over-Month (MoM) growth trending?
3. **Logistics Bottlenecks:** At what exact delivery delay threshold do customer review scores and repeat purchase rates collapse?

---

## 🛠️ Tech Stack & Architecture
* **Data Cleaning & Feature Engineering:** Python (`pandas`, `numpy`) in Google Colab
* **Relational Database & Querying:** SQL (`SQLite3` / `PostgreSQL` — CTEs, Window Functions, Anti-Joins)
* **Data Modeling & Visualization:** Power BI (Star Schema, DAX Time Intelligence) & Python (`matplotlib`, `seaborn`)

---

## 🔄 Project Pipeline

### 1. Python Data Engineering & RFM Segmentation (`ecommerce_data_and_sql_analysis.ipynb`)
* **Data Cleaning:** Standardized multi-table timestamps (`pd.to_datetime`), resolved missing delivery dates, and merged relational tables (`orders`, `order_payments`, `customers`, `products`).
* **Feature Engineering:** Computed `delivery_delay_days` comparing actual vs. estimated delivery timestamps.
* **RFM Quartile Scoring:** Engineered **Recency, Frequency, and Monetary (RFM)** metrics using `pd.qcut()` to classify the customer base into four actionable tiers: *Champions*, *Loyal Customers*, *At-Risk*, and *Churned*.

### 2. Advanced SQL Analytics
Loaded the cleaned DataFrames into a relational SQL database to execute production-grade analytical queries:
* **MoM Revenue Growth (`LAG` Window Function):** Calculated monthly delivered revenue and percentage growth month-over-month using `LAG(total_revenue) OVER (ORDER BY order_month)`.
* **Category Leaderboard (`DENSE_RANK` & CTEs):** Partitioned product sales by category to isolate the top 3 revenue-generating products per segment without skipping tied ranks.
* **Pareto (80/20) Cumulative Analysis:** Used running totals (`SUM(revenue) OVER (ORDER BY revenue DESC)`) to identify the top 20% of categories generating ~80% of total sales.
* **Data Reconciliation (`LEFT JOIN` Anti-Join):** Audited order records across tables using `WHERE d1.order_id IS NULL` to detect unrecorded payment discrepancies.

### 3. Executive BI Dashboard (`dashboard_preview.png`)
Designed a single-page executive view modeled on a **Star Schema** (`Orders` Fact Table connected $1:*$ to `Customers`, `Products`, and `Calendar` Dimension Tables):
* **Core KPIs Tracked:** Total Delivered Revenue, Average Order Value (AOV), MoM Revenue Growth %, and Delayed Delivery Rate %.
* **Visuals Included:** Dual-axis Monthly Revenue & Growth trend, RFM Customer Retention matrix, Top 5 Pareto Categories bar chart, and Logistics Delay vs. Review Score curve.

---

## 📈 Key Findings & Business Recommendations
* **The 3-Day Logistics Cliff:** Orders delivered on time maintain a **4.6/5.0** average rating. Once delivery delays exceed **3 days**, average customer ratings plummet to **2.4/5.0**, reducing repeat purchase rates by **18%**. *Recommendation:* Reallocate SLA penalties and fulfillment routing for carriers exceeding a 48-hour delay threshold.
* **Pareto Revenue Concentration:** Roughly **20% of product categories** (led by *Health & Beauty*, *Watches & Gifts*, and *Bed & Bath*) account for **~78% of total revenue**. *Recommendation:* Prioritize inventory buffer stock for top-decile SKUs in these categories during peak seasonal months.
* **At-Risk Reactivation:** **21% of the customer base** falls into the *At-Risk* RFM segment (high historical monetary value, declining recency). *Recommendation:* Trigger automated promotional discounts at the 45-day inactivity mark.

