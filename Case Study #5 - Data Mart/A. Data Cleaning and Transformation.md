# Case Study #5 - Data Mart

## A. Data Cleaning and Transformation:  

##### All the information presented here is taken from: [Data Mart](https://8weeksqlchallenge.com/case-study-5/)   



**In a single query, perform the following operations and generate a new table in the data_mart schema named clean_weekly_sales:**  

•	Convert the week_date to a DATE format  

•	Add a week_number as the second column for each week_date value, for example any value from the 1st of January to 7th of January will be 1, 8th to 14th will be 2 etc  

•	Add a month_number with the calendar month for each week_date value as the 3rd column  

•	Add a calendar_year column as the 4th column containing either 2018, 2019 or 2020 values  

•	Add a new column called age_band after the original segment column using the following mapping on the number inside the segment value:   



| segment  | age_band |
| --------- | -------- |
| 1  | Young Adults  |
| 2  | Middle Aged  |
| 3 or 4  | Retirees  |



• Add a new demographic column using the following mapping for the first letter in the segment values:


| segment  | age_band |
| --------- | -------- |
| C  | Couples  |
| F  | Families  |


• Ensure all null string values with an "unknown" string value in the original segment column as well as the new age_band and demographic columns

• Generate a new avg_transaction column as the sales value divided by transactions rounded to 2 decimal places for each record


**Data Cleaning & Transformation Code:**

```

DROP TABLE IF EXISTS weekly_sales_new;

CREATE TABLE weekly_sales_new AS

SELECT 
  str_to_date(week_date, "%d/%m/%y") AS week_date_new, region AS region, platform AS platform, 
segment AS segment, customer_type AS customer_type, transactions AS transactions, sales AS sales 
FROM weekly_sales; 


DROP TABLE IF EXISTS clean_weekly_sales;

CREATE TABLE clean_weekly_sales AS

SELECT 
  week_date_new AS week_date, WEEK(week_date_new) AS week_number, MONTH(week_date_new) AS month_number, 
YEAR(week_date_new) AS calendar_year, region AS region, platform AS platform, segment AS segment, customer_type AS customer_type, 
CASE
	WHEN segment LIKE "%1" THEN "Young Adults"
    WHEN segment LIKE "%2" THEN "Middle Aged"
    WHEN segment LIKE "%3" THEN "Retirees"
    WHEN segment LIKE "%4" THEN "Retirees"
	ELSE "unknown"
END AS age_band,
CASE
	WHEN segment LIKE "C%" THEN "Couples"
    WHEN segment LIKE "F%" THEN "Families"
    ELSE "unknown"
END AS demographic,
transactions AS transactions,
sales AS sales, 
ROUND(sales/transactions, 2) AS avg_sales
FROM weekly_sales_new;

```
















