# Case Study #5 - Data Mart

## D. Bonus Question

## Questions and Solutions:

##### Note: I have taken the screenshot of each answer in MySQL Workbench and attached the same here under Answer section.

**Q1. Which areas of the business have the highest negative impact in sales metrics performance in 2020 for the 12 week before and after period?**

1) region  
2) platform  
3) age_band  
4) demographic  
5) customer_type  

Do you have any further recommendations for Danny’s team at Data Mart or any interesting insights based off this analysis?  

**1) region:**

```

WITH sales_before_after AS
(SELECT 
  region,
  SUM(CASE 
          WHEN (week_number BETWEEN 12 AND 23) AND (calendar_year="2020") THEN sales 
	END) AS before_change_sales,
  SUM(CASE
          WHEN (week_number BETWEEN 24 AND 35) AND (calendar_year="2020") THEN sales 
	END) AS after_change_sales
FROM clean_weekly_sales
GROUP BY region 
ORDER BY region)


SELECT 
  *, (after_change_sales - before_change_sales) AS difference, 
  100*((after_change_sales - before_change_sales)/before_change_sales) AS pct_variance 
FROM sales_before_after;

```

**Answer:**


![d1ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/83073b77-bb41-4f29-8978-142c54e93917)
















