# 📊 VietDist Sales Performance & Distribution Analytics Platform
### End-to-End ETL Pipeline using Python, PostgreSQL & Power BI

**Author:** Trần Quốc Hưng

**Project Type:** Data Engineering | Analytics Engineering | ETL Pipeline

**Tools Used**

- Python
- Pandas
- SQLAlchemy
- PostgreSQL
- pgAdmin
- Power BI Desktop
- Git & GitHub

---

# 📑 Table of Contents

- Background & Overview
- Business Objective
- Project Scope
- System Architecture
- Technology Stack
- Project Structure
- ETL Pipeline Overview
- Environment Setup
- PostgreSQL Setup
- Running the Pipeline

---

# 📌 Background & Overview

Modern distribution companies generate business data every day from different departments such as Sales, Products, Customers, Employees and Sales Targets.

Although the data is available, it is often stored in multiple Excel workbooks with different structures and formats. This creates several challenges:

- Manual reporting consumes significant time.
- Different departments maintain inconsistent file structures.
- Historical versions are difficult to manage.
- Business users cannot analyze data efficiently.
- Power BI reports depend heavily on manual preprocessing.

To solve these problems, this project develops an **End-to-End ETL Pipeline** that automatically extracts Excel files, standardizes them, stores them inside PostgreSQL using the Medallion Architecture, and serves Power BI dashboards for business analysis.

Instead of manually preparing Excel reports every month, users only need to place new files into the **data** folder and execute the ETL pipeline.

---

# 🎯 Business Objectives

The project aims to automate the complete data preparation process.

Main objectives include:

✔ Import multiple Excel workbooks automatically

✔ Read every worksheet without manual intervention

✔ Preserve original source data

✔ Standardize data quality

✔ Build a centralized PostgreSQL Data Warehouse

✔ Provide clean analytical datasets for Power BI

✔ Reduce manual reporting effort

---

# 📂 Data Sources

The ETL pipeline processes business data stored as local Excel workbooks.

Example source files:

```text
data/

sales_transactions.xlsx

sales_target.xlsx

customer_master.xlsx

employee_master.xlsx

product_master.xlsx

distributor_master.xlsx

promotion_program.xlsx
```

Each workbook may contain one or multiple worksheets.

Every worksheet represents an independent business dataset.

---

# 🏗 System Architecture

```
                   Local Excel Files
                           │
                           ▼
                  Python ETL Pipeline
                           │
            ┌──────────────┴──────────────┐
            │                             │
            ▼                             ▼
      Data Extraction             Metadata Generation
            │                             │
            └──────────────┬──────────────┘
                           ▼
                    PostgreSQL Raw
                           │
                           ▼
                  SQL Data Cleaning
                           │
                           ▼
                  PostgreSQL Silver
                           │
                           ▼
               Business Transformation
                           │
                           ▼
                  PostgreSQL Gold
                           │
                           ▼
                     Power BI Desktop
```

The project follows the **Medallion Architecture**, separating raw, cleaned and analytical data into different database schemas.

---

# 🛠 Technology Stack

| Technology | Purpose |
|------------|----------|
| Python | ETL Development |
| Pandas | Read & Transform Excel |
| SQLAlchemy | Database Connection |
| PostgreSQL | Data Warehouse |
| pgAdmin | Database Administration |
| Power BI | Reporting & Dashboard |
| Git | Version Control |

---

# 📁 Project Structure

```text
vietdist_analytics/

│

├── data/

│      sales_transactions.xlsx

│      sales_target.xlsx

│      customer_master.xlsx

│      product_master.xlsx

│      employee_master.xlsx

│      ...

│

├── ingestion/

│      ├── loaders/

│      │        load_Raw.py

│      │

│      └── utils/

│               db_utils.py

│               file_parser.py

│

├── sql/

│      Raw/

│      silver/

│      gold/

│

├── powerbi/

│      vietdist_dashboard.pbix

│

├── docs/

│

└── README.md
```

Each folder has a dedicated responsibility.

### data/

Stores all source Excel workbooks.

---

### ingestion/

Contains all Python ETL scripts.

Responsible for:

- Reading Excel files
- Loading PostgreSQL
- Adding Metadata
- Logging Pipeline

---

### sql/

Contains SQL scripts used to transform data from Raw to Silver and Gold.

---

### powerbi/

Contains Power BI report connected to PostgreSQL.

---

### docs/

Project documentation.

---

# ⚙ ETL Pipeline Overview

The ETL pipeline consists of three major layers.

```
Extraction

↓

Raw Layer

↓

Silver Layer

↓

Gold Layer

↓

Power BI
```

Each layer has a different responsibility.

---

## Raw Layer

Purpose

Store raw data exactly as received.

Characteristics

- No business transformation
- Preserve original records
- Add ETL metadata
- Historical loading
- Append mode

---

## Silver Layer

Purpose

Clean and standardize raw datasets.

Main operations

- Remove duplicates
- Handle NULL values
- Rename columns
- Convert data types
- Standardize values
- Validate data quality

---

## Gold Layer

Purpose

Prepare business-ready tables.

Includes

- Dimension Tables
- Fact Tables
- Data Mart
- Business Metrics

Power BI connects directly to this layer.

---

# 🚀 Environment Setup

## Step 1

Clone Repository

```bash
git clone https://github.com/yourusername/vietdist_analytics.git
```

---

## Step 2

Create Virtual Environment

```bash
python -m venv venv
```

Activate

Windows

```bash
venv\Scripts\activate
```

Mac

```bash
source venv/bin/activate
```

---

## Step 3

Install Dependencies

```bash
pip install pandas

pip install sqlalchemy

pip install psycopg2-binary

pip install openpyxl

pip install python-dotenv
```

---

# PostgreSQL Setup

Create database

```
viet_list_dw
```

Create schemas

```
raw

staging

dwh
```

These schemas represent

Raw

Silver

Gold

respectively.

---

# Running the ETL Pipeline

Place all Excel files inside

```
data/
```

Execute

```bash
python ingestion/loaders/load_Raw.py
```

The pipeline will automatically

✔ Scan every workbook

✔ Read every worksheet

✔ Generate PostgreSQL tables

✔ Add metadata

✔ Load into Raw

✔ Record pipeline logs

---

# Pipeline Workflow

```
Excel Workbook

↓

Worksheet

↓

Pandas DataFrame

↓

Metadata

↓

Raw Table

↓

PostgreSQL

↓

SQL Transformation

↓

Silver

↓

Gold

↓

Power BI Dashboard
```

---

# 🐍 ETL Implementation

The ETL pipeline is developed entirely in Python using **Pandas** and **SQLAlchemy**. It automates the ingestion of multiple Excel workbooks, standardizes the data, enriches it with metadata, and loads it into the PostgreSQL Raw layer.

The pipeline is designed with a modular architecture, where each module has a single responsibility, making the project easier to maintain and extend.

---

# 📂 ETL Module Architecture

```text
                    load_Raw.py
                           │
        ┌──────────────────┴──────────────────┐
        │                                     │
        ▼                                     ▼
 file_parser.py                        db_utils.py
        │                                     │
        ▼                                     ▼
 Read Excel Files                PostgreSQL Operations
        │                                     │
        └──────────────────┬──────────────────┘
                           │
                           ▼
                    PostgreSQL Raw
```

Each module performs a dedicated task during the ETL process.

---

# 📖 Module 1 — file_parser.py

## Purpose

This module is responsible for reading source files into Pandas DataFrames.

Instead of manually selecting the reading method, the parser automatically detects the file extension and uses the appropriate Pandas function.

---

## Supported File Formats

| File Type | Reader |
|------------|------------------------|
| CSV | pandas.read_csv() |
| XLSX | pandas.read_excel() |
| XLSM | pandas.read_excel() |

---

## Parsing Workflow

```text
Input File

        │

        ▼

Check Extension

        │

 ┌──────┼───────────┐

 │      │           │

 ▼      ▼           ▼

CSV    XLSX       XLSM

 │       │           │

 ▼       ▼           ▼

read_csv()   read_excel()

        │

        ▼

Pandas DataFrame
```

---

## Error Handling

If the uploaded file format is not supported, the parser immediately raises an exception.

This prevents invalid files from entering the ETL pipeline.

---

# 📖 Module 2 — db_utils.py

This module centralizes all PostgreSQL operations.

Instead of writing database logic inside the ETL pipeline, every database action is encapsulated in reusable utility functions.

---

## Responsibilities

- Create PostgreSQL connection
- Generate SQLAlchemy Engine
- Add ETL Metadata
- Load Raw Layer
- Load Silver Layer

---

# PostgreSQL Connection

The ETL pipeline connects to PostgreSQL through SQLAlchemy.

```text
Python

↓

SQLAlchemy Engine

↓

PostgreSQL Database
```

The database engine is shared across all loading operations.

---

# Metadata Generation

Before loading data into PostgreSQL, additional metadata columns are generated.

These columns help trace every record back to its origin.

| Column | Description |
|----------|-------------------------------|
| _source_file | Original workbook & worksheet |
| _source_platform | local_manual |
| _ingested_at | ETL execution timestamp |
| _batch_id | Unique ETL Batch |

---

## Why Metadata?

Metadata provides complete data lineage.

For example,

```text
Employee.xlsx

↓

Sheet : Employee

↓

Loaded

↓

2026-07-10

↓

Batch

3de6-xxxx
```

If data quality issues occur later, every row can be traced back to its original source.

---

# Raw Loading

The function

```
load_to_Raw()
```

loads DataFrames directly into the PostgreSQL **raw** schema.

Workflow

```text
DataFrame

↓

SQLAlchemy

↓

raw.table

↓

Append Mode
```

Append mode preserves historical ingestion instead of replacing previous data.

---

# Silver Loading

The function

```
load_to_silver()
```

stores cleaned datasets inside the **staging** schema.

Workflow

```text
Clean DataFrame

↓

SQLAlchemy

↓

staging.table
```

---

# 📖 Module 3 — load_Raw.py

This is the main ETL script.

Running this file starts the complete Raw ingestion process.

```bash
python ingestion/loaders/load_Raw.py
```

---

# Overall Pipeline Execution

The execution flow follows the sequence below.

```text
Start Pipeline

↓

Generate Batch ID

↓

Connect PostgreSQL

↓

Scan data Folder

↓

Read Excel Workbook

↓

Read Every Worksheet

↓

Standardize Columns

↓

Add Metadata

↓

Create Raw Table

↓

Insert Data

↓

Write Ingestion Log

↓

Finish
```

---

# Step 1 — Generate Batch ID

Every ETL execution generates a unique Batch ID.

Example

```
5b27c9d1-bcb8-4d77-a938-xxxxxxxx
```

This ID is attached to every imported record.

Benefits

- Pipeline Monitoring
- ETL Audit
- Data Lineage

---

# Step 2 — Scan Source Folder

The pipeline automatically scans

```
data/
```

to locate every available Excel workbook.

No file names are hardcoded.

Whenever a new workbook is added into the folder, it will automatically be detected.

---

# Step 3 — Read Workbook

For every workbook

```text
Sales_Target.xlsx
```

the pipeline loads all worksheets.

Example

```text
Sales_Target.xlsx

├── Target_V1

├── Target_V2

└── Target_V3
```

Each worksheet becomes an independent dataset.

---

# Step 4 — Read Every Worksheet

Each worksheet is converted into a Pandas DataFrame.

```text
Worksheet

↓

Pandas DataFrame
```

At this stage, no business transformation has been applied.

---

# Step 5 — Standardize Column Names

Before creating PostgreSQL tables, column names are standardized.

The pipeline automatically

- Remove leading spaces
- Replace blank spaces with "_"
- Convert names to lowercase

Example

| Original | Standardized |
|-----------|--------------|
| Employee ID | employee_id |
| Product Name | product_name |
| Sales Amount | sales_amount |

This ensures compatibility with PostgreSQL naming conventions.

---

# Step 6 — Add Metadata

Metadata columns are appended to every DataFrame.

```text
_source_file

_source_platform

_ingested_at

_batch_id
```

These fields are stored together with business data.

---

# Step 7 — Create Raw Tables

Instead of manually creating database tables, the ETL pipeline generates them dynamically.

Naming convention

```
<file_name>_<sheet_name>
```

Example

```text
sales_target.xlsx

↓

Target_V1

↓

raw.sales_target_target_v1
```

This design allows the pipeline to process newly added worksheets without modifying the source code.

---

# Step 8 — Load Data

After the table has been created, the DataFrame is inserted into PostgreSQL.

Loading mode

```
Append
```

Historical data is preserved across multiple ETL executions.

---

# Step 9 — Ingestion Logging

Each loading activity is recorded in

```
raw.ingest_log
```

The log contains

- Batch ID
- Source File
- Table Name
- Number of Rows
- Status
- Processing Time

Example

| Batch | File | Rows | Status |
|--------|------|------|---------|
| 5b27... | sales_target.xlsx | 325 | SUCCESS |

---

# Exception Handling

The pipeline includes error handling to ensure failures in one workbook do not terminate the entire ETL execution.

If an error occurs,

- Exception details are printed.
- Stack trace is recorded.
- Remaining files continue to be processed.

This improves pipeline robustness when handling multiple source files.

---

# Raw Layer Output

After a successful execution, PostgreSQL contains

```text
raw

├── sales_transactions_orders

├── customer_master_customer

├── product_master_products

├── sales_target_target_v1

├── sales_target_target_v2

├── sales_target_target_v3

├── employee_master_employee

└── ingest_log
```

These tables preserve the original business data and serve as the foundation for the Silver transformation layer.
# 🔄 Data Transformation Pipeline

After all source data has been successfully imported into the **raw** schema, the next stage of the ETL process prepares the data for analytical reporting.

Unlike the ingestion stage, which focuses on preserving the original source data, the transformation stage emphasizes data quality, consistency, and business logic.

The transformed datasets are stored in the `staging` schema before being promoted to the final Data Warehouse (`dwh`).

---

# 🗂 ETL Pipeline

```text
                Local Excel Files

                        │

                        ▼

                Python ETL Pipeline

                        │

                        ▼

                PostgreSQL (raw)

                        │

        SQL Cleaning & Standardization

                        │

                        ▼

             PostgreSQL (staging)

                        │

      Business Transformation & Modeling

                        │

                        ▼

               PostgreSQL (dwh)

                        │

                        ▼

                  Power BI Dashboard
```

---

# 📖 Stage 1 — Raw Schema

The **raw** schema stores imported data exactly as it appears in the source Excel files.

Characteristics

- Preserve original data
- No business transformation
- Append-only loading
- ETL metadata included
- Data lineage maintained

Example

```text
raw

├── customer_master

├── employee_master

├── product_master

├── sales_target_target_v1

├── sales_transactions

└── ingest_log
```

---

# 📖 Stage 2 — Staging Schema

The **staging** schema is responsible for preparing datasets before analytical modeling.

Typical transformation tasks include:

- Standardize column names
- Convert data types
- Handle missing values
- Remove duplicate records
- Validate key fields
- Apply business rules

Workflow

```text
raw.customer_master

        │

        ▼

Remove Duplicates

        │

        ▼

Handle NULL Values

        │

        ▼

Convert Data Types

        │

        ▼

Standardize Values

        │

        ▼

staging.customer_master
```

The cleaned datasets stored in `staging` provide a reliable source for downstream reporting and analytics.

---

# 📖 Stage 3 — Data Warehouse (dwh)

The **dwh** schema contains business-ready datasets optimized for reporting.

Unlike the staging layer, which focuses on data preparation, the Data Warehouse organizes information into analytical structures that support business intelligence tools.

Typical objects stored in this schema include:

- Dimension tables
- Fact tables
- Aggregated reporting tables
- Data marts

These datasets are designed for fast querying and efficient visualization in Power BI.

---

# 📊 Power BI Integration

The final reporting layer connects directly to the PostgreSQL Data Warehouse.

```text
PostgreSQL (dwh)

        │

        ▼

Power BI Desktop

        │

        ▼

Interactive Dashboard
```

Power BI retrieves only the curated datasets from the `dwh` schema, ensuring that reports are built on standardized and validated data.

---

# 📈 Dashboard Capabilities

The reporting dashboard supports analysis across multiple business perspectives, including:

- Sales Performance
- Revenue Trends
- Sales Target Achievement
- Distributor Performance
- Customer Analysis
- Product Performance
- Employee Performance

Interactive filters allow users to explore data by time period, distributor, customer, product, and employee.

---

# 🔍 Data Lineage

The complete data flow throughout the project is illustrated below.

```text
Excel Workbook

        │

        ▼

Worksheet

        │

        ▼

Pandas DataFrame

        │

        ▼

Metadata Enrichment

        │

        ▼

raw Schema

        │

        ▼

SQL Transformation

        │

        ▼

staging Schema

        │

        ▼

Business Modeling

        │

        ▼

dwh Schema

        │

        ▼

Power BI Dashboard
```

Each stage has a distinct responsibility, ensuring that raw source data remains unchanged while downstream layers progressively improve data quality and business usability.

---

# ⚡ Pipeline Summary

The ETL pipeline automates the entire data preparation process from source files to business dashboards.

Pipeline summary:

1. Scan the local `data/` directory for Excel workbooks.
2. Read every worksheet into a Pandas DataFrame.
3. Standardize column names.
4. Enrich data with ETL metadata.
5. Create destination tables dynamically in the `raw` schema.
6. Load data into PostgreSQL.
7. Record execution details in the ingestion log.
8. Clean and standardize data in the `staging` schema.
9. Build business-ready datasets in the `dwh` schema.
10. Connect Power BI to the Data Warehouse for reporting.

---

# 🚀 Future Improvements

The current implementation establishes a reusable ETL framework that can be extended with additional capabilities in future versions.

Potential enhancements include:

- Incremental loading to process only new or updated records.
- Automated scheduling using Apache Airflow or Windows Task Scheduler.
- Data validation and quality reports.
- Configuration management using `.env` files.
- Centralized logging and monitoring.
- Docker-based deployment.
- Automated testing for ETL components.

---

# 📌 Key Features

✔ Automated Excel ingestion

✔ Dynamic worksheet processing

✔ Automatic table creation

✔ Metadata tracking

✔ ETL execution logging

✔ PostgreSQL integration

✔ Layered database architecture (`raw`, `staging`, `dwh`)

✔ Power BI reporting support

✔ Modular Python codebase

---

# 👨‍💻 Author

**Trần Quốc Hưng**

Data Analyst | Analytics Engineering

**Technologies**

- Python
- SQL
- PostgreSQL
- Power BI
- Pandas
- SQLAlchemy
- Git & GitHub
