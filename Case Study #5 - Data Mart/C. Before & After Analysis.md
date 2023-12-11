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


![c1ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/ebc07984-b9be-4e02-a5bf-b47833cce4cd)


* After the implementation of sustainable packaging, there has been a decrease of  $26,884,188 in sales. The percentage change in sales is about -1.146% compared to 4 weeks before the change.
   
* This indicates that the new plan of implementing sustainable packaging has not yet gained positive feedback from the customers. This negative change and impact usually happens in the market with the advent of new methodologies and processess since the general public is not acquinted to such things.


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

![c2ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/349542a0-8c0c-4e7c-8daa-e2e0105d17a2)  


* Similarly, as we have seen in the previous scenario, implementation of sustainable packaging has negative impact on the sales. There has been a decrease of $152,325,394 in sales which amounts to -2.14% percentage change in sales.


**Q3. How do the sale metrics for these 2 periods before and after compare with the previous years in 2018 and 2019?**


**4 week:**

```

WITH sales_per_week_before_after AS
(SELECT calendar_year,
	SUM(CASE 
		WHEN (week_number BETWEEN 20 AND 23) THEN sales 
	END) AS sales_per_week_before,
	SUM(CASE
		WHEN (week_number BETWEEN 24 AND 27) THEN sales 
	END) AS sales_per_week_after
FROM clean_weekly_sales
GROUP BY calendar_year, week_number 
ORDER BY week_number),

total_sales_before_after AS
(SELECT 
  calendar_year, SUM(sales_per_week_before) AS before_change_sales, SUM(sales_per_week_after) AS after_change_sales  
FROM sales_per_week_before_after 
GROUP BY calendar_year 
ORDER BY calendar_year)

SELECT 
  calendar_year, before_change_sales, after_change_sales, (after_change_sales - before_change_sales) AS difference, 
  (100*(after_change_sales - before_change_sales)/before_change_sales) AS pct_variance 
FROM total_sales_before_after;

```

**Answer:**

![c3ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/1e0d699e-ae18-4819-9000-7cb5835c6a43)

**4 week:**

* In 2018, there is a positive change of 0.19% compared to the 4 week period before 2018-06-15     
  
* Similarly, in the year 2019, there is 0.10% positive change in sales compared to the 4 week period before the 2019-06-15
      
* But in 2020, after the implemantation of sustainable packaging, there is a decrease in sales which is about -1.146%
  
* This indicates that coming up with new processes and advancements may not always guarantee a positive result


**# 12 week:**

```

WITH sales_per_week_before_after AS
(SELECT calendar_year,
	SUM(CASE 
	        WHEN (week_number BETWEEN 12 AND 23) THEN sales 
	END) AS sales_per_week_before,
	SUM(CASE
                WHEN (week_number BETWEEN 24 AND 35) THEN sales 
	END) AS sales_per_week_after
FROM clean_weekly_sales
GROUP BY calendar_year, week_number 
ORDER BY week_number),

total_sales_before_after AS
(SELECT 
  calendar_year, SUM(sales_per_week_before) AS before_change_sales, SUM(sales_per_week_after) AS after_change_sales 
FROM sales_per_week_before_after 
GROUP BY calendar_year 
ORDER BY calendar_year)

SELECT 
  calendar_year, before_change_sales, after_change_sales, (after_change_sales - before_change_sales) AS difference, 
  (100*(after_change_sales - before_change_sales)/before_change_sales) AS pct_variance 
FROM total_sales_before_after;

```

**Answer:**

![c4ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/1582e5c3-cac2-4655-a3c6-3f5b603b4489)


* We could see a good 1.62% increase in sales in 2018 compared to 12 weeks before  2018-06-15
  
* In 2019, there is a decline in sales percentage by -0.3% compared to 12 weeks before 2019-06-15
  
* But in 2020, after the implementation of sustainable packaging, there is a significant amount of decrease in sales by -2.137% compared to 12 weeks before the packaging change
 
* This difference further more becomes damaging when we compare it with the best performing year 2018. There is a huge difference of -3.77% decrease in sales in 2020 compared to 2018













