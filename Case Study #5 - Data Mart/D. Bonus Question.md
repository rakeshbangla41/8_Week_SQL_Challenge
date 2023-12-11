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

* There is a decrease in sales by -2.4% in retail sales after packaging change
  
* Sales made through Shopify have increased by about 7.18% post sustainable packaging. This indicates that online customers are much more susceptible and positively respond 
  to change than retail customers and Shopify is a good platform for implementing such changes in the future too

  


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

* There is a negative impact on the sales irrespective of the age band
  
* The highest negative impact is from the age-band 'unknown' (-3.34%) while the least negative impact is from Young Adults(-0.92%)


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

* Sustainable packaging had negative impact on sales irrespective of the demographic
  
* The impact varies from -3.34 (unknown) to -0.86 (Couples)
  

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


* Only New customers responded positively to the new packaging change with 1% increase in sales compared to previou 12 weeks
* Both Guest and Existing customers have shown negative impact on the new change with -3% and -2.2% decline in sales respectively

  

### Conclusion:

#### Which areas of the business have the highest negative impact in sales metrics performance in 2020 for the 12 week before and after period?**

The areas which had the highest negative impact on the sales in 2020 for 12 weeks after the launch of new sustainable packaging are:

* Age-band and demographic 'unknown' recorded the most negative impact on sales with -3.34% decrease 
* region 'Asia' recorded the second most negative impact on sales with -3.26% decline

  

#### Do you have any further recommendations for Danny’s team at Data Mart or any interesting insights based off this analysis?**

**After looking at the negative impact of sustainable packaging change, these are my analysis and suggestions to Danny's team at Data Mart:**

* Before the implementation of sustainable packaging, the team at Data Mart should have created awareness among the general consumer about the positive benefits of sustainable packaging and the use of bio-degradable materials.
* The general consumers may not know the toxic affects of normal plastic packaging and its negative affect on the environment.
* We should create awareness among the people on how the usage of sustainable packaging and bio-degradable materials can result in a better and greener environment. When we do this, there are definetly more chances of consumer's reacting positively to this change.




