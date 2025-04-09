# Retail Store Transaction Dashboard

## Dashboard 
![retail store dashboard](https://github.com/user-attachments/assets/93fcfd64-60a5-4f2e-8ac2-ade5c3aff8a3)

## Table of Content
- [Project Overview](#project-overview)
- [Data Source](#data-source)
- [Tools & Technologies](#tools-&-technologies)
- [Objectives](#objectives)
- [Data Cleaning & Preparation](#data-cleaning-&-preparation)
- [Business Questions Answered (EDA)](#business-questions-answered)



## Project Overview

This project focuses on analyzing retail store transactions to uncover key business insights, such as product performance, sales trends, and customer behavior. It demonstrates an end-to-end data analytics workflow using **Excel**, **MySQL**, **Power Query**, and **Power BI**.

## Data Source

- The dataset was sourced from **[Kaggle](https://www.kaggle.com/datasets/ahmedmohamed2003/retail-store-sales-dirty-for-data-cleaning)**  


##  Tools & Technologies

- **Excel** – Initial data formatting and structure
- **MySQL** – Data wrangling, missing value handling, and transformation
- **Power Query** – Advanced data shaping and transformation
- **DAX** – Creation of dynamic measures and calculations
- **Power BI** – Dashboard creation and data visualization

## Objectives

- Clean and prepare retail transaction data
- Handle missing values efficiently
- Create visual insights for key business metrics
- Practice dashboard creation and storytelling


## Data Cleaning & Preparation

### Preprocessing in Excel
- The raw CSV file contained blank and empty values that caused issues when loading into MySQL.
- All columns were temporarily converted to **text (string)** format in Excel to ensure complete data was imported into MySQL without rows being skipped or dropped.

  ### Transformation in MySQL
- After successful import, each column was converted to its proper data type (`FLOAT`, `INT`, `DATE`, etc.).
- Missing values in the `Item` column were identified by analyzing **patterns** in the **Category** and **Price Per Unit** columns.
- `Quantity` was calculated as `Total Spend / Unit Price` if missing.
- `Unit Price` was calculated as `Total Spend / Quantity` if missing.
- If both `Quantity` and `Total Spend` were missing, `Quantity` was estimated using the **average quantity by product category**.

 **Power Query (in Power BI)** was used for:
  - Final data transformation
  - Creating derived columns:
    - **Date-related columns**: Day name, Month name, Year
    - **Day category**: Grouping into **Weekday** or **Weekend**
  - Filtering and reshaping data for visualization


- **DAX** used to:
  - Create dynamic KPIs (Total Sales, Quantity, Transactions)
  - Calculate **percent contributions**, **averages**, and **YOY trends**
  - Enable drilldowns and filter interactivity

## Business Questions Answered (EDA)

- What are the top-selling product categories?
- What time of the year sees the highest sales?
- How do weekday sales compare to weekend sales?
- Which payment methods are most commonly used?
- Are online or in-store purchases more dominant?


## Data Analysis

```sql
-- checking for duplicate

WITH duplicate_check AS(
SELECT *,
ROW_NUMBER() 
OVER (PARTITION BY Transaction_ID,Customer_ID, Category, Item, Price_Per_Unit, Quantity ,Total_Spent,Payment_Method,Location, Transaction_Date, Discount_Applied ORDER BY Category) AS row_no
FROM retail_store_sales2
) 

SELECT *
FROM duplicate_check
WHERE row_no > 1
;
-- Replacing the missing or empty cell in item column base on identified pattern. 


SELECT store1.Category,store2.Category,store1.Price_Per_Unit,store2.Price_Per_Unit,store1.Item,store2.Item
FROM retail_store_sales2 AS store1
JOIN retail_store_sales2 AS store2
	ON store1.Category = store2.Category
    AND store1.Price_Per_Unit = store2.Price_Per_Unit
     WHERE store1.Item IS NULL
    AND store2.Item  IS NOT NULL
        ;
        
 UPDATE retail_store_sales2 AS store1
 JOIN retail_store_sales2 AS store2
	ON store1.Category = store2.Category
    AND store1.Price_Per_Unit = store2.Price_Per_Unit
    SET store1.Item = store2.Item
     WHERE store1.Item IS NULL
    AND store2.Item  IS NOT NULL

```
## Key Insights/Result

- ₦1.637M revenue from 12,575 transactions
- 71% of transactions occurred on weekdays
- Food and Furniture were the most sold product categories (8.9K units)
- Sales dropped significantly in 2025
- Online and in-store sales were almost evenly split
- Cash was the most common payment method

##  Recommendations

Increase weekend sales through targeted promotions, focus on top-selling categories like Beverages and Furniture, enhance digital payment options for smoother transactions, analyze store vs online performance for strategic channel optimization, align stock levels with seasonal sales trends, implement customer segmentation for personalized marketing, and evaluate low-performing products for improvement or phase-out.

## Limitations

- The dataset does not contain unique customer identifiers, making it difficult to perform personalized customer segmentation or analyze customer lifetime value.

-  Some critical values like `Quantity` were missing and had to be estimated, which may introduce slight inaccuracies in sales metrics.

- Absence of exact time (hour/minute) in the transaction date limits hourly trend analysis and deeper time-based insights.

- Without customer location or age/gender information, regional or demographic-based insights could not be derived.


## Challenges Encountered & Solutions

| Challenge | Solution |
|----------|----------|
| MySQL skipping rows with blank values during import | Converted all columns to string in Excel to preserve full data before importing |
| Data type mismatches after import | Converted strings to proper types (FLOAT, INT, DATE) inside MySQL |
| Missing values in Quantity column | Used SQL to calculate and replace with category wise mean |
| formatting issues | Handled via Power Query before final load into Power BI |


## Contact 
- Email: mayowa24.jan@gmail.com

