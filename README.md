# End-to-End Sales Data Mart (Medallion Architecture)

**Tech Stack:** Databricks | PySpark | AWS S3 | Delta Lake | Medallion Architecture

## 📌 Project Overview
This project demonstrates an end-to-end data pipeline built on **Databricks** and **AWS S3**, following the **Medallion Architecture** (Bronze -> Silver -> Gold). 

The pipeline ingests raw OLTP data, processes it through staging and transformation layers, and delivers a highly optimized **Gold-layer Star Schema** ready for BI reporting, dashboards, and analytics. The pipeline is designed to handle both initial full loads and subsequent incremental loads.

## 🏗️ Architecture & Data Flow
The data flows from an external OLTP system into AWS S3, where Databricks (PySpark) processes it through the medallion layers.

*(Drag and drop your first image—the architecture diagram—right here)*

**Pipeline Execution Details:**
*   **Ingestion:** Raw files land in the AWS S3 `landing` folder. The pipeline reads these files, and upon successful processing into the Bronze layer, automatically moves the source files to a `processed` (archive) folder to prevent duplicate ingestion.
*   **Initial Load:** The pipeline is designed to handle a full historical load on the first run.
*   **Incremental Load:** Subsequent runs are structured to process only new, incoming data using incremental loading strategies, ensuring efficient compute usage.
*   **Staging Layer:** Temporary staging tables are created prior to the final Silver and Gold layer transformations to ensure data quality, deduplication, and schema enforcement.

## 📊 Data Model (Gold Layer)
To optimize query performance for the serving layer (Dashboards/Genie), the Gold layer is modeled as a **Star Schema**. It consists of a central fact table surrounded by conformed dimension tables.

*(Drag and drop your second image—the Star Schema—right here)*

*   **Fact Table:** `fact_orders` (Contains quantitative metrics like `order_qty` and foreign keys to dimensions).
*   **Dimension Tables:** 
    *   `dim_customers` (Customer demographics)
    *   `dim_products` (Product catalog details)
    *   `dim_product_pricing` (Historical pricing data)
    *   `dim_date` (Time intelligence for reporting)

## 📂 Repository Structure
The notebooks are designed to be run sequentially to build the pipeline:

*   `1_setup/` - Notebooks for initializing the Databricks environment, catalogs, and AWS S3 connections.
*   `2_dimension_data_processing/` - PySpark notebooks handling the transformation, staging, and loading of dimension tables into the Gold layer.
*   `3_fact_data_processing/` - PySpark notebooks handling the aggregation, staging, and loading of the central fact table.

## 🚀 How to Run This Project
1. Clone this repository into your Databricks workspace using the **Git Folder** integration.
2. Ensure you have an AWS S3 bucket configured with a `landing` folder containing the raw source data, and an empty `processed` folder for archiving.
3. Configure your Databricks Secrets for AWS credentials (do not hardcode keys!).
4. Run the notebooks sequentially starting from `1_setup` through `3_fact_data_processing`.

## 🔒 Security Note
No sensitive credentials (AWS Keys, Databricks Tokens) are stored in this repository. All credentials are handled securely via Databricks Secrets.