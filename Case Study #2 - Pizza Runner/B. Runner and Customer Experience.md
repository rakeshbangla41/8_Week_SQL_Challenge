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

* In the first week of Jan, 2 new runners signed up
* In the second and third week of Jan, 1 new runner signed up

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

* On an average, it takes 12 minutes to prepare a single pizza
* For preparing two pizzas, it takes 21 minutes which means 10.5 minutes for each pizza
* To make 3 pizzas, it takes 29 minutes which means 9.6 minutes for each pizza
* To prepare 5 pizzas, it takes an average of 15 minutes which means 3 minutes to make each pizza

  From this data, we can say that more the number of pizzas to prepare in an order, lesser the time it will take

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

* On an average, Customer 105 has the highest distance to travel with 25 KM and Customer 104 has the least distance to travel with 10 KM 

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

* Runner 1's average speed ranges from 37.5 KM/hr to 60 KM/hr
* Runner 2's average speed ranges from 35.1 KM/hr to 93.6 KM/hr. The difference of speed between lowest and highest is almost 3 times. We can further investigate to know the eaxct reason how and why the 93.6 KM/hr order was delivered so fast
* Runner 3's average speed was 40 KM/hr

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

**Answer:**


![b7ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/8fae0d4f-1318-4290-b912-1c659b2c023f)














