# Case Study #5 - Data Mart

## C. Before & After Analysis

## Questions and Solutions:

##### Note: I have taken the screenshot of each answer in MySQL Workbench and attached the same here under Answer section.

**Q1. What is the total sales for the 4 weeks before and after 2020-06-15? What is the growth or reduction rate in actual values and percentage of sales?**

```

WITH sales_per_week_before_after AS
(SELECT 
    SUM(CASE 
            WHEN (week_number BETWEEN 20 AND 23) AND (calendar_year="2020") THEN sales 
    END) AS sales_per_week_before,
    SUM(CASE
            WHEN (week_number BETWEEN 24 AND 27) AND (calendar_year="2020") THEN sales 
    END) AS sales_per_week_after
FROM clean_weekly_sales
GROUP BY calendar_year, week_number, week_date 
ORDER BY week_number),

total_sales_before_after AS
(SELECT
  SUM(sales_per_week_before) AS before_change_sales, SUM(sales_per_week_after) AS after_change_sales 
FROM sales_per_week_before_after)

SELECT 
  before_change_sales, after_change_sales, (after_change_sales - before_change_sales) AS difference, 
  (100*(after_change_sales - before_change_sales)/before_change_sales) AS pct_variance 
FROM total_sales_before_after;

```

**Answer:**


![b1ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/1f69f727-4765-4963-9899-3736bd81a099)


**Q2. What about the entire 12 weeks before and after?**

```

WITH sales_per_week_before_after AS
(SELECT 
    SUM(CASE 
		    WHEN (week_number BETWEEN 12 AND 23) AND (calendar_year="2020") THEN sales 
	END) AS sales_per_week_before,
    SUM(CASE
		    WHEN (week_number BETWEEN 24 AND 35) AND (calendar_year="2020") THEN sales 
	END) AS sales_per_week_after
FROM clean_weekly_sales
GROUP BY calendar_year, week_number, week_date 
ORDER BY week_number),

total_sales_before_after AS
(SELECT 
  SUM(sales_per_week_before) AS before_change_sales, SUM(sales_per_week_after) AS after_change_sales 
FROM sales_per_week_before_after)

SELECT 
  before_change_sales, after_change_sales, (after_change_sales - before_change_sales) as difference, 
  (100*(after_change_sales - before_change_sales)/before_change_sales) AS pct_variance 
FROM total_sales_before_after;

```

**Answer:**

![b2ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/763442d5-5b26-4a0d-b76f-f865ba9574b0)














