# 🏗️ Data Lakehouse on Databricks

A minimal, technical implementation of a **Lakehouse architecture** using Databricks, structured into **Bronze, Silver, and Gold** layers and developed as part of the *Data with Baraa – Databricks Bootcamp*.
This is a WIP and new changes will be commited shortly.

## 📂 Project Structure
```
datawithbaraa_databricks_bootcamp/
├── bronze/              # Raw data ingestion (Delta tables)
├── silver/              # Cleansed & standardized transformations
│   ├── crm/             # CRM domain transformations
│   └── erp/             # ERP domain transformations
└── gold/ (TBD)          # Curated analytical models
```

## 🔧 Tech Stack
- Databricks Lakehouse Platform
- Apache Spark / PySpark
- Delta Lake
- Python
- Git & GitHub

## 🔄 Pipeline Overview
- **Bronze:** Load raw datasets into Delta format.
- **Silver:** Apply cleaning, normalization, schema enforcement, and deduplication.
- **Gold:** Produce optimized, analytics‑ready tables for BI and downstream consumption.

## 📌 Next Steps
- Create Gold Layer with data modeling and business rules.
- Add workflow orchestration (Databricks Jobs).
- Implement data quality checks.
