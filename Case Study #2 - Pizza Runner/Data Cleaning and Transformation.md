# Case Study #2 - Pizza Runner

## Data Cleaning and Transformation:

**Table 'customer_orders' before data cleaning and transformation:**

![table2_customer_orders_before_data_cleaning](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/26834858-c3a0-4b79-82ac-530c5558f563)

* Both the columns **'exclusions'** and **'extras'** have missing and null values.

* In this particular scenario, it is better to replace both missing and null values with **'NULL'**. This will have no impact on the output.

**Table 'customer_orders' after data cleaning and transformation:**

![table2_customer_orders_after_data_cleaning](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/8385e5ab-8feb-4d39-b2a7-2d91ed9bfc42)


**Table 'runner_orders' before data cleaning and transformation:**

![table3_runner_orders_before_data_cleaning](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/2df838f4-7a73-4ef5-9fc4-0c7f61251c02)

* In **'pickup_time'** column, remove **'null'** and replace with **'NULL'**.
* In **'distance**' column, remove **'km'** and replace with blank space **''**, remove **'null'** and replace with **'NULL'**.
* In **'duration'** column, remove **'minutes'** and replace with blank space **''**, remove **'null'** and replace with **'NULL'**.
* In **'cancellation'** column, remove **'NaN'** and empty spaces and replace with **'NULL'**.

**Table 'runner_orders' after data cleaning and transformation:**

![table3_runner_orders_after_data_cleaning](https://github.com/rakeshbangla41/8_Week_SQL_Challenge/assets/132288134/cf4c43e7-9860-47ab-b129-5557abc23385)






