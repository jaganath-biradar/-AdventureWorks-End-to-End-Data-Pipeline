📌 Project Overview

This project demonstrates an end-to-end data pipeline where raw CSV files are ingested into PostgreSQL, cleaned and transformed using SQL-based ETL, modeled into a Star Schema, and finally consumed in Power BI for analytics and reporting.

The project follows industry best practices such as staging tables, data validation, and proper filter flow.

🛠️ Tech Stack

Database: PostgreSQL

ETL: SQL (psql \copy, staging tables)

BI Tool: Power BI

Data Source: AdventureWorks CSV files

🗂️ Data Sources

AdventureWorks Customer Lookup

AdventureWorks Product Lookup

AdventureWorks Product Categories Lookup

AdventureWorks Product Subcategories Lookup

AdventureWorks Territory Lookup

AdventureWorks Calendar Lookup

AdventureWorks Sales Data (2020, 2021, 2022)

Returns Data

🏗️ Architecture
CSV Files
   ↓
Staging Tables (TEXT data types)
   ↓
Cleaned & Typed Dimension / Fact Tables
   ↓
Star Schema (PostgreSQL)
   ↓
Power BI Dashboard

🔄 ETL Approach
1️⃣ Staging Layer

Created staging tables (stg_*) with TEXT columns.

Purpose:

Handle dirty data (nulls, special characters, invalid numbers)

Prevent load failures during ingestion

Example:

CREATE TABLE stg_dim_customer (
    customer_key TEXT,
    first_name TEXT,
    last_name TEXT,
    ...
);

2️⃣ Data Loading

Used psql \copy to load CSVs from local file system.

Ensured correct handling of:

Quoted text

Embedded commas

Headers

\copy stg_dim_customer
FROM 'path/to/AdventureWorks Customer Lookup.csv'
DELIMITER ','
CSV HEADER;

3️⃣ Data Cleaning & Transformation

Converted data types from TEXT → INT, DATE, NUMERIC.

Removed invalid rows using regex filtering.

Calculated derived metrics such as sales_amount.

Example:

INSERT INTO dim_customer
SELECT
    customer_key::INT,
    first_name,
    last_name
FROM stg_dim_customer
WHERE customer_key ~ '^[0-9]+$';

📐 Data Model (Star Schema)
Dimension Tables

dim_customer

dim_product

dim_product_category

dim_product_subcategory

dim_calendar

dim_territory

Fact Tables

fact_sales (merged 2020–2022)

fact_returns

All relationships follow:

One-to-many

Single-direction filter flow

Dimensions → Facts

🔍 Power BI Modeling

Connected PostgreSQL using Import mode.

Verified schema consistency (public).

Built relationships following star schema design.

Validated:

Filter context

Filter flow

Aggregation accuracy

🧠 Key Concepts Implemented

Staging tables for raw data ingestion

SQL-based ETL transformations

Handling dirty CSV data

Star schema design

Filter context vs filter flow in Power BI

Multi-year fact table merging

🚀 Outcome

Clean, scalable analytical data model

Reliable Power BI reporting

Interview-ready end-to-end data project

📎 Future Enhancements

Add incremental refresh in Power BI

Automate ETL using Python or Airflow

Add advanced DAX measures (YTD, YoY growth)

Implement data quality checks

👤 Author

Jaggu
Aspiring Data Analyst / Business Intelligence Professional
