# Insights Summary & Methodology Notes

This document expands on the README with the reasoning behind each insight and flags where the original analysis needed correction — because a Big Four interviewer will ask "why," not just "what."

## 1. Revenue by Gender
Male customers: $157,890 | Female customers: $75,191.
This is a ~2:1 split. Before recommending action, this needs to be checked against *customer count* by gender, not just revenue — if the male base is proportionally larger, the per-customer gap may be small or nonexistent. (Add a `revenue / customer_count` query before presenting this as a targeting insight.)

## 2. Discount Usage vs. Spend
839 customers (out of 3,900) used a discount and still spent above the dataset average purchase amount (~$59.76). That's about 21% of the base — discounting is reaching a meaningful chunk of customers who weren't price-constrained. This is the strongest, most defensible insight in the analysis because it's a direct, unambiguous comparison.

## 3. Subscription Status vs. Spend
Subscribers: 1,053 customers, $62,645 total revenue, $59.49 avg spend.
Non-subscribers: 2,847 customers, $170,436 total revenue, $59.87 avg spend.
Average spend is nearly identical between the two groups. The revenue gap is purely a function of subscriber count, not subscriber value. This means "subscription status" is not currently a useful proxy for customer value in this dataset — worth stating explicitly rather than implying subscribers are more valuable.

## 4. Segmentation — Corrected Version
**Original logic (flawed):**
```sql
CASE WHEN previous_purchases = 1 THEN 'NEW' 
WHEN previous_purchases BETWEEN 2 AND 10 THEN 'RUNNING' 
ELSE 'LOYAL' END
```
Because `previous_purchases` is roughly uniform across 1–50, fixed thresholds push ~80% of customers into "Loyal" regardless of actual behavior. That's a distribution artifact, not a business insight.

**Corrected version — percentile-based segmentation:**
```sql
WITH ranked_customers AS (
    SELECT customer_id, previous_purchases,
           NTILE(3) OVER (ORDER BY previous_purchases) AS segment_tier
    FROM customer_shopping_behavior
)
SELECT 
    CASE segment_tier 
        WHEN 1 THEN 'New' 
        WHEN 2 THEN 'Running' 
        WHEN 3 THEN 'Loyal' 
    END AS customer_segment,
    COUNT(*) AS number_of_customers
FROM ranked_customers
GROUP BY segment_tier;
```
This splits customers into three *equal-sized* groups based on their actual rank in the distribution, which gives a segmentation that's meaningful regardless of how `previous_purchases` happens to be distributed.

## 5. Shipping Type
Standard: $58.46 avg | Express: $60.48 avg — a ~3% difference. Not statistically or practically meaningful on its own; would need a significance test (t-test) before making any claim here. Flagged as a weak insight rather than dressed up as a strong one.

## Known Limitations
- Synthetic dataset — no real seasonality, macro, or competitor context.
- No timestamp field, so nothing here is a true "trend" (time-series) — it's a cross-sectional snapshot.
- Sample size per subgroup (e.g., per product per category) is small enough that some rankings (like "top rated product") could shift with small data changes — worth noting confidence is directional, not definitive.
