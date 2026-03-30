# us-petroleum-energy-pipeline-March2026
# 🇺🇸 US Petroleum & Energy Data Pipeline

## 📌 Overview

This project is an end-to-end data engineering pipeline that analyzes U.S. energy and petroleum price trends, with a focus on **state-level price changes in March 2026**.

The pipeline ingests raw data from the U.S. Energy Information Administration API, processes it using a Medallion architecture on Databricks, and visualizes insights through interactive dashboards in Tableau Public.

---

## 🎯 Project Objective

* Analyze how **energy and petroleum prices changed across U.S. states**
* Focus on **March 2026 trends** and compare with prior months
* Provide **data-driven insights** with contextual awareness of global events (e.g., geopolitical tensions)

---

## 🧱 Architecture

```
EIA API → Bronze Layer → Silver Layer → Gold Layer → Tableau Dashboard
```

### Medallion Architecture:

* **Bronze** → Raw API ingestion (JSON)
* **Silver** → Cleaned and structured data
* **Gold** → Aggregated KPIs for analytics

📌 Architecture Diagram:
![Architecture](docs/architecture.png)

---

## ⚙️ Tech Stack

* **Data Source**: U.S. Energy Information Administration API
* **Processing**: Databricks (PySpark)
* **Development**: Visual Studio Code
* **Version Control**: GitHub
* **Visualization**: Tableau Public
* **AI Assistance**: GitHub Copilot

---

## 📂 Repository Structure

```
us-petroleum-energy-pipeline/
├── README.md
├── requirements.txt
├── notebooks/
│   ├── 01_bronze_ingest.py
│   ├── 02_silver_transform.py
│   ├── 03_gold_aggregates.py
│   └── 04_ai_insights.py
├── sql/
│   └── gold_kpi_queries.sql
├── workflows/
│   └── pipeline_job.yml
├── docs/
│   ├── architecture.png
│   └── dashboard_screenshot.png
└── data/
    └── sample/
```

---

## 🔄 Data Pipeline Flow

### 🥉 Bronze Layer — Data Ingestion

* Pulls raw data from EIA API
* Stores unprocessed data in Databricks tables

### 🥈 Silver Layer — Data Transformation

* Cleans nulls and formats data types
* Converts date fields
* Prepares structured dataset for analysis

### 🥇 Gold Layer — Aggregations

* Monthly average prices by state
* Trend analysis (Jan → March 2026)
* KPI-ready datasets for dashboards

---

## 📊 Key Insights (Example)

* Identify states with the **highest price increase in March 2026**
* Compare **month-over-month trends**
* Highlight **regional differences in energy pricing**

📌 Dashboard Preview:
![Dashboard](docs/dashboard_screenshot.png)

---

## 📈 Tableau Dashboard

The dashboard includes:

* 🗺️ Map of price changes by state
* 📉 Monthly trend lines
* 📊 Top states by price increase

👉 (Link to Tableau Public dashboard here)

---

## 🤖 AI-Assisted Development

This project leverages AI tools such as:

* GitHub Copilot for code generation
* AI-assisted insights generation on aggregated data

---

## ⏱️ Pipeline Orchestration

* Scheduled using Databricks workflows
* Incremental data processing design
* YAML-based job configuration

---

## 🚀 How to Run

1. Clone the repo:

```
git clone https://github.com/<your-username>/us-petroleum-energy-pipeline.git
```

2. Configure API key from EIA

3. Run pipeline:

* Bronze → Silver → Gold

4. Export Gold data → Tableau

---

## 🧠 Learnings

* Built a complete **Medallion data architecture**
* Integrated APIs into scalable pipelines
* Developed end-to-end workflow from ingestion → visualization
* Applied data storytelling to real-world energy trends

---

## ⚠️ Disclaimer

This project analyzes historical data trends.
Any relationship between price changes and geopolitical events is **interpretative, not causal**.

---

## 🙌 Future Improvements

* Add real-time streaming data
* Integrate more datasets (oil production, imports)
* Automate Tableau refresh
* Enhance AI-driven insights

---

## 📬 Contact

Feel free to connect or reach out for feedback!

---
