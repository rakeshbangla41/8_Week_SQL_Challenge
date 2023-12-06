# Case Study #2 - Pizza Runner

## Data Cleaning and Transformation:   


**1) Table 'customer_orders' before data cleaning and transformation:**  


![table2_customer_orders_before_data_cleaning](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/26834858-c3a0-4b79-82ac-530c5558f563)  


* Both the columns **'exclusions'** and **'extras'** have missing and null values.

* In this particular scenario, it is better to replace both missing and null values with **'NULL'**. This will have no impact on the output.

**We will create a temporary table called 'customer_orders_temp' and will use this table from now on for analysis.**

```
DROP TABLE IF EXISTS customer_orders_temp;

CREATE TEMPORARY TABLE customer_orders_temp AS
SELECT
       order_id,
       customer_id,
       pizza_id,
       CASE
           WHEN exclusions = '' THEN NULL
           WHEN exclusions = 'null' THEN NULL
           ELSE exclusions
       END AS exclusions,
       CASE
           WHEN extras = '' THEN NULL
           WHEN extras = 'null' THEN NULL
           ELSE extras
       END AS extras,
       order_time
FROM customer_orders;

```

**Table 'customer_orders' after data cleaning and transformation:**  


![table2_customer_orders_after_data_cleaning](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/8385e5ab-8feb-4d39-b2a7-2d91ed9bfc42)  



**2) Table 'runner_orders' before data cleaning and transformation:**  


![table3_runner_orders_before_data_cleaning](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/2df838f4-7a73-4ef5-9fc4-0c7f61251c02)  


* In **'pickup_time'** column, remove **'null'** and replace with **'NULL'**.
* In **'distance**' column, remove **'km'** and replace with blank space **''**, remove **'null'** and replace with **'NULL'**.
* In **'duration'** column, remove **'minutes'** and replace with blank space **''**, remove **'null'** and replace with **'NULL'**.
* In **'cancellation'** column, remove **'NaN'** and empty spaces and replace with **'NULL'**.
 
**We will create a temporary table called 'runner_orders_temp' and will use this table from now on for analysis.**

```
DROP TABLE IF EXISTS runner_orders_temp;

CREATE TEMPORARY TABLE runner_orders_temp AS
SELECT
       order_id,
       runner_id,
       CASE
           WHEN pickup_time LIKE 'null' THEN NULL
           ELSE pickup_time
       END AS pickup_time,
       CASE
           WHEN distance LIKE 'null' THEN NULL
           ELSE CAST(regexp_replace(distance, '[a-z]+', '') AS FLOAT)
       END AS distance,
       CASE
           WHEN duration LIKE 'null' THEN NULL
           ELSE CAST(regexp_replace(duration, '[a-z]+', '') AS FLOAT)
       END AS duration,
       CASE
           WHEN cancellation LIKE '' THEN NULL
           WHEN cancellation LIKE 'null' THEN NULL
           ELSE cancellation
       END AS cancellation
FROM runner_orders;

```

**Table 'runner_orders' after data cleaning and transformation:**  


![table3_runner_orders_after_data_cleaning](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/cf4c43e7-9860-47ab-b129-5557abc23385)   


### * All the other tables are good to go for analysis and does not need any data cleaning and transformation. So, I'm keeping them as it is.




