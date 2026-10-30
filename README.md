# E-Commerce Customer Analytics: RFM Segmentation & Cohort Retention (SQL)

> Customer segmentation (RFM) and monthly cohort retention analysis on ~100K real e-commerce orders, built with PostgreSQL.

**Tools:** PostgreSQL · (optional) Power BI / Looker Studio / Tableau for visualization
**Dataset:** [Olist Brazilian E-Commerce Public Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) (~100K orders, 2016–2018, real anonymized data)

## Business Questions
1. Who are the most valuable customers, and how much revenue do they contribute?
2. How well does the store retain customers after their first purchase?
3. Which segments deserve priority for marketing actions?

## Repository Structure
```
sql/
  01_schema_and_load.sql      -- DDL + CSV loading
  02_data_quality_checks.sql  -- data validation + clean base view
  03_rfm.sql                  -- RFM scoring + segmentation
  04_cohort_retention.sql     -- monthly cohorts + retention pivot
data/                         -- place CSV files here (not committed)
dashboard/                    -- dashboard screenshots / links
```

## Key Analytical Decisions
| Issue in the raw data | How it was handled |
|---|---|
| `customer_id` is unique per order, not per person | Used `customer_unique_id` as the true customer key |
| One order can have multiple payment rows | Aggregated payments per order before joining to avoid double counting |
| `canceled` / `unavailable` orders | Excluded |
| Most customers purchase only once, so NTILE on Frequency is meaningless | Rule-based Frequency score (1x / 2x / 3x+) |
| Earliest cohorts are tiny and the last months are incomplete | Excluded from conclusions |
| Snapshot date | `MAX(order_date) + 1`, not today's date |

## How to Run
1. Download the dataset from Kaggle and create a PostgreSQL database.
2. Run the files in `sql/` in order (01 → 04).
3. Connect the `rfm_segments` and `cohort_retention` tables to your BI tool.

## Results & Insights  *(fill in after running the queries; do not use made-up numbers)*
- **Repeat rate:** ___% of customers purchased more than once (query #6 in file 02).
- **Largest segment:** ___ (___% of customers, ___% of revenue).
- **Month-1 retention:** average ___%, dropping to ___% by month 3.
- **Key finding (example format):** "Most revenue comes from one-time buyers and retention is low, so the business priority is converting the second purchase, not just acquisition."

## Business Recommendations (example format)
| Segment | Recommended Action |
|---|---|
| Champions / Loyal | Loyalty program, early access to new products |
| New Customers / Promising | Second-purchase voucher within 30 days |
| At Risk | Win-back email/WhatsApp with an incentive |
| Hibernating High-Value | High-value reactivation campaign |

## Future Improvements
- Simple CLV per segment
- Delivery delay vs. retention analysis (join `order_reviews` and delivery dates)
- Cohorts by product category or state

## Author
**<YOUR NAME>** · [LinkedIn](https://linkedin.com/in/<username>) · [GitHub](https://github.com/<username>)
