### [Source of this study material : Applied SQL For Data Analytics / Data Science With BigQuery by Jeff James](https://www.udemy.com/course/applied-sql-for-data-analytics-data-science-with-bigquery/)


## Playing with BigQuery

- There's a default function called **GENERATE_ARRAY()** where you can generate an array of numbers, etc. And by **UNNESTING** the array, you spread the elements over the rows and you can give it a name as **num** like below.


```
SELECT num FROM UNNEST(GENERATE_ARRAY(1,10)) as num
```


![unnest-and-generate-array](/pictures/BigQuery/join-data/unnest-and-generate-array.PNG "UNNEST and GENERATE_ARRAY")


- Then we will create another table and try to join them together on the common rows from num and num2. If you just join the two tables like below, it will do an **inner join**.


```
WITH table_one AS (
SELECT num FROM UNNEST(GENERATE_ARRAY(1,10)) as num
), 
table_two AS (
  SELECT num2 
  FROM UNNEST(GENERATE_ARRAY(8,12)) AS num2
)

SELECT num FROM table_one JOIN table_two ON num = num2
```


![join-table](/pictures/BigQuery/join-data/join-table.PNG "join table")


```
WITH table_one AS (
SELECT num FROM UNNEST(GENERATE_ARRAY(1,10)) as num
), 
table_two AS (
  SELECT num2 
  FROM UNNEST(GENERATE_ARRAY(8,12)) AS num2
)

SELECT num, num2 
FROM table_one LEFT JOIN table_two ON num = num2
```


![left-join](/pictures/BigQuery/join-data/left-join.PNG "left join")


- If you want to replace that NULL into something else, you can use **COALESCE function** to convert that NULL. 


```
WITH table_one AS (
SELECT num FROM UNNEST(GENERATE_ARRAY(1,10)) as num
), 
table_two AS (
  SELECT num2 
  FROM UNNEST(GENERATE_ARRAY(8,12)) AS num2
)

SELECT num, COALESCE(num2, 999) 
FROM table_one LEFT JOIN table_two ON num = num2
```


![coalesce-function](/pictures/BigQuery/join-data/coalesce-function.PNG "coalesce function")



- If we do the right join, you will see the table like this.


```
WITH table_one AS (
SELECT num1 FROM UNNEST(GENERATE_ARRAY(1,10)) as num1
),

table_two AS (
SELECT num2
FROM UNNEST(GENERATE_ARRAY(8, 12))
 as num2)

SELECT num1, num2
FROM table_one
RIGHT JOIN table_two ON num1 = num2
```


![right-join](/pictures/BigQuery/join-data/right-join.PNG "right join")


- If we want all the values from both tables, we can use **FULL OUTER JOIN**.


```
WITH table_one AS (
SELECT num1 FROM UNNEST(GENERATE_ARRAY(1,10)) as num1
),

table_two AS (
SELECT num2
FROM UNNEST(GENERATE_ARRAY(8, 12))
 as num2)

SELECT num1, num2
FROM table_one
FULL OUTER JOIN table_two ON num1 = num2
```


![full-outer-join](/pictures/BigQuery/join-data/full-outer-join.PNG "full outer join")


- Full outer join can be used in many real-world day jobs. One example is a 'table of all pages with crawls from google bot' and a 'table of all pages with visits from the google search engine'. And because you want to see all the data together, you will use the FULL OUTER JOIN.


- Now, we will play with some real data from orders table. First, we will see how many distinct orders and customer ID are there. We have 99,441 unique orders.


```
SELECT COUNT(DISTINCT order_id), COUNT(DISTINCT customer_id)
 FROM `jrjames83-1171.sampledata.orders` 
```


- Next, we will see the order status. And the number of each order status.


```
SELECT order_status, COUNT(order_status) as num_of_cases
FROM `jrjames83-1171.sampledata.orders`
GROUP BY order_status
ORDER BY num_of_cases DESC
```


![order-status](/pictures/BigQuery/join-data/order-status.PNG "order status")


- Then we will **EXTRACT** the month and year from the timestamp data.


```
SELECT 
  EXTRACT(year FROM order_purchase_timestamp) as year,
  EXTRACT(month FROM order_purchase_timestamp) as month,
  COUNT(*)
FROM `jrjames83-1171.sampledata.orders`
GROUP BY year, month
ORDER BY year, month
```

![extract-month-year](/pictures/BigQuery/join-data/extract-month-year.PNG "extract month and year")


- And this will do a join on customer table and orders table over the customer_id column.


```
SELECT o.customer_id, o.order_id
FROM `jrjames83-1171.sampledata.orders` o
  JOIN `jrjames83-1171.sampledata.customers` c ON o.customer_id = c.customer_id
```


![join-tables](/pictures/BigQuery/join-data/join-tables.PNG "join tables")


- Then we will figure out which city is responsible for the most orders.


```
SELECT c.customer_city, count(order_id) as num_of_orders
FROM `jrjames83-1171.sampledata.orders` o
  JOIN `jrjames83-1171.sampledata.customers` c ON c.customer_id = o.customer_id
GROUP BY c.customer_city
ORDER BY num_of_orders DESC
```


![city-with-most-orders](/pictures/BigQuery/join-data/city-with-most-orders.PNG "city with most orders")


- Now, we will use a **window function** and a built-in **ROW_NUMBER()** function to partition their orders per customer_id to see how many rows of orders that particular customer_id placed.


```
SELECT customer_unique_id, o.order_purchase_timestamp,
  ROW_NUMBER() OVER(PARTITION BY customer_unique_id ORDER BY o.order_purchase_timestamp) as customer_order_number
FROM `jrjames83-1171.sampledata.customers` c
  JOIN `jrjames83-1171.sampledata.orders` o ON o.customer_id = c.customer_id
```


![row-number-over-partition-by](/pictures/BigQuery/join-data/row-number-partition-by.PNG "row number over partition by")



```
WITH base_table AS (
SELECT customer_unique_id, o.order_purchase_timestamp, ROW_NUMBER() OVER(PARTITION BY customer_unique_id ORDER BY o.order_purchase_timestamp) as num_of_orders
FROM `jrjames83-1171.sampledata.customers` c
  JOIN `jrjames83-1171.sampledata.orders` o ON c.customer_id = o.customer_id
GROUP BY customer_unique_id, o.order_purchase_timestamp
),

table_two as (
SELECT * FROM base_table
WHERE num_of_orders = 1
)

SELECT
  EXTRACT(year FROM order_purchase_timestamp) AS year,
  EXTRACT(month FROM order_purchase_timestamp) AS month,
  COUNT(customer_unique_id) as num_of_new_customers
FROM table_two
GROUP BY 1, 2
ORDER BY 3 DESC
```


- Now, this query first gives number to each row over each unique customer ID ordered by order timestamp. You can see which customers with the same ID bought multiple times from the site. Then, we will filter the table out to see only the first-time buying customers. And we will extract year, month from the timestamp and count the number of new customers for each year and month.


![finding-new-customers](/pictures/BigQuery/join-data/finding_new_customers.PNG "finding new customers")



- Finally, I'll calculate the average days between the purhcases per product category name plus the number of times ordered using another table called order_items. The important thing here is that I used **DATE_DIFF(date(), date(), DAY)** function to figure out the days between orders.


```
WITH  base_table AS (
SELECT oi.product_id, p.product_category_name, o.order_purchase_timestamp, 
      ROW_NUMBER() OVER(PARTITION BY oi.product_id ORDER BY o.order_purchase_timestamp) AS num_of_orders,
      LAG(order_purchase_timestamp) OVER(PARTITION BY oi.product_id ORDER BY order_purchase_timestamp) as previous_order_timestamp
FROM `jrjames83-1171.sampledata.order_items` oi
  JOIN `jrjames83-1171.sampledata.orders` o ON o.order_id = oi.order_id
  JOIN `jrjames83-1171.sampledata.products` p ON oi.product_id = p.product_id
ORDER BY 1, 3
),

diffs_table as (
SELECT bt.* , DATE_DIFF(DATE(bt.order_purchase_timestamp), DATE(bt.previous_order_timestamp), DAY) as days_between_orders
FROM base_table bt
)


SELECT product_category_name, SUM(days_between_orders) / COUNT(days_between_orders) as avg_days_between_purchases, COUNT(*) as times_ordered
FROM diffs_table
WHERE days_between_orders is not NULL
GROUP BY product_category_name
```


![average-days-between](/pictures/BigQuery/join-data/average-days-between.PNG "average days between orders")