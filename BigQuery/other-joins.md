### [Source of this study material : Applied SQL For Data Analytics / Data Science With BigQuery by Jeff James](https://www.udemy.com/course/applied-sql-for-data-analytics-data-science-with-bigquery/)


## Other joins you might need

- Let's construct a query to see only the customers who bought multiple times (more than once) using ROW_NUMBER() function and NOT IN operator.


```
WITH base_table as (
SELECT customer_unique_id, o.order_purchase_timestamp, ROW_NUMBER() OVER(PARTITION BY customer_unique_id ORDER BY o.order_purchase_timestamp) as purchase_times
FROM `jrjames83-1171.sampledata.customers` c
  JOIN `jrjames83-1171.sampledata.orders` o ON c.customer_id = o.customer_id
GROUP BY 1, 2
ORDER BY 1, 3
),

exclude_these as (
SELECT customer_unique_id
FROM base_table
GROUP BY 1
HAVING MAX(purchase_times) = 1
)

SELECT *
FROM base_table
WHERE customer_unique_id NOT IN (SELECT customer_unique_id FROM exclude_these)
ORDER BY 1,3
```


![window-function](/pictures/BigQuery/other_joins/window-function.PNG "window function")



- Then we will use the LAG() function to juxtapose a lagged timestamp (previous_purchase_timestamp) beside the purchase timestamp column. 


```
WITH base_table as (
SELECT customer_unique_id, o.order_purchase_timestamp, ROW_NUMBER() OVER(PARTITION BY customer_unique_id ORDER BY o.order_purchase_timestamp) as purchase_times
FROM `jrjames83-1171.sampledata.customers` c
  JOIN `jrjames83-1171.sampledata.orders` o ON c.customer_id = o.customer_id
GROUP BY 1, 2
ORDER BY 1, 3
),

exclude_these as (
SELECT customer_unique_id
FROM base_table
GROUP BY 1
HAVING MAX(purchase_times) = 1
)

SELECT bt.*, LAG(order_purchase_timestamp) OVER(PARTITION BY customer_unique_id ORDER BY order_purchase_timestamp) as previous_purchase_timestamp
FROM base_table bt
WHERE customer_unique_id NOT IN (SELECT customer_unique_id FROM exclude_these)
ORDER BY 1,3
```


![lagged-timestamp](/pictures/BigQuery/other_joins/lagged-timestamp.PNG "lagged timestamp")


- Then we will calculate how many days between the purchases the multiple times buying customers have.


```
WITH base_table as (
SELECT customer_unique_id, o.order_purchase_timestamp, ROW_NUMBER() OVER(PARTITION BY customer_unique_id ORDER BY o.order_purchase_timestamp) as purchase_times
FROM `jrjames83-1171.sampledata.customers` c
  JOIN `jrjames83-1171.sampledata.orders` o ON c.customer_id = o.customer_id
GROUP BY 1, 2
ORDER BY 1, 3
),

exclude_these as (
SELECT customer_unique_id
FROM base_table
GROUP BY 1
HAVING MAX(purchase_times) = 1
),

previous_date_table as (
SELECT bt.*, LAG(order_purchase_timestamp) OVER(PARTITION BY customer_unique_id ORDER BY order_purchase_timestamp) as previous_purchase_timestamp
FROM base_table bt
WHERE customer_unique_id NOT IN (SELECT customer_unique_id FROM exclude_these)
ORDER BY 1,3
)

SELECT *, DATE_DIFF(order_purchase_timestamp, previous_purchase_timestamp, day) AS  days_between_purchases
FROM previous_date_table
WHERE previous_purchase_timestamp is not NULL
```


![days-between-purchases](/pictures/BigQuery/other_joins/days_between_purchases.PNG "days between purchases")



- Then we will see the number of customers per purchase times. 


```
WITH base_table as (
SELECT customer_unique_id, o.order_purchase_timestamp, ROW_NUMBER() OVER(PARTITION BY customer_unique_id ORDER BY o.order_purchase_timestamp) as purchase_times
FROM `jrjames83-1171.sampledata.customers` c
  JOIN `jrjames83-1171.sampledata.orders` o ON c.customer_id = o.customer_id
GROUP BY 1, 2
ORDER BY 1, 3
),

exclude_these as (
SELECT customer_unique_id
FROM base_table
GROUP BY 1
HAVING MAX(purchase_times) = 1
),

previous_date_table as (
SELECT bt.*, LAG(order_purchase_timestamp) OVER(PARTITION BY customer_unique_id ORDER BY order_purchase_timestamp) as previous_purchase_timestamp
FROM base_table bt
WHERE customer_unique_id NOT IN (SELECT customer_unique_id FROM exclude_these)
ORDER BY 1,3
)

SELECT purchase_times, COUNT(DISTINCT customer_unique_id) AS num_of_customers
FROM previous_date_table
GROUP BY 1
ORDER BY 1
```


![num-of-customers](/pictures/BigQuery/other_joins/num-of-customers.PNG "num of customers")





