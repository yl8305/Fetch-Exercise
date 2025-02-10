# Data Quality and ETL Report

## Overview
This document summarizes the data quality assessment and ETL (Extract, Transform, Load) process performed on our source JSON files. The key files processed are:
- `users.json`
- `brands.json`
- `receipts.json`

Data was processed using a custom Python script that loads each JSON record (line-by-line) while skipping duplicates, transforms fields (e.g., converting MongoDB date formats), and loads the data into an in-memory SQLite database. Four main tables were created:
- **USER**
- **BRAND**
- **RECEIPT**
- **RECEIPT_ITEM**

## Data Quality Summary

### users.json
- **Unique Records Loaded:** 212  
- **Duplicate Records Found:** 283  
- **Observations:**  
  - A significant number of duplicate user records were detected and skipped.
  - Consider investigating the source of these duplicates if unique user data is critical.

### brands.json
- **Unique Records Loaded:** 1167  
- **Duplicate Records Found:** 0  
- **Observations:**  
  - Data for brands appears clean with no duplicates.
  
### receipts.json
- **Unique Receipt Records Loaded:** 1119
- **Duplicate Records Found:** 0  
- **Observations:** 
  - Not every receipt record includes item-level details.
  - Some receipts include a nested field named `rewardsReceiptItemList` that holds the purchased item details.

### Receipt Items Extraction
- **Total Receipt Items Extracted:** 6941  
- **Observations:**  
  - Receipt items were extracted from the nested `rewardsReceiptItemList` in the receipts.
  - Not all receipts provide this field, which means that item-level details are missing for some receipts.

## ETL Process and Table Creation Steps

1. **Data Loading:**  
   - Each JSON file was read line by line.
   - A custom loader function extracted unique records based on the `_id.$oid` field and skipped duplicates.
   - The resulting unique records were stored for further processing.

2. **Table Creation:**  
   An in-memory SQLite database was used to create the following tables:
   - **USER:**  
     Stores user details such as `user_id`, `state`, `created_date`, `last_login`, `role`, `active`, and `sign_up_source`.
   - **BRAND:**  
     Contains details about brands such as `brand_id`, `barcode`, `brand_code`, `category`, `category_code`, `cpg_id`, `top_brand`, and `name`.
   - **RECEIPT:**  
     Contains the receipt information including fields like `receipt_id`, `user_id`, `bonus_points_earned`, `bonus_points_earned_reason`, `create_date`, `date_scanned`, `finished_date`, `modify_date`, `points_awarded_date`, `points_earned`, `purchase_date`, `purchased_item_count`, `total_spent`, and `rewards_receipt_status`.
   - **RECEIPT_ITEM:**  
     Populated by extracting the nested receipt items from the `rewardsReceiptItemList` field within receipts. A unique `receipt_item_id` was generated (using the receipt's ID and the index within the list) and the details (e.g., `barcode`, `description`, `final_price`, etc.) were extracted.

3. **Data Transformation:**  
   - MongoDB date formats (`{"$date": ...}`) were transformed into integer timestamps.
   - Numeric values and booleans were cast to the appropriate types.
   - Nested receipt items were iterated over and flattened for insertion into the `RECEIPT_ITEM` table.

4. **Data Loading and Verification:**  
   - After processing, the data was committed to the SQLite database.
   - Verification queries confirmed:
     - **USER:** 212 records
     - **BRAND:** 1167 records
     - **RECEIPT:** 1119 records
     - **RECEIPT_ITEM:** 6941 records

## Recommendations & Next Steps
- **Review Duplicate Handling:**  
  Investigate the cause of the duplicate records in `users.json` and, if necessary, implement additional deduplication mechanisms at the source.
  
- **Ensure Receipt Completeness:**  
  Confirm with data providers whether all receipts are expected to have item-level details and address any inconsistencies accordingly.
  
- **Data Type Validation:**  
  Continue monitoring for potential issues with data types (e.g., numeric fields, dates, booleans) to ensure consistency across the data sets.
  
- **Ongoing Quality Monitoring:**  
  Document any anomalies or process changes in subsequent reports to maintain data quality over time.

---

This document serves as both a record of the ETL process and a reference for data quality issues encountered during ingestion and transformation. Adjustments to the process should be considered based on further stakeholder feedback or data source changes.

## File Structure Issues
- users.json, brands.json, receipts.json are not valid JSON files
  - Each line contains a separate JSON object without proper array structure

## Data Format Issues
- Date fields in `receipts.json` use MongoDB date format ($date)
- Some numeric fields contain non-numeric values
- Inconsistent boolean representations in topBrand field

## Missing Data
- Some required fields are missing in various records
- Incomplete user information
- Missing receipt items data

## Data Consistency Issues
- Duplicate IDs present in the dataset
- Inconsistent state abbreviations in user data
- Mismatched foreign key relationships

## Data Type Issues
- Inconsistent data types for numeric fields
- Boolean fields sometimes contain non-boolean values
- Date fields occasionally contain invalid formats

## Recommendations
- Standardize JSON file structure
- Implement proper data validation
- Add data type constraints
- Establish clear foreign key relationships

## Questions for Stakeholders
- What is the expected format for dates?
- How should we handle duplicate records?
- What are the business rules for valid states? 