# Case Study #5 - Data Mart

## A. Data Cleaning and Transformation:  

All the information presented here is taken from: [Data Mart](https://8weeksqlchallenge.com/case-study-5/)

In a single query, perform the following operations and generate a new table in the data_mart schema named clean_weekly_sales:  

•	Convert the week_date to a DATE format  

•	Add a week_number as the second column for each week_date value, for example any value from the 1st of January to 7th of January will be 1, 8th to 14th will be 2 etc  

•	Add a month_number with the calendar month for each week_date value as the 3rd column  

•	Add a calendar_year column as the 4th column containing either 2018, 2019 or 2020 values  

•	Add a new column called age_band after the original segment column using the following mapping on the number inside the segment value   



| segment  | age_band |
| --------- | -------- |
| 1  | Young Adults  |
| 2  | Middle Aged  |
| 3or 4  | Retirees  |






