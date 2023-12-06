# Case Study #2 - Pizza Runner

## C. Ingredient Optimisation

## Questions and Solutions:

##### Note: I have taken the screenshot of each answer in MySQL Workbench and attached the same here under Answer section.

**Q1. What are the standard ingredients for each pizza?**

```
WITH pizza_id_toppings_join AS
(SELECT 
  pizza_id, topping_name 
FROM pizza_recipes pr 
JOIN pizza_toppings pt 
ON pr.toppings=pt.topping_id 
GROUP BY pizza_id, topping_name)

SELECT 
  pizza_id, GROUP_CONCAT(topping_name) AS standard_ingredients 
FROM pizza_id_toppings_join 
GROUP BY pizza_id;

```

**Answer:**


![c1ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/a5f5d67a-7fd1-404c-9d11-24b2f07d7c73)


**Q2. What was the most commonly added extra?**

```
SELECT
  topping_name, COUNT(*) AS no_of_times_added
FROM customer_orders_temp cot
JOIN pizza_toppings pt
ON cot.extras=pt.topping_id 
WHERE extras IS NOT NULL
GROUP BY topping_name
ORDER BY no_of_times_added DESC LIMIT 1;

```

**Answer:**


![c2ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/a306d19e-a30b-4a55-a638-1f96e310534f)


**Q3. What was the most common exclusion?**

```
SELECT
  topping_name, COUNT(*) AS no_of_times_excluded
FROM customer_orders_temp cot
JOIN pizza_toppings pt
ON cot.exclusions = pt.topping_id 
WHERE exclusions IS NOT NULL
GROUP BY topping_name
ORDER BY no_of_times_excluded DESC LIMIT 1;

```

**Answer:**


![c3ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/4fe9165b-0870-4cac-8a05-208a4919494c)


**Q4. What is the total quantity of each ingredient used in all delivered pizzas sorted by most frequent first?**

**Solution - 1:**

```
WITH table_joins_with_no_cancellations AS
(SELECT 
  cot.order_id, cot.pizza_id, pr.toppings, pt.topping_name, cot.exclusions, cot.extras
FROM customer_orders_temp cot 
JOIN runner_orders_temp rot 
ON cot.order_id = rot.order_id 
JOIN pizza_recipes pr 
ON cot.pizza_id = pr.pizza_id 
JOIN pizza_toppings pt 
ON pr.toppings=pt.topping_id
WHERE rot.cancellation IS NULL 
ORDER BY cot.order_id)

SELECT 
  topping_name, COUNT(*) AS no_of_times_used 
FROM table_joins_with_no_cancellations 
GROUP BY topping_name 
ORDER BY no_of_times_used DESC;

```

**Solution - 2:**

```
WITH table_joins_with_no_cancellations AS
(SELECT 
  cot.order_id, cot.pizza_id, pr.toppings, pt.topping_name, cot.exclusions, cot.extras
FROM customer_orders_temp cot 
JOIN runner_orders_temp rot 
ON cot.order_id = rot.order_id 
JOIN pizza_recipes pr 
ON cot.pizza_id = pr.pizza_id 
JOIN pizza_toppings pt 
ON pr.toppings = pt.topping_id
WHERE rot.cancellation IS NULL 
ORDER BY cot.order_id),

topping_count AS
(SELECT 
  topping_name, COUNT(*) AS no_of_times_used 
FROM table_joins_with_no_cancellations 
GROUP BY topping_name, exclusions, extras 
ORDER BY no_of_times_used DESC)

SELECT 
  topping_name, SUM(no_of_times_used) AS total_no_of_times_used 
FROM topping_count 
GROUP BY topping_name 
ORDER BY total_no_of_times_used DESC;

```

**Answer:**


![c6ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/8159b3e4-c783-49c4-a440-d67d8c0365b2)




















