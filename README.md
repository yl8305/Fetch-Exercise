# Fetch Rewards Coding Exercise - Analytics Engineer

This project is a coding exercise for the Analytics Engineer role at Fetch Rewards. It demonstrates how to preprocess unstructured JSON data, build a structured relational database, and analyze data quality issues – all within a Jupyter Notebook environment.

## Table of Contents
- [Project Overview](#project-overview)
- [Setup and Installation](#setup-and-installation)
- [Preprocessing Unstructured JSON Data](#preprocessing-unstructured-json-data)
- [Structured Database Schema](#structured-database-schema)
- [ER Diagram](#er-diagram)
- [Data Quality Analysis & Recommendations](#data-quality-analysis--recommendations)
- [Stakeholder Communication](#stakeholder-communication)
- [Running the Notebook](#running-the-notebook)
- [Future Considerations](#future-considerations)
- [License](#license)

## Project Overview

The FETCH_OA project transforms unstructured JSON data into a well-organized relational database. This entails:
- **Preprocessing:** Converting timestamps, filtering duplicates, and handling nested JSON structures.
- **Database Creation:** Building and populating tables such as `USER`, `BRAND`, `RECEIPT`, and `RECEIPT_ITEM`.
- **Visualization:** Displaying an ER diagram to explain the schema relationships.
- **Analysis:** Providing insights into data quality issues and offering recommendations for improvements.
- **Communication:** Drafting a sample email to stakeholders regarding key data quality concerns.

## Setup and Installation

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

## Preprocessing Unstructured JSON Data

The notebook (`fetch_query.ipynb`) begins by importing necessary libraries and defining helper functions to process raw JSON data.

## Structured Database Schema

After preprocessing, the notebook connects to a MySQL database and creates four main tables:

- **USER:** Stores user details such as user ID, state, created date, last login, role, active status, and sign-up source.
- **BRAND:** Contains brand information, including brand ID, barcode, brand code, category, and name.
- **RECEIPT:** Holds receipt data like receipt ID, user ID, timestamps for various events, bonus points, and statuses.
- **RECEIPT_ITEM:** Captures detailed information about individual items on receipts, with a foreign key linking each item to its corresponding receipt.

## ER Diagram

An ER (Entity Relationship) diagram is generated to illustrate the relationships between the tables. This diagram is stored as a PNG file (`mermaid-diagram-2025-02-12-145928.png`).

# Data Quality Analysis & Recommendations

The notebook provides a detailed exploration of several data quality issues:
- **Duplicate Records:**  
  Duplicate `user_id` records in the `USER` table (283 duplicates found) and duplicate `barcode` entries in the `BRAND` table (7 duplicates) are identified. Recommendations include enforcing unique constraints and considering composite keys when necessary.

- **Receipt Matching:**  
  Some receipts reference a `user_id` that does not exist in the `USER` table. Although these receipts are retained to preserve transactional insights, recommendations include either assigning a default "GUEST" user or flagging transactions as orphaned.

- **Missing Data:**  
  Approximately 40.04% of receipts in the `RECEIPT` table lack a `purchase_date`, which could skew transaction analyses. Suggestions include backfilling missing data and enhancing validation rules.

- **Inconsistent Barcode Matching:**  
  The current matching logic for linking receipt items to brands (using `user_flagged_barcode`, then `barcode`, and finally `brand_code`) leaves about 90% of receipt items unmatched. A recommended approach is to assign these to an "UNMATCHED" placeholder to better track data.

SQL queries in the notebook support and quantify these findings, reinforcing the recommended data quality improvements.

## Stakeholder Communication

The notebook also includes a drafted communication to stakeholders.

## Running the Notebook

To run the analysis:
1. **Ensure MySQL is Running:**  
   Verify that your MySQL server is active and the credentials in the notebook match your configuration.

2. **Prepare Data Files:**  
   Place the JSON files (`users.json`, `brands.json`, `receipts.json`) in the same directory as `fetch_query.ipynb`.

3. **Open and Execute:**  
   Open `fetch_query.ipynb` in Jupyter Notebook (or an equivalent environment) and run the cells sequentially. Review outputs in the console for table schema details, record samples, and data quality metrics.


```