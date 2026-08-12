# Online Retail Sales Performance Dashboard

## Live Dashboard
🔗 [View interactive dashboard on Tableau Public](https://public.tableau.com/app/profile/rahul.kulkarni6129/viz/dataanalystproject_17865305719790/OnlineRetailSalesPerformanceDashboard2009-2011)

## Business Question
How is revenue trending over time, which products and regions drive the most value, and where are the opportunities to grow average order value?

## Dataset
[Online Retail II](https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci) — UK-based online gift-ware retailer, Dec 2009–Dec 2011, 1,067,371 raw transaction rows. Built on a cleaned version of the dataset (804,824 transactions, 5,855 customers); cleaning logic and decisions (missing Customer IDs, cancelled orders, administrative codes, a duplicate-row investigation) are documented in `cleaning_script.py` and in the companion project, [customer-segmentation-retention-analysis](https://github.com/Rahulkulkarni2002/customer-segmentation-retention-analysis).

## Tools & Skills Demonstrated
- **SQL** (via DuckDB): aggregation, `GROUP BY`, `DATE_TRUNC`, subqueries, filtering on raw transaction data
- **Python/pandas**: derived metrics (`.pct_change()` for growth), multi-level `.groupby().agg()`, boolean filtering, CSV export pipelines
- **Data storytelling**: every metric is paired with a business interpretation, not reported as a bare number
- **Tableau Public**: interactive dashboard design, filtering, sorting, and layout for a non-technical audience

## Methodology
Each KPI was built in three steps: (1) SQL aggregation against the cleaned dataset, (2) a pandas-based derived calculation where relevant (growth %, per-invoice averages), and (3) a sanity check against known context before being trusted — several initial results led to further investigation rather than being reported at face value (see Key Findings below).

## Key Findings

**1. Strong, repeating seasonality.** Revenue climbs sharply every September–November (holiday buildup) and drops sharply every January (post-holiday slump) — the same pattern independently in both 2010 and 2011, ruling out a one-time cause like a single marketing campaign or data anomaly.

**2. Revenue and volume tell different stories.** The top product by revenue (REGENCY CAKESTAND 3 TIER, ~$11.50/unit, $286K total) doesn't appear in the top 10 by quantity sold. The top product by quantity (WORLD WAR 2 GLIDERS, 109,169 units) sells for ~$0.23/unit. A retailer needs both rankings — one for margin-driving merchandising decisions, one for volume/basket-size drivers — reporting only one would miss half the picture.

**3. The UK drives volume, not all of the value.** The UK accounts for 92% of transactions but only 83.3% of revenue. Investigating the gap traced it to a small number of high-value non-UK accounts — e.g., EIRE has just 3 customers averaging $200,220 each, well above the dataset's median customer spend of under $900. This is consistent with wholesale/B2B account patterns identified in the companion segmentation project, and the same accounts likely distort country-level revenue and AOV figures the same way they distort individual customer Monetary values.

**4. December shows fewer but larger orders.** Average order value spikes in both Decembers (~$630–670 vs. a typical $430–540 range) despite the final month of data being incomplete. This isn't a contradiction of finding #1 — total revenue and average order value measure different things — and is consistent with last-minute holiday shoppers bundling multiple gifts into a single larger order.

**5. Small sample sizes can mislead country-level metrics.** Several countries' average order values were based on 1–2 orders (e.g., Lebanon: $1,693.88 from a single order, Thailand: $1,535.27 from 2 orders) — not statistically meaningful. These were excluded from the country-level AOV comparison using a 20+ order minimum threshold, applied and documented before the figures were used anywhere further.

## Data Quality Note
The dataset's transaction records end 2011-12-09, so December 2011 reflects only 9 days rather than a full month (~30 days elsewhere). Comparing it directly against complete months would show a misleading apparent decline. It is excluded from monthly trend analysis and growth calculations, with the exclusion noted directly on the dashboard rather than silently dropped.

## Files
- `sales_kpi_analysis.ipynb` — full analysis notebook (SQL + pandas), with markdown commentary on every finding
- `cleaning_script.py` — data cleaning logic (shared with the companion segmentation project)
- `monthly_revenue.csv`, `top_products_revenue.csv`, `top_products_quantity.csv`, `country_revenue.csv`, `aov_by_country.csv` — exported, analysis-ready tables feeding the dashboard

## How to Reproduce
1. Clone this repo
2. Download the [Online Retail II dataset](https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci) and place it in the project folder as `online_retail_II.csv`
3. Run `cleaning_script.py` to produce `cleaned_retail.csv`
4. Open `sales_kpi_analysis.ipynb` and run all cells to reproduce the analysis and CSV exports
5. Open the exported CSVs in Tableau Public (or Power BI) to rebuild the dashboard

## Resume Summary
Built a SQL and Python-driven sales analytics project analyzing 800K+ retail transactions — surfaced seasonal revenue trends, product performance discrepancies between revenue and volume, country-level revenue/AOV patterns, and data quality issues (partial-month bias, small-sample distortion), delivered via a published interactive Tableau dashboard.
