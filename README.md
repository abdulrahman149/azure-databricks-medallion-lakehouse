# Azure Databricks End-to-End Medallion Lakehouse

## 📌 Project Overview
This project demonstrates a production-ready data engineering pipeline using the Medallion architecture (Bronze, Silver, Gold) on Azure Databricks. It processes the public NYC Taxi dataset from raw ingestion to business-level aggregations while enforcing strict data quality and enterprise-grade data governance.

## 🏗️ Architecture & Tech Stack
* **Cloud Infrastructure:** Azure Data Lake Storage Gen2 (ADLS Gen2)
* **Compute & Orchestration:** Databricks Serverless Compute, Databricks Workflows
* **Data Processing:** Apache Spark (PySpark), Spark Structured Streaming, Delta Lake
* **Governance & Security:** Unity Catalog, Azure Managed Identities (Access Connector)

## 🚀 Pipeline Lifecycle
1. **Bronze (Raw Data):** 
   * Incrementally ingests raw Parquet files from an ADLS Gen2 external volume using **Databricks Auto Loader** (`cloudFiles`). 
   * Captures audit metadata (`_ingestion_time`, `_source_file`) and utilizes checkpointing for idempotent processing.
2. **Silver (Cleansed Data):**
   * Uses PySpark Structured Streaming with `.trigger(availableNow=True)` to process only new records.
   * Enforces data quality rules (removing negative distances, zero passengers, and null timestamps).
3. **Gold (Business Aggregations):**
   * Transforms cleansed data into business-ready KPIs (Daily Revenue, Passenger Volumes, Payment Type Summaries).
4. **Governance:**
   * Implements fine-grained access control using **Unity Catalog**.
   * Applies dynamic **Row-Level Security (RLS)** to restrict viewable regions/payment types based on user groups.
   * Uses **Dynamic Data Masking** to obfuscate sensitive financial columns for non-privileged users.

## 📊 Data Lineage
The pipeline's lineage is automatically captured and governed by Unity Catalog, providing full observability from source volume to curated dashboard aggregations.

## ⚙️ How to Run
1. Provision an Azure Databricks Workspace (Premium Tier) and ADLS Gen2 storage.
2. Configure a Unity Catalog Metastore and link the storage via an Azure Managed Identity.
3. Import the notebooks in numerical order (`01` through `04`).
4. Execute notebooks `01` - `03` via a Databricks Workflow DAG.
5. Run notebook `04` via SQL Editor to apply security UDFs.
