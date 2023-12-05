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

**1. What is the total amount each customer spent at the restaurant?**

```Red
SELECT 
  s.customer_id, SUM(m.price) AS total_amount_spent 
FROM sales s JOIN menu m 
  ON s.product_id=m.product_id 
GROUP BY customer_id ORDER BY s.customer_id;

```











