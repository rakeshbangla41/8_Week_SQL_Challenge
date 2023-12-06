# Case Study #2 - Pizza Runner

## B. Runner and Customer Experience

## Questions and Solutions:

##### Note: I have taken the screenshot of each answer in MySQL Workbench and attached the same here under Answer section.

**Q1. How many runners signed up for each 1 week period? (i.e. week starts 2021-01-01)**

```
SELECT 
  WEEK(registration_date, 1) AS week_no, COUNT(*) AS runner_signup_count 
FROM runners
GROUP BY WEEK(registration_date, 1);

```

**Answer:**


![b1ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/5d51d291-9da3-4381-bb4e-cf90dc651830)


**Q2. What was the average time in minutes it took for each runner to arrive at the Pizza Runner HQ to pickup the order?**

```
WITH time_difference AS
(SELECT
  rot.runner_id, cot.order_id, TIMESTAMPDIFF(Minute, cot.order_time, rot.pickup_time) AS difference_in_minutes
FROM customer_orders_temp cot  
JOIN runner_orders_temp rot
ON cot.order_id = rot.order_id
WHERE rot.cancellation IS NULL
GROUP BY rot.runner_id, cot.order_id, cot.order_time, rot.pickup_time)

SELECT
  runner_id, ROUND(AVG(difference_in_minutes), 2) AS avg_pick_up_time_minutes
FROM time_difference
GROUP BY runner_id;

```

**Answer:**


![b2ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/94fc8d89-2f83-48dd-b615-ed1949146b98)


**Q3. Is there any relationship between the number of pizzas and how long the order takes to prepare?**

```
WITH no_pizzas_time_difference AS
(SELECT 
  rot.order_id, COUNT(*) AS no_of_pizzas_ordered, TIMESTAMPDIFF(Minute, cot.order_time, rot.pickup_time) AS difference_in_minutes 
FROM customer_orders_temp cot 
JOIN runner_orders_temp rot 
ON cot.order_id = rot.order_id 
WHERE rot.cancellation IS NULL
GROUP BY rot.order_id, cot.order_time, rot.pickup_time 
ORDER BY no_of_pizzas_ordered DESC)

SELECT 
  no_of_pizzas_ordered, ROUND(AVG(difference_in_minutes), 1) AS avg_time_to_prepare_minutes 
FROM no_pizzas_time_difference 
GROUP BY no_of_pizzas_ordered 
ORDER BY no_of_pizzas_ordered;

```

**Answer:**


![b3ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/d17b2c47-996c-423f-83b1-8a7b45ecb923)


**Q4. What was the average distance travelled for each customer?**

```
SELECT 
  customer_id, ROUND(AVG(distance), 2) AS avg_distance_travelled_KM 
FROM customer_orders_temp co 
JOIN runner_orders_temp ro 
ON co.order_id = ro.order_id 
GROUP BY customer_id;

```

**Answer:**

![b4ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/1bfd5ee8-515a-4a52-843c-aab6d039121a)


**Q5. What was the difference between the longest and shortest delivery times for all orders?**

```
SELECT 
  MAX(duration)-MIN(duration) AS duration_difference 
FROM runner_orders_temp;

```

**Answer:**


![b5ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/6592e51b-1731-4d5b-bbd3-eb739c5e744e)


**Q6. What was the average speed for each runner for each delivery and do you notice any trend for these values?**

```
SELECT 
  order_id, runner_id, distance, duration, ROUND(distance/(duration/60), 1) AS speed_kmperhr 
FROM runner_orders_temp 
WHERE cancellation IS NULL;

```

**Answer:**


![b6ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/13c5fb87-4c83-48fd-b632-74df30b5a089)

**Q7. What is the successful delivery percentage for each runner?**

```
WITH orders_assigned_delivered AS
(SELECT 
  runner_id, count(order_id) AS orders_assigned, COUNT(distance) AS orders_delivered 
FROM runner_orders_temp 
WHERE cancellation IS NULL 
GROUP BY runner_id)

SELECT 
  *, ROUND(100*orders_delivered/orders_assigned, 2) AS succ_delivery_pct 
FROM orders_assigned_delivered;

```


![b7ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/8fae0d4f-1318-4290-b912-1c659b2c023f)














