# 50 Years of US Petroleum Markets — EIA Data Pipeline

End-to-end data engineering + analytics project using the EIA public API,
Databricks Delta Lake (Bronze/Silver/Gold medallion architecture), and Tableau Public.

## Project narrative
The 2026 Iran conflict pushed Brent crude up ~50% from January to March 2026
as Strait of Hormuz shipments fell. This project tracks 50 years of EIA weekly
petroleum data to put that spike in full historical context — alongside the 1991
Gulf War, 2008 financial crisis, and 2020 COVID collapse.

## Tech stack
PySpark | Delta Lake (Bronze/Silver/Gold) | Databricks Community Edition | Tableau Public

## Notebooks
01_bronze_ingest — Fetch 7 EIA series via /v2/seriesid/ API with pagination
02_silver_clean  — Clean, type-cast, join, add era labels + conflict flag
03_gold_kpis     — KPI aggregates, era averages, CSV export for Tableau

## Dashboard
Live Tableau Public dashboard: [ADD YOUR TABLEAU PUBLIC LINK HERE]

## How to run
1. Get a free EIA API key: https://www.eia.gov/opendata/register.php
2. Sign up for free Databricks Community Edition
3. Clone this repo into Databricks Repos
4. Run notebooks in order: 01 -> 02 -> 03
5. Download gold CSVs and connect to Tableau Public
