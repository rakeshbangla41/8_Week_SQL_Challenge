# Case Study #1 - Danny's Diner

![dannys_diner_img](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/0705c4b9-d5ae-4e2d-a883-29f256115210)

### Note: 
All the information presented here is taken from: [Danny's Diner](https://8weeksqlchallenge.com/case-study-1/)

## Problem Statement:

Danny owns a restaurant which serves Japanese food. The restaurant has captured some very basic data from their few months of operation but have no idea on how to use their data to help them run the business.   

Danny wants to use the data to answer a few simple questions about his customers, especially about their visiting patterns, how much money they’ve spent and also which menu items are their favourite. Having this deeper connection with his customers will help him deliver a better and more personalised experience for his loyal customers.  

He plans on using these insights to help him decide whether he should expand the existing customer loyalty program - additionally he needs help to generate some basic datasets so his team can easily inspect the data without needing to use SQL.  

## Entity Relationship Diagram:
![erd](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/7852f956-986f-4bc3-839e-7a0953b12872)

## Questions and Solutions:

**Q1. What is the total amount each customer spent at the restaurant?**


```
SELECT 
  s.customer_id, SUM(m.price) AS total_amount_spent 
FROM sales s JOIN menu m 
ON s.product_id=m.product_id 
GROUP BY customer_id
ORDER BY s.customer_id;

```
**Answer:**

![1ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/2869e429-edcb-4719-9c50-74be4be70454)

**Q2. How many days has each customer visited the restaurant?**

```
SELECT 
  customer_id, COUNT(DISTINCT(order_date)) AS no_of_days_visited 
FROM sales 
GROUP BY customer_id
ORDER BY customer_id;

```

**Answer:**

![2ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/1021d4c8-27fe-4682-8087-9bde5d372e79)


**Q3. What was the first item from the menu purchased by each customer?**

```
WITH ordered_items AS
(SELECT 
  customer_id, product_name, order_date, RANK() OVER(PARTITION BY customer_id ORDER BY order_date) AS rnk 
FROM sales s JOIN menu m 
ON s.product_id=m.product_id)

SELECT 
  customer_id, order_date, product_name 
FROM ordered_items 
WHERE rnk=1;

```

**Answer:**

![3ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/ae013149-9b4a-4a09-a57c-f7dd2ba3c88d)


**Q4. What is the most purchased item on the menu and how many times was it purchased by all customers?**

```
SELECT 
  product_name, COUNT(*) AS no_of_times_ordered 
FROM sales s JOIN menu m 
ON s.product_id = m.product_id 
GROUP BY product_name 
ORDER BY no_of_times_ordered DESC LIMIT 1;

```

**Answer:**

![4ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/126b9823-56fc-4608-ae7b-11ffd7635f2f)

**Q5. Which item was the most popular for each customer?**

```
WITH product_times_ordered AS
(SELECT 
  customer_id, product_name, COUNT(*) AS times_ordered 
FROM sales s JOIN menu m 
ON s.product_id=m.product_id 
GROUP BY customer_id, product_name),

product_times_ranked AS
(SELECT 
  *, RANK() OVER(PARTITION BY customer_id ORDER BY times_ordered DESC) AS rnk 
FROM product_times_ordered)

SELECT 
  customer_id, product_name, times_ordered 
FROM product_times_ranked 
WHERE rnk=1;

```

**Answer:**

![5ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/e82d694f-88b4-4782-bed2-a2c09bf52c8f)


**Q6. Which item was purchased first by the customer after they became a member?**

```
WITH member_joined_orders_after AS
(SELECT
  s.customer_id, m.product_name, join_date, order_date
FROM sales s JOIN members mm
ON s.customer_id = mm.customer_id
JOIN menu m
ON s.product_id = m.product_id 
WHERE order_date > join_date
ORDER BY s.customer_id, s.order_date),

orders_ranked AS
(SELECT
  *, RANK() OVER(PARTITION BY customer_id ORDER BY order_date) AS rnk
FROM member_joined_orders_after)

SELECT
  customer_id, product_name
FROM orders_ranked
WHERE rnk=1;

```

**Answer:**

![6ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/dcf5ddd1-bd73-429a-9210-714df2f3c4df)


**Q7. Which item was purchased just before the customer became a member?**

```
WITH member_joined_orders_before AS
(SELECT
  s.customer_id, m.product_name, join_date, order_date
FROM sales s JOIN members mm
ON s.customer_id = mm.customer_id
JOIN menu m
ON s.product_id = m.product_id 
WHERE order_date < join_date
ORDER BY s.customer_id, s.order_date),

orders_ranked AS
(SELECT
  *, RANK() OVER(PARTITION BY customer_id ORDER BY order_date) AS rnk
FROM member_joined_orders_before)

SELECT
  customer_id, product_name
FROM orders_ranked
WHERE rnk=1;

```

**Q8. What is the total items and amount spent for each member before they became a member?**

```
SELECT
  s.customer_id, COUNT(*) AS total_items_ordered_before_memebership, SUM(price) AS total_amount_spent_before_memebership
FROM sales s JOIN menu m
ON s.product_id = m.product_id 
JOIN members mb
ON s.customer_id = mb.customer_id
WHERE s.order_date < mb.join_date
GROUP BY customer_id ORDER BY customer_id;

```

**Answer:**

![8ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/bc661a05-16b7-4729-89fc-fd9bc65ae253)


**Q9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?**

```
WITH sales_products AS 
(SELECT
  s.customer_id, m.product_name, m.price
FROM sales s JOIN menu m
ON s.product_id=m.product_id), 

points_calculated AS
(SELECT
*, 
CASE 
	WHEN product_name = "sushi" THEN 20*price
    ELSE 10*price
END AS points
FROM sales_products)

SELECT
  customer_id, SUM(points) AS points_earned
FROM points_calculated
GROUP BY customer_id;

```

**Answer:**

![9ans](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/ec9059b1-bd4a-4838-994d-4e407e748631)


**Q10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?**

```
WITH joined_tables_sales_jan AS
(SELECT 
  s.customer_id, order_date, product_name, price, join_date, DATE_ADD(join_date, INTERVAL 6 DAY) AS first_week
FROM sales s JOIN members mm 
ON s.customer_id = mm.customer_id 
JOIN menu m 
ON s.product_id = m.product_id 
WHERE order_date BETWEEN "2021-01-01" AND "2021-01-31" 
ORDER BY customer_id, order_date),

points_calculated AS
(SELECT 
*,
CASE
	WHEN order_date BETWEEN join_date AND first_week THEN price*20
    WHEN product_name="sushi" THEN price*20
	ELSE price*10
END AS points
FROM joined_tables_sales_jan)

SELECT 
  customer_id, SUM(points) AS total_points_earned_till_jan 
FROM points_calculated 
GROUP BY customer_id 
ORDER BY customer_id;

```


















