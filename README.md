# 🏗️ Data Lakehouse on Databricks

A complete, professional implementation of a **Lakehouse architecture** using Databricks, structured into **Bronze, Silver, and Gold** layers and developed as part of the *Data with Baraa - Databricks Bootcamp*. The project processes bike sales data and includes a fully orchestrated ETL pipeline triggered automatically when new files arrive in the raw volume.

---

## 📂 Project Structure

```
datawithbaraa_databricks_bootcamp/
├── bronze/                  # Raw data ingestion (Delta tables)
├── silver/                  # Clean & standardized transformations
│   ├── crm/                 # CRM domain transformations
│   └── erp/                 # ERP domain transformations
├── gold/                    # Curated analytical models
└── init_lakehouse.ipynb     # Initial lakehouse configurations
```

---

## 🔧 Tech Stack
- Databricks Lakehouse Platform
- Apache Spark / PySpark
- Delta Lake
- Python
- Git & GitHub

---

## 🔄 Pipeline Overview
- **Bronze:** Load raw datasets into Delta format.
- **Silver:** Apply cleaning, normalization, schema enforcement, and deduplication.
- **Gold:** Produce optimized, analytics-ready tables for BI and downstream consumption.

---

## ⚙️ Lakehouse Initialization (SQL)
```
%sql
USE CATALOG workspace;

CREATE SCHEMA IF NOT EXISTS bronze
COMMENT 'Bronze layer: raw ingested data';

CREATE SCHEMA IF NOT EXISTS silver
COMMENT 'Silver layer: cleaned and transformed data';

CREATE SCHEMA IF NOT EXISTS gold
COMMENT 'Gold layer: business-ready data';

CREATE VOLUME IF NOT EXISTS workspace.bronze.source_system
COMMENT 'Volume for raw source files (CSV)';
```


---

# 1️⃣ Architecture Diagram
```
mermaid
flowchart TD
    A[Init Lakehouse] --> B[Bronze Layer]

    B --> C1[Silver CRM - cust_info]
    B --> C2[Silver CRM - prd_info]
    B --> C3[Silver CRM - sales_details]
    B --> E1[Silver ERP - cust_act12]
    B --> E2[Silver ERP - loc_a101]
    B --> E3[Silver ERP - px_cat_g1v2]

    C1 --> S[Silver Sanity Checks]
    C2 --> S
    C3 --> S
    E1 --> S
    E2 --> S
    E3 --> S

    S --> G1[Gold Dim Customers]
    S --> G2[Gold Dim Products]
    S --> G3[Gold Fact Sales]

    G1 --> GSC[Gold Sanity Checks]
    G2 --> GSC
    G3 --> GSC
```

---

# 2️⃣ Job Orchestration Overview
The ETL pipeline is orchestrated using a Databricks Job composed of multiple serverless tasks, optimized for parallel execution where possible.

#### Pipeline flow with multiprocessing
To maximize performance, Silver and Gold transformations run in parallel, coordinated by validation checkpoints:

1. **init_lakehouse**  
   Prepares schemas, volumes, and catalog.

2. **bronze_layer**  
   Ingests raw CSV files into Delta tables.

3. **Parallel Silver tasks**
   - silver_crm_cust_info
   - silver_crm_prd_info
   - silver_crm_sales_details
   - silver_erp_cust_act12
   - silver_erp_loc_a101
   - silver_erp_px_cat_g1v2

4. **silver_sanity_checks**  
   Validates that all expected Silver tables were successfully created.  
   This step ensures multiprocessing did not skip or fail silently.

5. **Parallel Gold tasks**
   - gold_dim_customers
   - gold_dim_products
   - gold_fact_sales

6. **gold_sanity_checks**  
   Ensures all required Gold tables exist before downstream consumption.

**Job features**
- Serverless execution
- Multiprocessing for Silver and Gold layers
- Validation checkpoints to guarantee pipeline integrity
- Automatic triggering when new files arrive
- Email notifications on success or failure
- Centralized logging and monitoring

### **Tasks included:**
- `init_lakehouse` - prepares schemas, volumes, and catalog
- `bronze_layer` - ingests raw CSV files into Delta tables
- `silver_crm_*` - transforms CRM domain datasets
- `silver_erp_*` - transforms ERP domain datasets
- `silver_sanity_checks` - validates Silver layer integrity
- `gold_dim_*` - produces final analytical models
- `gold_sanity_checks` - validates Gold layer integrity

### **Job features:**
- Fully serverless execution
- Task dependencies ensuring correct order
- Centralized logging and monitoring
- Email notifications on success or failure

---

# 3️⃣ Triggering Mechanism
The job is automatically triggered when new files are added to the raw volume:

```workspace.bronze.raw_sources```

This enables near-real-time ingestion and processing without manual intervention.

---

## 🚀 Execution
1. Import notebooks into Databricks.
2. Attach to a DBR 14+ cluster.
3. Run notebooks in order: **Bronze → Silver → Silver Sanity Checks → Gold → Gold Sanity Checks**.
4. Or rely on the automated job trigger for full orchestration.

---

## 📌 Summary
This repository implements a production-ready Lakehouse with:
- Multi-layer Delta architecture
- Domain-oriented Silver transformations
- Automated orchestration with triggers and notifications
- A clear, scalable structure for future enhancements
