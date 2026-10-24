<div align="center">

<img src="assets/banner.svg" width="100%" alt="E-Commerce Business Analytics — SQL and Power BI on 99K Olist orders.">

<img src="https://img.shields.io/badge/SQL-0a0e1a?style=for-the-badge" alt="SQL">
<img src="https://img.shields.io/badge/MySQL-0a0e1a?style=for-the-badge&logo=mysql&logoColor=c9a96e" alt="MySQL">
<img src="https://img.shields.io/badge/PostgreSQL-0a0e1a?style=for-the-badge&logo=postgresql&logoColor=c9a96e" alt="PostgreSQL">
<img src="https://img.shields.io/badge/Power_BI-0a0e1a?style=for-the-badge" alt="Power BI">
<img src="https://img.shields.io/badge/Python-0a0e1a?style=for-the-badge&logo=python&logoColor=c9a96e" alt="Python">
<img src="https://img.shields.io/badge/Pandas-0a0e1a?style=for-the-badge&logo=pandas&logoColor=c9a96e" alt="Pandas">

</div>

> **In one line:** advanced SQL (CTEs, window functions, stored procedures) answers real business questions on the Brazilian Olist marketplace, and a 5-page Power BI report turns the answers into KPIs.

## Business questions

| Question | SQL module |
| --- | --- |
| Which categories drive most of the revenue? | `03_revenue_analysis.sql` (Pareto) |
| Who are our most valuable customers? | `04_customer_rfm.sql` (RFM with window functions) |
| Where do deliveries go wrong, and does it hurt reviews? | `05_delivery_analysis.sql` |
| Which sellers underperform? | `06_seller_performance.sql` |
| Do customers come back? | `07_cohort_analysis.sql` |

## Key findings

Every number below comes from the query outputs in [`results/`](results).

| | Finding | Source |
| --- | --- | --- |
| 📦 | **Revenue is concentrated:** the top 10 product categories bring in **62.4%** of revenue (top 5: 39.7%), led by health & beauty and watches & gifts. | `03_revenue_q2.csv` |
| 📈 | **Black Friday spike:** revenue jumped **+53.6%** in Nov 2017, then fell **−26.9%** in Dec 2017. | `03_revenue_q1.csv` |
| 🗺️ | **São Paulo dominates:** SP alone generates **R$5.77M** from 40,500 orders, about 2.8× the next state (RJ). | `03_revenue_q4.csv` |
| 🚚 | **Delivery:** average delivery takes **12.5 days**; **8.1%** of orders arrive late. Shipping is the slowest stage (9.3 of 12.5 days). | `05_delivery_q1.csv`, `05_delivery_q4.csv` |
| ⭐ | **Late delivery kills satisfaction:** late orders average a **2.57** review vs **4.29** on time. | `05_delivery_q5.csv` |
| 🌎 | **Northern states wait longest:** Roraima averages 29.3 days and Alagoas has a 23.9% late rate. | `05_delivery_q3.csv` |
| 👥 | **RFM:** "At Risk" customers are 7.7% of the base but hold **14.7%** of revenue, which makes them the clearest win-back target. | `04_rfm_segments_q1.csv` |
| 🏪 | **Seller quality:** the 387 underperforming sellers average a **22%** cancellation rate and a 1.95 review score, vs 0.25% and 4.27 for average sellers. | `06_sellers_q2.csv` |

## Power BI dashboard

<div align="center">
<img src="powerbi/image.png" width="90%" alt="Power BI executive summary page">
</div>

Five report pages (Executive Summary, Revenue Analysis, Customer Intelligence, Delivery Performance and Seller Scorecard) are built on a star schema with DAX measures and drill-through. Screenshots of every page are in the [Power BI guide](powerbi/README.md), and the report file is [`Dashboard.pbix`](Dashboard.pbix).

## SQL techniques

| Technique | Where it is used |
| --- | --- |
| CTEs (`WITH`) | Revenue, RFM, cohorts |
| Window functions (`RANK`, `DENSE_RANK`, `LAG`, `LEAD`, `ROW_NUMBER`, `NTILE`) | RFM scoring, seller ranking, MoM growth |
| Multi-table joins (5+ tables) | All business queries |
| Subqueries and `CASE WHEN` | Pareto cut-off, segmentation |
| Date functions | Cohorts, delivery timing |
| Stored procedures and indexes | Reusable KPI reports, query speed |

## Dataset

[Olist Brazilian E-Commerce (Kaggle)](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce): **99,441 orders**, 112,650 order items, 3,095 sellers, 9 tables, Sep 2016 – Oct 2018.

## Project structure

```text
ecommerce-sql-analytics/
├── sql/              01_schema_setup … 08_stored_procedures
├── data/             Olist CSVs (see data/README.md)
├── results/          CSV output of every analysis query
├── powerbi/          Dashboard guide + screenshots
├── Dashboard.pbix    Power BI report
├── load_data.py      Loads the CSVs into the database
└── run_analysis.py   Runs the SQL files and saves results/
```

## How to run

```bash
# 1. Create the schema
mysql -u root -p ecommerce < sql/01_schema_setup.sql
# 2. Load the Olist CSVs
python load_data.py
# 3. Run every analysis query and export results/
python run_analysis.py
```

---

<div align="center">
<sub>Built by <a href="https://github.com/Suhas5497"><b>Suhas Dhamapurkar</b></a> — Data Analyst &amp; AI/ML Engineer · Founder of <a href="https://resilytics.in">Resilytics</a><br>
<a href="https://linkedin.com/in/suhas-1710d">LinkedIn</a> · <a href="https://portfolio-git-main-suhasdhamapurkar1710-gmailcoms-projects.vercel.app/">Portfolio</a> · <a href="mailto:suhasdhamapurkar1710@gmail.com">Email</a></sub>
</div>
