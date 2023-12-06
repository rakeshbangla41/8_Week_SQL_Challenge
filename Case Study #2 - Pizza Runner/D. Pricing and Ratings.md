# Case Study #2 - Pizza Runner

## D. Pricing and Ratings

## Questions and Solutions:

##### Note: I have taken the screenshot of each answer in MySQL Workbench and attached the same here under Answer section.

**Q1. If a Meat Lovers pizza costs $12 and Vegetarian costs $10 and there were no charges for changes - how much money has Pizza Runner made so far if there are no delivery fees?**

```
WITH calculated_amount as
(SELECT
  cot.order_id, 
CASE
     WHEN pizza_id = 1 THEN 12 ELSE 10 
END AS price
FROM customer_orders_temp cot  
JOIN runner_orders_temp rot
ON cot.order_id = rot.order_id
WHERE cancellation IS NULL)

SELECT
  SUM(price) AS money_earned
FROM calculated_amount;

```

**Answer:**


![d1ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/5f9fd883-2390-42c8-9559-b154f3745783)


**Q2. What if there was an additional $1 charge for any pizza extras? Example: Add cheese is $1 extra**

```
WITH calculated_amount AS
(SELECT
  cot.order_id,
CASE
     WHEN pizza_id = 1 THEN 12 ELSE 10 
END AS price,
CASE
     WHEN EXTRAS IS NULL THEN 0 ELSE 1
END AS extras_price
FROM customer_orders_temp cot  
JOIN runner_orders_temp rot
ON cot.order_id = rot.order_id
WHERE cancellation IS NULL),

calculated_amount_w_extras AS
(SELECT
  order_id, SUM(price+extras_price) AS total_amount
FROM calculated_amount
GROUP BY order_id)

SELECT
  SUM(total_amount) AS money_earned_with_extras
FROM calculated_amount_w_extras;

```

**Answer:**


![d2ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/18574b19-5cc9-4eb0-adc2-45b036fd88a8)


**Q3. The Pizza Runner team now wants to add an additional ratings system that allows customers to rate their runner, how would you design an additional table for this new dataset, 
generate a schema for this new table and insert your own data for ratings for each successful customer order between 1 to 5.**

```
DROP TABLE IF EXISTS customer_rating;

CREATE TABLE customer_rating (
            order_id INTEGER,
            customer_id INTEGER,
            runner_id INTEGER,
            rating INTEGER,
            comments VARCHAR(120)
            );
            
INSERT INTO customer_rating 
	(order_id, customer_id, runner_id, rating , comments)
VALUES 
    ('1', '101', '1', '1', "delayed delivery, the pizza was cold when it reached my home"),
    ('2', '101', '1', '4', "on time delivery"),
    ('3', '102', '2', '3', "good"),
    ('8', '106', '3', '5', "the delivery was on time. The delivery executive was polite and professional"),
    ('10', '104', '1', '4', "delivered on time, professional service");

SELECT * FROM customer_rating;

```

**Answer:**


![d3ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/b8828030-3b69-4420-8aab-9377d954e969)


**Q4. Using your newly generated table - can you join all of the information together to form a table which has the following information for successful deliveries?**

```
SELECT 
  DISTINCT(cr.order_id), cr.customer_id, cr.runner_id, order_time, pickup_time, TIMESTAMPDIFF(MINUTE, order_time, pickup_time) AS time_difference_min, duration, 
  ROUND(distance/(duration/60), 1) AS speed_kmperhr, rating, comments 
FROM customer_rating cr 
JOIN customer_orders_temp cot 
ON cr.order_id = cot.order_id 
JOIN runner_orders_temp rot 
ON cr.order_id = rot.order_id;

```

**Answer:**


![d4ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/ea947125-83d0-485a-b8eb-976080a92c78)


**Q5. If a Meat Lovers pizza was $12 and Vegetarian $10 fixed prices with no cost for extras and each runner is paid $0.30 per kilometre traveled,
how much money does Pizza Runner have left over after these deliveries?**

```
WITH calculated_amount_w_distance AS
(SELECT
  cot.order_id, cot.pizza_id, cot.exclusions, cot.extras, 
CASE
	WHEN pizza_id = 1 THEN 12 ELSE 10 
END AS price,
rot.runner_id, rot.distance, ROUND((0.30*rot.distance), 2) AS distance_amount_paid_for_runner
FROM customer_orders_temp cot 
JOIN runner_orders_temp rot
ON cot.order_id = rot.order_id
WHERE rot.cancellation IS NULL)

SELECT
  SUM(price) AS total_amount_earned, ROUND(SUM(distance_amount_paid_for_runner), 2) AS total_amount_paid_for_runners,  
  SUM(price) - ROUND(SUM(distance_amount_paid_for_runner), 2) AS money_left_with_pizza_runner
FROM calculated_amount_w_distance;

```

**Answer:**

![d5ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/70b3971b-d094-4719-be8c-c8a1ecb17ba0)














