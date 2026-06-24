# E-Commerce Sales Analysis: End-to-End SQL Analytics Project

## Overview

This project analyzes over 1 million e-commerce transactions from the Online Retail II dataset (2009–2011) using PostgreSQL and SQL.

Unlike many analytics projects that begin with a pre-built database, this project started with raw Excel files. The data was extracted, transformed, loaded into PostgreSQL, validated, and then analyzed to answer key business questions.

The project demonstrates the complete analytics workflow:

* Data Acquisition
* Database Design
* ETL Development
* Data Validation
* SQL Analysis
* Business Intelligence

---

## Business Problem

Management wanted to understand sales performance and customer behavior by answering the following questions:

* Which products generate the most revenue?
* Which customers spend the most?
* Which countries perform best?
* Which months generate the highest sales?

---

## Dataset

**Source:** Online Retail II Dataset

The dataset was provided as an Excel workbook containing two worksheets:

* Year 2009–2010
* Year 2010–2011

The dataset contains transaction-level retail sales data including:

* Invoice Numbers
* Product Descriptions
* Customer IDs
* Countries
* Purchase Dates
* Quantities
* Unit Prices

---

## Project Architecture

Raw Excel Files
↓
Database Design
↓
PostgreSQL Schema
↓
Python ETL Pipeline
↓
Data Validation
↓
SQL Analysis
↓
Business Insights

---

## Database Design

A relational database was designed to organize transactional, customer, and product data.

### Invoice Table

Stores transaction-level information.

| Column       | Description         |
| ------------ | ------------------- |
| invoiceid    | Invoice Number      |
| customerid   | Customer Identifier |
| invoice_date | Transaction Date    |
| quantity     | Units Purchased     |
| price        | Unit Price          |

### Customers Table

Stores customer information.

| Column     | Description         |
| ---------- | ------------------- |
| customerid | Customer Identifier |
| invoiceid  | Related Invoice     |
| country    | Customer Country    |

### Products Table

Stores product information.

| Column      | Description         |
| ----------- | ------------------- |
| productid   | Product Identifier  |
| invoiceid   | Related Invoice     |
| description | Product Description |

---

## ETL Process

The source data was provided in Excel format and imported into PostgreSQL using Python.

### Technologies Used

* Python
* Pandas
* Psycopg2
* PostgreSQL

### ETL Workflow

1. Read both Excel worksheets using Pandas.
2. Extract transaction records.
3. Connect to PostgreSQL using Psycopg2.
4. Insert records into database tables.
5. Commit data in batches.
6. Validate successful imports using SQL quality checks.

### Data Validation

The following checks were performed:

* Null value detection
* Duplicate invoice detection
* Invalid quantity checks
* Invalid price checks
* Row count verification

These checks ensured the dataset was ready for analysis.

---

## Analysis Workflow

### 1. Data Exploration

Initial exploration included:

* Reviewing table structures
* Inspecting sample records
* Counting rows

### 2. Data Quality Assessment

Performed:

* Missing value analysis
* Duplicate detection
* Validation of quantity values
* Validation of price values

### 3. Data Preparation

Joined invoice, customer, and product tables to create a unified analytical dataset.

### 4. KPI Calculation

Calculated key business metrics including:

* Total Revenue
* Total Transactions
* Total Customers
* Average Order Value (AOV)
* Modal Order Value (MOV)

### 5. Business Analysis

Performed:

* Revenue analysis
* Product analysis
* Customer spending analysis
* Country performance analysis
* Monthly sales analysis
* Year-over-year sales analysis

---

## Key Metrics

| KPI                 | Value     |
| ------------------- | --------- |
| Total Customers     | 5,943     |
| Total Transactions  | 1,067,371 |
| Average Order Value | £18.07    |
| Modal Order Value   | £15.00    |

---

## Business Questions & Findings

### 1. Which Year Generated the Most Revenue?

Revenue peaked in 2010, making it the strongest year in the dataset.

---

### 2. Which Months Generated the Highest Revenue?

Top-performing month by year:

| Year | Month    |
| ---- | -------- |
| 2009 | December |
| 2010 | December |
| 2011 | November |

#### Revenue

| Year | Month    | Revenue      |
| ---- | -------- | ------------ |
| 2009 | December | £77,475,021  |
| 2010 | December | £256,663,166 |
| 2011 | November | £150,292,728 |

Sales consistently peaked during the holiday shopping season.

---

### 3. Which Products Sold the Most Each Year?

| Year | Product                            | Quantity Sold |
| ---- | ---------------------------------- | ------------- |
| 2009 | White Hanging Heart T-Light Holder | 95,474        |
| 2010 | White Hanging Heart T-Light Holder | 116,048       |
| 2011 | Jumbo Bag Red Retrospot            | 890,988       |

---

### 4. Which Products Dominated the Highest-Revenue Month?

| Year | Month | Product                            | Quantity Sold |
| ---- | ----- | ---------------------------------- | ------------- |
| 2009 | Dec   | White Hanging Heart T-Light Holder | 95,474        |
| 2010 | Dec   | Pack of 72 Retrospot Cake Cases    | 208,601       |
| 2011 | Nov   | Rabbit Night Light                 | 206,645       |

---

### 5. Which Customers Spend the Most?

Top spending customers were concentrated in:

* United Kingdom
* Netherlands
* Ireland (EIRE)

The United Kingdom generated the largest share of revenue across all years.

---


### 6. Which Countries Perform Best?

The United Kingdom was the strongest-performing market by revenue and transaction volume.

Other high-value markets included:

* Netherlands
* Ireland (EIRE)

---

## Business Recommendations

### Inventory Planning

Maintain strong inventory levels for consistently high-performing products:

* White Hanging Heart T-Light Holder
* Jumbo Bag Red Retrospot
* Rabbit Night Light

### Seasonal Strategy

Increase inventory and marketing investment before:

* November
* December

These months consistently produce peak sales.

### Customer Retention

Convert guest(NaN) purchasers into registered customers through:

* Loyalty programs
* Email incentives
* Exclusive member discounts

### International Growth

Expand marketing efforts in:

* Netherlands
* Ireland (EIRE)

These regions contain high-value customers and strong revenue potential.

---

## Skills Demonstrated

### SQL

* Joins
* Aggregations
* CTEs
* Window Functions
* Ranking Functions
* Date Functions
* KPI Calculations
* Business Analysis Queries

### PostgreSQL

* Database Design
* Schema Creation
* Data Validation
* Query Optimization

### Python

* Pandas
* Psycopg2
* ETL Development
* Excel Data Processing

### Data Analytics

* Revenue Analysis
* Customer Segmentation
* Product Performance Analysis
* Trend Analysis
* Business Intelligence
* Data Storytelling

---

## Repository Structure

ecommerce-sql-business-analysis/

├── data/

│   ├── online_retail_II.xlsx

│

├── SQL/

│   └── queries.sql

│

├── report/

│   └── report.pdf

│

├── images/

│   ├── revenue_by_year.png

│   ├── revenue_by_month.png

│   ├── top_products.png

│   ├── top_countries.png

│   └── customer_spending.png

│

├── ETL/

│   └── excel_to_postgresql.py

│

└── README.md

---

## Author

Arnold Muia

Data Analytics Portfolio Project

PostgreSQL | SQL | Python | ETL | Business Intelligence
