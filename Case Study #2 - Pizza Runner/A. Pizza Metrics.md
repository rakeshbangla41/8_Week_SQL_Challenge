# Case Study #2 - Pizza Runner

## A. Pizza Metrics

## Questions and Solutions:

##### Note: I have taken the screenshot of each answer in MySQL Workbench and attached the same here under Answer section.

**Q1. How many pizzas were ordered?**

```
SELECT
  COUNT(*) AS no_of_pizzas_ordered
FROM customer_orders_temp;

```

**Answer:**


![a1ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/675d3fc9-79e1-459d-85fb-2aa4f865ef45)


**Q2. How many unique customer orders were made?**

```
SELECT 
  COUNT(DISTINCT order_id) AS unique_no_of_orders 
FROM customer_orders_temp;

```

**Answer:**


![a2ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/b4925714-7497-4042-b721-4af3e105c243)


**Q3. How many successful orders were delivered by each runner?**

```
SELECT 
  runner_id, COUNT(*) AS successful_deliveries 
FROM runner_orders_temp 
WHERE distance != 0 
GROUP BY runner_id;

```

**Answer:**


![a3ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/7ac57176-be69-4bbb-8e3a-1147b573cd05)



