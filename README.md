## Fetch Rewards Coding Exercise - Analytics Engineer

This project is a coding exercise for the Analytics Engineer role at Fetch Rewards. It demonstrates how to preprocess unstructured JSON data, build a structured relational database, and analyze data quality issues – all within a Jupyter Notebook environment.

### Table of Contents
- [Setup and Installation](#setup-and-installation)
- [Preprocessing Unstructured JSON Data](#preprocessing-unstructured-json-data)
- [Structured Database Schema](#structured-database-schema)
- [ER Diagram](#er-diagram)
- [Business Stakeholder Queries](#business-stakeholder-queries)
- [Data Quality Analysis & Recommendations](#data-quality-analysis--recommendations)
- [Stakeholder Communication](#stakeholder-communication)
- [Running the Notebook](#running-the-notebook)

### Setup and Installation

1. **Python Environment Setup:**  
   Create and activate your virtual environment:
   ```bash
   source myenv/bin/activate
   ```

2. **Install Dependencies:**  
   Install all required packages using:
   ```bash
   pip install -r requirement.txt
   ```

3. **Prerequisites:**  
   - Python 3.x
   - MySQL Server (ensure you have the relevant credentials)
   - JSON data files (`users.json`, `brands.json`, `receipts.json`) in the project directory

### Preprocessing Unstructured JSON Data

The notebook (`fetch_query.ipynb`) begins by importing necessary libraries and defining helper functions to process raw JSON data.

### Structured Database Schema

After preprocessing, the notebook connects to a MySQL database and creates four main tables:

- **USER:** Stores user details such as `user_id`, `state`, `created_date`, `last_login`, `role`, `active_status`, and `sign_up_source`.
- **BRAND:** Contains brand information, including `brand_id`, `barcode`, `brand_code`, `category`, and `name`.
- **RECEIPT:** Holds receipt data like `receipt_id`, `user_id`, timestamps for various events, `bonus_points`, and `statuses`.
- **RECEIPT_ITEM:** Captures detailed information about individual items on receipts, with a foreign key linking each item to its corresponding receipt.

### ER Diagram

An ER (Entity Relationship) diagram is generated to illustrate the relationships between the tables. This diagram is stored as a PNG file (`mermaid-diagram-2025-02-12-145928.png`).

### Business Stakeholder Queries

The notebook contains SQL queries that address questions from business stakeholders. These queries focus on key business metrics, including:
- Top 5 brands by receipts scanned for most recent month  
- Comparison of the top 5 brands by receipts scanned in the recent month versus the previous month
- Average spend on receipts with a `rewardsReceiptStatus` of `Accepted` versus `Rejected`, identifying which is higher
- Total number of items purchased from receipts with a `rewardsReceiptStatus` of `Accepted` versus `Rejected`, identifying which is higher
- Brand with the highest total spend among users who joined within the past 6 months
- Brand with the highest number of transactions among users who joined within the past 6 months

### Data Quality Analysis & Recommendations

The notebook provides a detailed exploration of several data quality issues:
- Missing `user_id` Values in the `USER` Table
- Duplicate `user_id` Records in the `USER` Table
- Duplicate `barcode` Records in the `BRAND` Table
- Inconsistent `barcode` matching between `BRAND` and `RECEIPT_ITEM` Tables
- High percentage of missing data in the `RECEIPT` Table

SQL queries in the notebook support and quantify these findings, reinforcing the recommended data quality improvements.

### Stakeholder Communication

The notebook includes a drafted email communication to key business stakeholders. This email outlines critical data quality issues found during the analysis and seeks guidance on improving data integrity for more reliable business insights.

### Running the Notebook

To run the analysis:
1. **Ensure MySQL is Running:**  
   Verify that your MySQL server is active and the credentials in the notebook match your configuration.

2. **Prepare Data Files:**  
   Place the JSON files (`users.json`, `brands.json`, `receipts.json`) in the same directory as `fetch_query.ipynb`.

3. **Open and Execute:**  
   Open `fetch_query.ipynb` in Jupyter Notebook (or an equivalent environment) and run the cells sequentially. Review outputs in the console for table schema details, record samples, and data quality metrics.


```