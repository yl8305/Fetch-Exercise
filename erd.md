erDiagram
    USER {
        string user_id PK "Unique user ID"
        string state "State abbreviation"
        datetime created_date "Account creation date"
        datetime last_login "Last login timestamp"
        string role "User role (e.g., consumer, fetch-staff)"
        boolean active "Active status"
        string sign_up_source "Source of sign-up (e.g., Email)"
    }
    
    RECEIPT {
        string receipt_id PK "Receipt ID"
        string user_id FK "References USER.user_id"
        int bonus_points_earned "Bonus points awarded"
        string bonus_points_earned_reason "Reason for bonus points"
        datetime create_date "Date record was created"
        datetime date_scanned "Date receipt was scanned"
        datetime finished_date "Date receipt processing finished"
        datetime modify_date "Record modification date"
        datetime points_awarded_date "Date points were awarded"
        decimal points_earned "Points earned from receipt"
        datetime purchase_date "Purchase date"
        int purchased_item_count "Count of items purchased"
        decimal total_spent "Total spent amount"
        string rewards_receipt_status "Status of the receipt"
    }

    RECEIPT_ITEM {
        string receipt_item_id PK "Unique receipt item ID"
        string receipt_id FK "References RECEIPT.receipt_id"
        string barcode "Item barcode"
        string description "Item description"
        decimal final_price "Final price after discounts"
        decimal item_price "Original item price"
        int quantity_purchased "Quantity purchased"
        string partner_item_id "Partner item identifier"
        boolean needs_fetch_review "Flag for manual review"
        boolean prevent_target_gap_points "Indicator to prevent targeted gap points"
        string user_flagged_barcode "User flagged barcode"
        boolean user_flagged_new_item "Indicator if user flagged item as new"
        decimal user_flagged_price "User flagged price"
        int user_flagged_quantity "User flagged quantity"
        string brand_code "Optional brand code linking to BRAND"
    }

    BRAND {
        string brand_id PK "Unique brand identifier"
        string barcode "Brand barcode"
        string brand_code "Code corresponding with partner file"
        string category "Category name"
        string category_code "Category code"
        string cpg_id "Reference to CPG collection"
        boolean top_brand "Indicator if brand is top featured"
        string name "Brand name"
    }

    USER ||--o{ RECEIPT : "has"
    RECEIPT ||--|{ RECEIPT_ITEM : "contains"
    BRAND ||--o{ RECEIPT_ITEM : "may be linked to"