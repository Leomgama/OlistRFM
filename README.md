# Olist RFM Customer Segmentation

**Author:** Leonardo Gama
**Tools:** Python, Pandas, NumPy, Matplotlib, Seaborn, Scikit-learn, Jupyter
**Dataset:** [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

---

## Project Overview

This project applies **RFM (Recency, Frequency, Monetary) analysis** to ~96,000 delivered orders from Olist, a Brazilian e-commerce marketplace platform, segmenting ~93,000 unique customers into actionable groups for marketing prioritization. The analysis goes beyond basic RFM scoring to include unsupervised clustering, customer lifetime value ranking, cohort retention, geographic distribution, category mix, and quantified business opportunity sizing.

---

## Headline Result

**R$ 1.88M of addressable marketing opportunity identified** (~12% of total delivered revenue) across 11 customer segments, each tied to a specific marketing strategy and conservative conversion-rate assumption.

| Metric | Value |
|---|---|
| Delivered orders analyzed | 96,477 |
| Unique customers | 93,357 |
| Total delivered revenue | R$ 15,422,462 |
| Date range | 2016-10-03 → 2018-08-29 |
| Repeat-buyer rate | 3.0% |
| Total addressable opportunity | **R$ 1,884,820** |

---

## Key Findings

1. **R$ 3.16M of revenue is "At Risk"** — customers who spent ~R$ 438 on average but haven't returned in ~13 months. Even a conservative 5% win-back recovers ~R$ 158K with a single targeted campaign.
2. **The largest single play is converting Potential Loyalist into repeat buyers (R$ 634K)** — 7,232 customers who bought ~3 months ago at R$ 439 average ticket; warm and ready for a second-purchase nudge.
3. **Champions are 0.8% of customers but generate the highest per-customer value (R$ 452 avg ticket, F = 2.25)** — VIP retention is the most efficient marketing spend per real.
4. **Geographic concentration is extreme** — top 5 states (SP, RJ, MG, RS, PR) hold 73% of revenue.
5. **97% of customers are single-purchase** — Olist's retention engine is the biggest untapped lever.

---

## Methodology

### Data Preparation
- Filtered orders to `order_status == 'delivered'` (drops ~3% canceled/unavailable)
- Joined `customer_id` to `customer_unique_id` to fix the silent Frequency-collapse bug present in the GeeksforGeeks reference tutorial
- Aggregated split-payment rows in `order_payments` to one total per `order_id`

### Two Scoring Methods, Side by Side
- **Method A — Rank-weighted RFM score** (per the GeeksforGeeks reference): rank each dimension, normalize 0–100, weighted average (15% R, 28% F, 57% M), scaled to 0–5
- **Method B — Quintile R/F/M with the 11 standard segments** (industry standard): Recency and Monetary as quintiles via `pd.qcut`, Frequency via custom bins (because 97% of customers have F=1), then a (R × FM) rule grid assigns one of 11 named segments (Champions, Loyal, At Risk, Lost, etc.)

### Enhanced Analyses
- **KMeans (k=4)** on log-transformed and standardized R/F/M as a sanity check against the rule-based segmentation
- **CLV proxy** — Avg Order Value × Frequency × Recency-decay factor — ranks individual customers
- **Cohort retention heatmap** — % of each acquisition cohort active in subsequent months
- **Geographic distribution** — segment composition across Brazilian states
- **Category mix per segment** — what each segment buys most

### Business Recommendations
Each segment was assigned a strategy (retain, convert, win-back, deprioritize), a conservative conversion-rate assumption, and an R$ opportunity calculation. Total addressable opportunity = R$ 1.88M.

---

## Repository Structure

```
.
├── OLIST_RFM_v2.ipynb     # Main notebook — end-to-end pipeline
├── README.md              # This file
├── requirements.txt       # Python dependencies
├── .gitignore
└── Dataset/               # NOT in repo — download from Kaggle (see below)
```

---

## How to Reproduce

### 1. Clone the repo

```bash
git clone https://github.com/Leomgama/OlistRFM.git
cd OlistRFM
```

### 2. Download the dataset

The Olist dataset is hosted on Kaggle and is not included in this repo. Download it from:
**https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce**

Extract the 9 CSV files into a `Dataset/` folder at the project root:

```
OlistRFM/
└── Dataset/
    ├── olist_customers_dataset.csv
    ├── olist_geolocation_dataset.csv
    ├── olist_orders_dataset.csv
    ├── olist_order_items_dataset.csv
    ├── olist_order_payments_dataset.csv
    ├── olist_order_reviews_dataset.csv
    ├── olist_products_dataset.csv
    ├── olist_sellers_dataset.csv
    └── product_category_name_translation.csv
```

### 3. Set up the Python environment

```bash
python -m venv venv_olist
# Activate it:
#   Windows:  venv_olist\Scripts\activate
#   macOS/Linux: source venv_olist/bin/activate

pip install -r requirements.txt
```

### 4. Run the notebook

```bash
jupyter notebook OLIST_RFM_v2.ipynb
```

Then run all cells (Cell → Run All). The notebook executes end-to-end without errors and exports `olist_rfm_scored.csv` at the end.

---

## Acknowledgments

- Methodology reference: [RFM Analysis Using Python — GeeksforGeeks](https://www.geeksforgeeks.org/data-analysis/rfm-analysis-analysis-using-python/)
- Dataset: [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) (CC BY-NC-SA 4.0)
- 11-segment RFM framework: standard across Putler, Optimove, Mailchimp segmentation literature

---

*Last updated: May 2026*
