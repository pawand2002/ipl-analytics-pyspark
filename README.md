# 🏏 IPL Analytics with PySpark & Databricks

## 📌 Project Overview
This project demonstrates an **end‑to‑end PySpark data pipeline** using the IPL dataset (2008–2024).  
The primary objective is to **practice Spark optimization techniques** (joins, shuffles, AQE, Kryo, partitioning, Delta Lake tuning) while building a modular Lakehouse pipeline.

---

## 📂 Dataset
- **Source:** Kaggle IPL Complete Dataset (compiled from Cricsheet)  
- **Files:**
  - `matches.csv` → Match‑level data (teams, toss, winner, venue, player of match, etc.)  
  - `deliveries.csv` → Ball‑by‑ball data (batsman, bowler, runs, extras, dismissal info, etc.)  
- **Size:** ~27 MB deliveries, ~1 MB matches  

---

## ⚙️ Pipeline Steps

### 1. Bronze Layer
- Raw ingestion of CSV files into Databricks Volumes (`/Volumes/ipl_analytics/bronze/`).  
- Explicit schema definition for consistent data types.

### 2. Silver Layer
- Column normalization (`id → match_id`, `player_of_match → mom`, etc.).  
- Data quality checks:
  - Null handling  
  - Deduplication  
  - Constraints (e.g., runs ≥ 0)  
- Saved in Delta format, partitioned by season/match_id.

### 3. Gold Layer
- Transformations & aggregations:
  - Top batsmen per season  
  - Top bowlers per season  
  - Team win ratios by venue  
  - Venue scoring averages  
- Views saved in Delta for BI dashboards.

---

## ⚡ Optimization Practice

This project is designed to **practice Spark performance tuning**:

- **Join Strategies:** Inner, outer, semi, anti joins; broadcast joins.  
- **Wide vs Narrow Transformations:** GroupBy (wide) vs Filter/Select (narrow).  
- **Shuffle Partition Tuning:** Adjust `spark.sql.shuffle.partitions`.  
- **Adaptive Query Execution (AQE):** Handle skew dynamically.  
- **Salting Technique:** Manual skew mitigation.  
- **Serialization:** Kryo vs Java serializer.  
- **Delta Lake Optimizations:** `OPTIMIZE` and `ZORDER` for query speed.

---

## 📊 Power BI Dashboards

### 1. Top Batsmen per Season
- **Data Source:** `gold.top_batsmen_per_season`
- **Visuals:**
  - Bar chart → Runs scored by batsman, grouped by season  
  - Slicer → Season filter  
  - KPI card → Highlight top scorer each year  

### 2. Top Bowlers per Season
- **Data Source:** `gold.top_bowlers_per_season`
- **Visuals:**
  - Column chart → Wickets taken per bowler per season  
  - Line chart → Trend of wickets across seasons  

### 3. Team Win Ratio by Venue
- **Data Source:** `gold.team_win_ratio_by_venue`
- **Visuals:**
  - Heatmap → Venue vs Team, colored by win count  
  - Map → Stadium locations with win % bubble size  

### 4. Venue Scoring Averages
- **Data Source:** `gold.venue_runs`
- **Visuals:**
  - Line chart → Average runs per venue over seasons  
  - Drill‑through → Match‑level details  

---

## 🚀 How to Run
1. Upload `matches.csv` and `deliveries.csv` to `/Volumes/ipl_analytics/bronze/`.  
2. Run notebook step‑by‑step:
   - `IPL analytics project.ipynb`  
3. Use `.explain(True)` and job logs to observe shuffle vs non‑shuffle stages.  
4. Compare runtime before/after optimizations.

---

## 🎯 Learning Outcomes
- Understand Spark’s execution model (wide vs narrow transformations).  
- Practice optimization techniques with measurable impact.  
- Build a clean, modular Lakehouse pipeline in Databricks.  
- Prepare Gold layer views for BI tools while keeping focus on PySpark internals.  

---

## 📈 Before vs After Optimization Template
Use this section to log your experiments:

| Scenario                  | Baseline Runtime | Optimized Runtime | Shuffle Size | Technique Applied |
|---------------------------|------------------|-------------------|--------------|------------------|
| GroupBy runs per batsman  | 45s              | 18s               | 1.2 GB → 400 MB | Shuffle partitions = 50 |
| Join matches + deliveries | 60s              | 20s               | 2 GB → 0 GB | Broadcast join |
| Runs per team (skewed)    | 90s              | 30s               | 3 GB → 1 GB | AQE + salting |

---

## 🧑‍💻 Author
Built by **Pawan Dubey** for practicing PySpark optimizations and showcasing IPL analytics in Databricks Free Edition.
