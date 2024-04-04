### [Source of this study material : Applied SQL For Data Analytics / Data Science With BigQuery by Jeff James](https://www.udemy.com/course/applied-sql-for-data-analytics-data-science-with-bigquery/)


## Join's mini project

- This time, we want to know the revenue trend by hour. If the site updates at 2AM and slows down for 20 minutes, are we risking revenue? We will use orders table and order_payments table to get the answer.


```
SELECT EXTRACT(hour FROM o.order_purchase_timestamp) AS order_hour, SUM(op.payment_value) AS sum_of_payment
FROM  `jrjames83-1171.sampledata.orders` o
  JOIN `jrjames83-1171.sampledata.order_payments` op ON o.order_id = op.order_id
GROUP BY 1
ORDER BY 1
```


![sum-of-payment](/pictures/BigQuery/joins-mini-project/sum-of-payment.PNG "sum of payment")


- Then we will juxtapose the hourly payment value and the total payment value for comparison.


```
SELECT EXTRACT(hour FROM o.order_purchase_timestamp) AS order_hour, ROUND(SUM(op.payment_value), 2) AS payment,
      ROUND(SUM(SUM(op.payment_value)) OVER(),2) AS total_of_payment
FROM  `jrjames83-1171.sampledata.orders` o
  JOIN `jrjames83-1171.sampledata.order_payments` op ON o.order_id = op.order_id
GROUP BY 1
ORDER BY 1
```


![total-of-payment](/pictures/BigQuery/joins-mini-project/total-of-payment.PNG "total payment")


- We can do a trend analysis by saving the result and pasting it onto the Google Sheet. Then, click insert chart and it will automatically create a line chart for you.


```
SELECT EXTRACT(hour FROM o.order_purchase_timestamp) AS order_hour, ROUND(SUM(op.payment_value), 2) AS payment,
FROM  `jrjames83-1171.sampledata.orders` o
  JOIN `jrjames83-1171.sampledata.order_payments` op ON o.order_id = op.order_id
GROUP BY 1
ORDER BY 1
```


![insert-chart](/pictures/BigQuery/joins-mini-project/insert-chart.PNG "insert chart")



- But for this case, a bar chart makes more sense. You can edit the chart type by clicking the chart.


![change-to-a-bar-chart](/pictures/BigQuery/joins-mini-project/edit-to-a-bar-chart.PNG "change it to a bar chart")


- What you need to remember from this case is that we used a window function to calculate the total of payment **over the entire table** using SUM(SUM(payment_value)) OVER().


```
SELECT EXTRACT(hour FROM o.order_purchase_timestamp) AS order_hour, ROUND(SUM(op.payment_value), 2) AS payment,
        SUM(SUM(op.payment_value)) OVER() as total_payment
FROM  `jrjames83-1171.sampledata.orders` o
  JOIN `jrjames83-1171.sampledata.order_payments` op ON o.order_id = op.order_id
GROUP BY 1
ORDER BY 1
```

![window-function](/pictures/BigQuery/joins-mini-project/window-function.PNG "window function")



## Join's mini project -2


- In this project, the boss will ask you to show the revenue by time of day, morning, afternoon, evening, and late evening.


```
WITH base_table AS (
SELECT EXTRACT(hour FROM o.order_purchase_timestamp) as order_hour, ROUND(SUM(op.payment_value), 2) as payment
FROM `jrjames83-1171.sampledata.orders` o
  JOIN `jrjames83-1171.sampledata.order_payments` op ON op.order_id = o.order_id
GROUP BY 1
ORDER BY 1)

SELECT
  CASE
    WHEN order_hour >= 22 OR order_hour < 4 THEN 'late_evening'
    WHEN order_hour >= 4 AND order_hour < 11 THEN 'morning'
    WHEN order_hour >= 11 AND order_hour < 17 THEN 'afternoon'
    WHEN order_hour >= 17 AND order_hour < 22 THEN 'evening'
  END AS hour_type,
  ROUND(SUM(payment),2) || 'USD' as payment
FROM base_table
GROUP BY 1
```


- I can even dig deeper to show what hours are inside each daypart.


```
WITH base_table AS (
SELECT EXTRACT(hour FROM o.order_purchase_timestamp) as order_hour, ROUND(SUM(op.payment_value), 2) as payment
FROM `jrjames83-1171.sampledata.orders` o
  JOIN `jrjames83-1171.sampledata.order_payments` op ON op.order_id = o.order_id
GROUP BY 1
ORDER BY 1)

SELECT
  CASE
    WHEN order_hour >= 22 OR order_hour < 4 THEN 'late_evening'
    WHEN order_hour >= 4 AND order_hour < 11 THEN 'morning'
    WHEN order_hour >= 11 AND order_hour < 17 THEN 'afternoon'
    WHEN order_hour >= 17 AND order_hour < 22 THEN 'evening'
  END AS hour_type,
  order_hour,
  ROUND(SUM(payment),2) as payment 
FROM base_table
GROUP BY 1,2
ORDER BY 1,2
```


![daypart](/pictures/BigQuery/joins-mini-project/daypart.PNG "daypart")



## Tricks using case statements


- You can also use **small case statements** inside the SUM function and it creates column for each conditional output.


```
WITH base_table AS (
SELECT EXTRACT(hour FROM o.order_purchase_timestamp) as order_hour, ROUND(SUM(op.payment_value), 2) as payment
FROM `jrjames83-1171.sampledata.orders` o
  JOIN `jrjames83-1171.sampledata.order_payments` op ON op.order_id = o.order_id
GROUP BY 1
ORDER BY 1)

SELECT
  ROUND(SUM(CASE WHEN order_hour BETWEEN 0 AND 6 THEN payment ELSE 0 END),2) as overnight_sales,
  ROUND(SUM(CASE WHEN order_hour BETWEEN 7 AND 11 THEN payment ELSE 0 END),2) as morning_sales,
  ROUND(SUM(CASE WHEN order_hour BETWEEN 12 AND 17 THEN payment ELSE 0 END),2) as afternoon_sales,
  ROUND(SUM(CASE WHEN order_hour BETWEEN 18 AND 23 THEN payment ELSE 0 END),2) as evening_sales
FROM base_table
```


![conditional-output-column](/pictures/BigQuery/joins-mini-project/conditional-output-column.PNG "conditional output column")