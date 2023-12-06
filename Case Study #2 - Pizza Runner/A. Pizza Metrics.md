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


**Q4. How many of each type of pizza was delivered**?

```
SELECT
  pn.pizza_name, COUNT(*) AS no_of_pizzas_delivered
FROM customer_orders_temp cot
JOIN pizza_names pn
ON cot.pizza_id = pn.pizza_id
JOIN runner_orders_temp rot
ON cot.order_id = rot.order_id
WHERE distance !=0
GROUP BY pn.pizza_name;

```

**Answer:**


![a4ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/d72222e1-1306-41e7-9cf8-7b895483d9c6)


**Q5. How many Vegetarian and Meatlovers were ordered by each customer?**

```
SELECT
  cot.customer_id, pn.pizza_name, COUNT(*) as no_of_orders
FROM customer_orders_temp cot
JOIN pizza_names pn
ON cot.pizza_id = pn.pizza_id 
GROUP BY cot.customer_id, pn.pizza_name
ORDER BY customer_id;

```

**Answer:**


![a5ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/51b271d3-16f4-4974-93ac-daaf8abc2dae)


**Q6. What was the maximum number of pizzas delivered in a single order?**

```
SELECT 
  cot.order_id, count(cot.pizza_id) AS max_no_of_pizzas_delivered 
FROM customer_orders_temp cot 
JOIN runner_orders_temp rot 
ON cot.order_id = rot.order_id 
WHERE rot.distance !=0
GROUP BY cot.order_id 
ORDER BY max_no_of_pizzas_delivered DESC LIMIT 1;

```

**Answer:**


![a6ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/251b894c-79bb-4282-9197-8912f278e6ac)


**Q7. For each customer, how many delivered pizzas had at least 1 change and how many had no changes?**

**no changes -**

```
SELECT 
  customer_id, COUNT(*) AS no_changes 
FROM customer_orders_temp cot 
JOIN runner_orders_temp rot 
ON cot.order_id = rot.order_id 
WHERE rot.distance !=0 AND (exclusions IS NULL AND extras IS NULL) 
GROUP BY customer_id;

```

**Answer:**


![a7aans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/8cc78794-4908-4468-b198-5031f5754383)

**at least 1 change -**

```
SELECT 
  customer_id, COUNT(*) as at_least_one_change 
FROM customer_orders_temp cot 
JOIN runner_orders_temp rot 
ON cot.order_id = rot.order_id 
WHERE rot.distance !=0 AND (exclusions IS NULL AND extras IS NOT NULL) OR (exclusions IS NOT NULL AND extras IS NULL)
GROUP BY customer_id;

```

**Answer:**


![a7bans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/c131e5d3-3d9d-4795-a15f-a77d36dbcee5)


**Q8. How many pizzas were delivered that had both exclusions and extras?**

```
SELECT
  COUNT(*) AS both_exclusions_and_extras
FROM customer_orders_temp cot
JOIN runner_orders_temp rot
ON cot.order_id = rot.order_id
WHERE rot.distance !=0 AND (exclusions IS NOT NULL AND extras IS NOT NULL);

```

**Answer:**


![a8ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/85777371-b536-40bb-9b64-ead35aec9773)


**Q9. What was the total volume of pizzas ordered for each hour of the day?**

```
WITH order_hour AS
(SELECT 
  *, HOUR(order_time) AS hour_of_day 
FROM customer_orders_temp)

SELECT 
  hour_of_day, COUNT(*) AS no_of_pizzas_ordered 
FROM order_hour 
GROUP BY hour_of_day 
ORDER BY no_of_pizzas_ordered DESC;

```

**Answer:**


![a9ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/8633c93c-343c-4a80-9b10-b44d3de9f18a)


**Q10. What was the volume of orders for each day of the week?**

```
WITH day_name AS
(SELECT 
  *, DAYNAME(order_time) AS day_of_week 
FROM customer_orders_temp)

SELECT 
  day_of_week, COUNT(*) AS no_of_pizzas_ordered 
FROM day_name 
GROUP BY day_of_week 
ORDER BY no_of_pizzas_ordered DESC;

```

**Answer:**


![a10ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/62abc25f-7763-45f7-a574-716194a2e9c4)




















