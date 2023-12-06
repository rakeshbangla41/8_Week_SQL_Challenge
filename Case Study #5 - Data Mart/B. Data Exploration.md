# Case Study #5 - Data Mart

## A. Data Exploration

## Questions and Solutions:

##### Note: I have taken the screenshot of each answer in MySQL Workbench and attached the same here under Answer section.

**Q1. What day of the week is used for each week_date value?**

```
SELECT 
  DISTINCT(DAYNAME(week_date)) AS week_date_day 
FROM clean_weekly_sales;

```

**Answer:**


![b1ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/7ca0073c-0e05-474e-815e-e145b41aa205)


**Q2. How many total transactions were there for each year in the dataset?**

```
SELECT 
  calendar_year, COUNT(transactions) AS transcations_count 
FROM clean_weekly_sales 
GROUP BY calendar_year 
ORDER BY calendar_year;

```

**Answer:**

![b2ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/4ccc0894-76ac-4468-b532-306ff0985e9e)


**Q3. What is the total sales for each region for each month?**

```
SELECT 
  region, month_number, SUM(sales) AS total_sales_amount 
FROM clean_weekly_sales 
GROUP BY region, month_number 
ORDER BY region, month_number;

```

**Answer:**


![b3ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/a271ad38-a8d7-420d-8ab6-29eff9294662)


**Q4. What is the total count of transactions for each platform?**

```
SELECT
  platform, COUNT(*) AS transactions_count_for_platform
FROM clean_weekly_sales
GROUP BY platform;

```

**Answer:**


![b4ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/ae66a01b-3318-4c7a-9aa1-a18dfc3afeb6)


**Q5. What is the percentage of sales for Retail vs Shopify for each month?**

```
WITH monthly_sales AS
(SELECT 
  calendar_year, month_number, platform, SUM(sales) AS monthly_sales 
FROM clean_weekly_sales 
GROUP BY calendar_year, month_number, platform 
ORDER BY calendar_year, month_number, platform)

SELECT 
calendar_year, month_number,
ROUND(100*MAX(CASE
	WHEN platform="Retail" THEN monthly_sales ELSE NULL END)/SUM(monthly_sales), 2)
AS retail_sales_percentage,
ROUND(100*MAX(CASE
	WHEN platform="Shopify" THEN monthly_sales ELSE NULL END)/SUM(monthly_sales), 2)
AS shopify_sales_percentage
FROM monthly_sales
GROUP BY calendar_year, month_number;

```

**Answer:**


![b5ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/b0ca8234-2529-4b77-a9b2-0f4482343afb)


**Q6. What is the percentage of sales by demographic for each year in the dataset?**

```
WITH yearly_sales AS
(SELECT 
  calendar_year, demographic, SUM(sales) AS yearly_sales 
FROM clean_weekly_sales 
GROUP BY calendar_year, demographic 
ORDER BY calendar_year)

SELECT 
  calendar_year,
  ROUND(100*MAX(CASE WHEN demographic="Couples" THEN yearly_sales ELSE NULL END)/SUM(yearly_sales), 2) AS couples_percentage,
  ROUND(100*MAX(CASE WHEN demographic="Families" THEN yearly_sales ELSE NULL END)/SUM(yearly_sales), 2) AS Families_percentage,
  ROUND(100*MAX(CASE WHEN demographic="unknown" THEN yearly_sales ELSE NULL END)/SUM(yearly_sales), 2) AS unknown_percentage
FROM yearly_sales
GROUP BY calendar_year;

```

**Answer:**


![b6ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/b52ea1cb-37db-4623-b8b3-e15305cfe96b)


**Q7. Which age_band and demographic values contribute the most to Retail sales?**

```
WITH retail_sales AS 
(SELECT 
  age_band, demographic, SUM(sales) AS total_retail_sales 
FROM clean_weekly_sales 
WHERE platform = "Retail" 
GROUP BY age_band, demographic 
ORDER BY total_retail_sales DESC)

SELECT 
  age_band, demographic, total_retail_sales, 
  ROUND(100*(total_retail_sales/SUM(total_retail_sales) OVER()), 2) AS retail_sales_contribution_pct 
  FROM retail_sales 
  ORDER BY retail_sales_contribution_pct DESC;

```





















