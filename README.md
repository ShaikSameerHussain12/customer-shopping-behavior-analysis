# customer-shopping-behavior-analysis
Retail customer behavior analysis (Python, SQL, Power BI) — includes corrected percentile-based segmentation and quantified business recommendations, not just charts.

# Customer Shopping Behavior Analysis
End-to-end retail analytics project: Python for data cleaning, SQL for business-question analysis, and Power BI for stakeholder-ready visualization.

![Python](https://img.shields.io/badge/Python-3.11-blue) ![SQL](https://img.shields.io/badge/SQL-T--SQL-orange) ![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-yellow) ![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

---

## 1. Business Problem

A retail company wants to understand what drives repeat purchases and higher spend across its customer base — and which levers (discounts, subscriptions, shipping type, product category) are actually worth investing in versus which ones just look good on paper.

**Business question:** How can shopping behavior data be used to identify which customer segments and product strategies drive revenue, and which commonly-used levers (like discounts) are overrated?

This project answers that with a full pipeline: raw data → cleaned dataset → SQL-driven business analysis → interactive dashboard → recommendations.

## 2. Dataset

- **Source:** [Customer Shopping Trends Dataset (Kaggle)](https://www.kaggle.com/datasets/iamsouravbanerjee/customer-shopping-trends-dataset) — synthetic retail transaction data
- **Size:** 3,900 rows × 18 columns
- **Fields:** customer demographics, item/category purchased, purchase amount, review rating, subscription status, shipping type, discount/promo usage, previous purchases, payment method, purchase frequency

> Note: this is a synthetic educational dataset, not proprietary company data. Treat findings as a methodology demonstration, not real market research — that caveat is stated here deliberately rather than left for someone else to catch.

## 3. Tools & Workflow

| Stage | Tool | What happened |
|---|---|---|
| Data cleaning & feature engineering | Python (pandas) | Handled 37 missing review ratings via category-median imputation, standardized column names, engineered `age_group` and `purchase_frequency_days`, identified and removed a fully redundant column (`promo_code_used` duplicated `discount_applied`) |
| Business analysis | SQL (T-SQL / SQL Server) | 10 business questions answered via aggregation, subqueries, window functions, and CTEs |
| Visualization | Power BI | Interactive dashboard with slicers for gender, category, subscription status, and shipping type |

Pipeline: `raw CSV → pandas cleaning → SQL Server → SQL analysis → Power BI dashboard`

## 4. Key Insights

- **Revenue is not gender-balanced.** Male customers generated $157,890 vs. $75,191 from female customers — worth investigating whether this reflects the customer base itself or a targeting gap in female-oriented categories.
- **Discounts don't reliably signal price sensitivity.** 839 customers used a discount *and* still spent above the average purchase amount — discounting is being used by customers who would likely have converted anyway, which is a margin leak, not a growth lever.
- **Subscribers underperform non-subscribers on revenue share.** Subscribers (1,053 customers) generated $62,645 vs. $170,436 from non-subscribers (2,847) — subscription isn't currently correlated with higher-value customers, so the current subscription program isn't doing its job as a loyalty lever.
- **Shipping speed barely moves basket size.** Express vs. Standard shipping average purchase amounts differ by about $2 ($60.48 vs $58.46) — not enough to justify shipping-speed-based promotions.
- **Category leaders are consistent:** Jewelry, Blouses, and Sandals top their respective categories in order volume, useful for inventory prioritization.

*(Full write-up with caveats on methodology limitations: see [`docs/insights_summary.md`](docs/insights_summary.md))*

## 5. Dashboard

![Dashboard Preview](https://github.com/ShaikSameerHussain12/customer-shopping-behavior-analysis/blob/main/customer_behavior_dashboard.png)

Interactive Power BI dashboard with filters for subscription status, gender, category, and shipping type. [Download the .pbix](https://github.com/ShaikSameerHussain12/customer-shopping-behavior-analysis/blob/main/Customer_behavior_Dashboard.pbix) to explore.

## 6. Business Recommendations

| Recommendation | Rationale |
|---|---|
| Audit discount policy | 839 customers spent above-average while discounted — discounting isn't targeting price-sensitive segments effectively |
| Redesign subscription incentives | Current subscribers don't outspend non-subscribers — the program needs a real value proposition, not just a status toggle |
| Rebuild segmentation on percentile-based thresholds | Fixed-number cutoffs (e.g., "10 previous purchases = loyal") produce distorted segments; recommend quartile-based segmentation instead |
| Prioritize top-rated, top-selling SKUs (Jewelry, Blouses, Sandals) in campaigns | Highest combination of volume and rating — lowest-risk marketing spend |

## 7. Limitations

- Dataset is synthetic; findings demonstrate methodology, not real market behavior.
- No time dimension in the raw data — "trend" analysis here is cross-sectional, not longitudinal.
- Segmentation logic in the original SQL used arbitrary fixed thresholds; see `docs/insights_summary.md` for the corrected percentile-based approach.

## Author

**Sameer** — Aspiring Data Analyst
[LinkedIn](https://www.linkedin.com/in/shaik-sameer-hussain-425a012bb/) 

