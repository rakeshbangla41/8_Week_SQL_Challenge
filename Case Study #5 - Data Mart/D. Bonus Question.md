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

**region:**

* The new sustainable packaging change had negative impact on all the region’s except for Europe. Europe is the only region which had an increase in sales by about +4.73%  which is quite significant given the context

* The highest negative impact was in Asia with -3.26% decrease in sales    



**2) platform:**

```

WITH sales_before_after AS
(SELECT 
  platform,
  SUM(CASE 
          WHEN (week_number BETWEEN 12 AND 23) AND (calendar_year="2020") THEN sales 
	END) AS before_change_sales,
  SUM(CASE
          WHEN (week_number BETWEEN 24 AND 35) AND (calendar_year="2020") THEN sales 
	END) AS after_change_sales
FROM clean_weekly_sales
GROUP BY platform 
ORDER BY platform)


SELECT 
  *, (after_change_sales - before_change_sales) AS difference, 
  100*((after_change_sales - before_change_sales)/before_change_sales) AS pct_variance 
FROM sales_before_after;

```

**Answer:**


![d2ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/35f45945-2a22-40ef-8440-dc7f407e8d3f)


**3) age_band:**

```

WITH sales_before_after AS
(SELECT 
  age_band,
  SUM(CASE 
          WHEN (week_number BETWEEN 12 AND 23) AND (calendar_year="2020") THEN sales 
	END) AS before_change_sales,
  SUM(CASE
          WHEN (week_number BETWEEN 24 AND 35) AND (calendar_year="2020") THEN sales 
	END) AS after_change_sales
FROM clean_weekly_sales
GROUP BY age_band 
ORDER BY age_band)


SELECT 
  *, (after_change_sales - before_change_sales) AS difference, 
  100*((after_change_sales - before_change_sales)/before_change_sales) AS pct_variance 
FROM sales_before_after;

```

**Answer:**


![d3ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/33cdcfcf-2658-4f82-b776-f1049ebb6382)



**4) demographic:**

```

WITH sales_before_after AS
(SELECT 
  demographic,
  SUM(CASE 
          WHEN (week_number BETWEEN 12 AND 23) AND (calendar_year="2020") THEN sales 
	END) AS before_change_sales,
  SUM(CASE
          WHEN (week_number BETWEEN 24 AND 35) AND (calendar_year="2020") THEN sales 
	END) AS after_change_sales
FROM clean_weekly_sales
GROUP BY demographic 
ORDER BY demographic)


SELECT 
  *, (after_change_sales - before_change_sales) AS difference, 
  100*((after_change_sales - before_change_sales)/before_change_sales) AS pct_variance 
FROM sales_before_after;

```

**Answer:**


![d4ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/ec917817-e67b-4882-92ed-483a5023ebe1)



**5) customer_type:**

```

WITH sales_before_after AS
(SELECT 
  customer_type,
  SUM(CASE 
          WHEN (week_number BETWEEN 12 AND 23) AND (calendar_year="2020") THEN sales 
	END) AS before_change_sales,
  SUM(CASE
          WHEN (week_number BETWEEN 24 AND 35) AND (calendar_year="2020") THEN sales 
	END) AS after_change_sales
FROM clean_weekly_sales
GROUP BY customer_type 
ORDER BY customer_type)


SELECT 
  *, (after_change_sales - before_change_sales) AS difference, 
  100*((after_change_sales - before_change_sales)/before_change_sales) AS pct_variance 
FROM sales_before_after;

```

**Answer:**


![d5ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/53c298ee-fede-4e60-92ef-ce97ba61f693)








