# 50 Years of US Petroleum Markets — EIA Data Pipeline

End-to-end data engineering and analytics portfolio project using the EIA public API,
Databricks Delta Lake (Bronze → Silver → Gold medallion architecture), Unity Catalog Volumes
for governed file export, and Tableau Public for visualisation.

---

## Project narrative

The 2026 Iran conflict pushed Brent crude up ~50% from January to March 2026 as Strait of
Hormuz shipments fell. This project tracks 50 years of EIA weekly petroleum data to put that
spike in full historical context — alongside the 1991 Gulf War, 2008 financial crisis, and
2020 COVID collapse. Every data point is labelled with its historical era so the dashboard
tells the full story at a glance.

---

## Tech stack

| Layer | Technology |
|---|---|
| Ingestion | EIA Public API (`/v2/seriesid/` endpoint) with pagination |
| Processing | Apache PySpark on Databricks (Delta Lake, medallion architecture) |
| Storage | Unity Catalog managed Delta tables + Unity Catalog Volumes (CSV export) |
| Orchestration | Databricks Jobs — 3-task DAG: Bronze → Silver → Gold (weekly schedule) |
| Visualisation | Tableau Public |
| Version control | GitHub via Databricks Repos |

---

## Medallion architecture

```
EIA API
   │
   ▼
[Bronze] — Raw Delta tables, full history ~1982–present, 7 series, ~2,000 rows each
   │         petroleum_bronze.raw_wti_price
   │         petroleum_bronze.raw_brent_price
   │         petroleum_bronze.raw_us_production
   │         petroleum_bronze.raw_crude_stocks
   │         petroleum_bronze.raw_product_supplied
   │         petroleum_bronze.raw_gasoline_price
   │         petroleum_bronze.raw_diesel_price
   │
   ▼
[Silver] — Cleaned, typed, joined, enriched with era labels + conflict flag
   │         petroleum_silver.petroleum_weekly
   │
   ▼
[Gold]  — KPI aggregates + era averages + CSV export to Unity Catalog Volume
            petroleum_gold.petroleum_weekly_fact
            petroleum_gold.era_averages
            petroleum_gold.headline_kpis
            /Volumes/petroleum_gold/exports/csv_exports/  (Tableau CSVs)
```

---

## Notebooks

| Notebook | Layer | Description |
|---|---|---|
| `Bronze_Ingest` | Bronze | Fetch 7 EIA series via `/v2/seriesid/` API with pagination. Full history, no date filter. |
| `Silver_Clean` | Silver | Cast types, join all series, add era labels, conflict flag, rolling calculations (WoW, 4wk avg). |
| `Gold_Kpis` | Gold | Aggregate KPIs, era averages, export CSVs to Unity Catalog Volume `/Volumes/petroleum_gold/exports/csv_exports/`. |

---

## Databricks Job — automated weekly pipeline

The three notebooks are orchestrated as a single Databricks Job with task dependencies:

```
bronze_ingest  ──►  silver_clean  ──►  gold_kpis
```

**Schedule:** Every Wednesday at 11:30 AM Central Time (CRON: `30 11 * * 3`)
EIA publishes new weekly petroleum data every Wednesday around 9:30 AM Central — the job
runs 2 hours later to ensure the API has the latest data available.

**To run manually:** Go to Jobs & Pipelines in the Databricks sidebar → select the job →
click Run Now.

---

## EIA API series used

All series use the `/v2/seriesid/` endpoint which accepts confirmed v1-format IDs.
No facet configuration needed — the endpoint handles routing internally.

| Series ID | Description |
|---|---|
| `PET.RWTC.W` | WTI Crude Oil Spot Price ($/barrel) |
| `PET.RBRTE.W` | Brent Crude Oil Spot Price ($/barrel) |
| `PET.WCRFPUS2.W` | US Weekly Crude Oil Production (Mbbl/day) |
| `PET.WCRSTUS1.W` | US Crude Oil Stocks (Million barrels) |
| `PET.WGFUPUS2.W` | US Weekly Product Supplied / Consumption (Mbbl/day) |
| `PET.EMM_EPMR_PTE_NUS_DPG.W` | US Gasoline Retail Price ($/gallon) |
| `PET.EMD_EPD2D_PTE_NUS_DPG.W` | US Diesel Retail Price ($/gallon) |

---

## Historical era labels (Silver layer)

Each row in the Silver and Gold tables is labelled with its historical context:

| Era label | Date range |
|---|---|
| 1973–1985 Oil Shock Era | Before Jan 1986 |
| 1986–1991 Glut & Recovery | Jan 1986 – Jul 1991 |
| 1991 Gulf War | Aug – Dec 1991 |
| 1992–2007 Stable Growth | Jan 1992 – Dec 2007 |
| 2008 Financial Crisis | Jan 2008 – Dec 2009 |
| 2010–2019 Shale Boom | Jan 2010 – Dec 2019 |
| 2020 COVID Collapse | Jan 2020 – Dec 2021 |
| 2022–2025 Post-COVID | Jan 2022 – Jan 4 2026 |
| 2026 Iran Conflict | Jan 5 2026 onwards |

---

## Key KPIs in the Gold layer

- Current WTI and Brent prices ($/barrel)
- WTI % change since January 2026 (conflict impact)
- WTI vs Brent spread (market stress indicator)
- 4-week rolling average WTI (smoothed trend)
- Crude stocks week-over-week change (supply signal)
- All-time WTI high and 2020 COVID low (historical context)
- Gasoline and diesel retail prices (consumer impact)
- Era average WTI price comparison table (50-year context)

---

## Dashboard

**Live Tableau Public dashboard:** [ADD YOUR TABLEAU PUBLIC LINK HERE]

The dashboard covers:
- 50-year WTI price timeline with era shading and conflict reference band
- Era average price comparison bar chart
- Consumer impact: gasoline and diesel retail prices
- US crude production and stocks trend
- 4 headline KPI tiles

---

## How to run

### Prerequisites
1. Free EIA API key: [https://www.eia.gov/opendata/register.php](https://www.eia.gov/opendata/register.php)
2. Free Databricks account: [https://community.cloud.databricks.com](https://community.cloud.databricks.com)

### Setup
1. Clone this repo into Databricks Repos (Settings → Linked Accounts → add GitHub token → Repos → Add Repo)
2. Create a cluster (Runtime 15.4 LTS or higher)
3. Open `Bronze_Ingest` and replace `YOUR_EIA_API_KEY` with your actual key

### Run order
```
Bronze_Ingest  →  Silver_Clean  →  Gold_KPIs
```
Each notebook validates its output at the end — check the printed summary before running the next step.

### Download CSVs for Tableau
After `Gold_KPIs` runs:
1. Go to **Catalog** in the Databricks sidebar
2. Navigate to `petroleum_gold` → `exports` → `csv_exports`
3. Click **Browse files** → open the export folder
4. Click the `part-00000-*.csv` file → **Download**
5. Rename to `petroleum_gold_weekly.csv` and upload to Tableau Public

### Set up the automated job
1. Go to **Jobs & Pipelines** in the sidebar → **Create Job**
2. Add three tasks: `bronze_ingest` → `silver_clean` → `gold_kpis` with task dependencies
3. Set schedule: Weekly, Wednesday, 11:30 AM, US/Central timezone
4. Click **Run Now** to test — all three tasks should turn green

---

## API notes

**Why `/v2/seriesid/` instead of the route-based endpoints?**

The route-based endpoints (e.g. `petroleum/supply/weekly/data/`) require knowing the exact
facet key names for each route (`duoarea`, `product`, `series` etc.) which vary per endpoint
and cause 400 errors when wrong. The `/v2/seriesid/` endpoint accepts the classic v1 format
series IDs directly and handles routing internally — no facet configuration needed.

**Why ~2,000 rows per series?**

No date filter is applied on fetch — the full history from ~1982 to present is retrieved.
This gives richer context for the 50-year dashboard narrative. Pagination is implemented
with an `offset` loop to ensure all rows are captured regardless of history length.

---

## Project structure

```
petroleum-analytics-portfolio/
├── notebooks/
│   ├── Bronze_Ingest.py    # EIA API → Bronze Delta tables
│   ├── Silver_Clean.py     # Clean, join, era labels → Silver Delta table
│   └── Gold_KPIs.py        # KPI aggregation → Gold tables + Volume CSV export
└── README.md
```

---

*Built by Prarthna Bisht | Houston, TX | [linkedin.com/in/prarthna-bisht](https://www.linkedin.com/in/prarthna-bisht/)*
<img width="468" height="644" alt="image" src="https://github.com/user-attachments/assets/56c4b53a-1c3d-4282-aa39-8cabfb724069" />
