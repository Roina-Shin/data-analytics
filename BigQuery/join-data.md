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


