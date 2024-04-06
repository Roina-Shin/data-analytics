### [Source of this study material : Applied SQL For Data Analytics / Data Science With BigQuery by Jeff James](https://www.udemy.com/course/applied-sql-for-data-analytics-data-science-with-bigquery/)


## Customer Lifestime Value

- First, we will calculate the **first order date** and **total revenue** per unique customer_id, using payments table.


```
SELECT DISTINCT customer_id, min(payment_date) AS first_payment, sum(amount) AS total_revenue
FROM `jrjames83-1171.sampledata.payments`
GROUP BY 1
ORDER BY 1
```


![first-payment-and-total-revenue](/pictures/BigQuery/customer-order-summary/first-payment-and-total-revenue.PNG "first payment and total revenue")


- But what if someone asks you to put their first payment revenue side by side with the dataset? And this is how I solved the problem. I made multiple tables that reference to the source table but also join queried tables together to get the desired output.


```
WITH base_table AS (
SELECT DISTINCT customer_id, min(payment_date) AS first_payment, sum(amount) AS total_revenue
FROM `jrjames83-1171.sampledata.payments`
GROUP BY 1
ORDER BY 1
),

payment_num_table AS (
SELECT customer_id, payment_date, ROW_NUMBER() OVER(PARTITION BY customer_id ORDER BY payment_date) AS payment_num, amount
FROM `jrjames83-1171.sampledata.payments`
ORDER BY 1, 3
),

only_first_payment AS (
SELECT *
FROM payment_num_table
WHERE payment_num = 1
)

SELECT b.*, o.amount AS first_payment_amount
FROM base_table b
  LEFT JOIN only_first_payment o ON b.first_payment = o.payment_date
ORDER BY 1,2
```

- And this is how the instructor solved the problem. He made the query much simpler and also added the percentage of the first time purchase value versus the total revenue per customer_id.


```
WITH base_table AS (
SELECT * FROM
(
SELECT customer_id, payment_date, 
ROW_NUMBER() OVER(PARTITION BY customer_id ORDER BY payment_date) AS order_num, 
amount
FROM `jrjames83-1171.sampledata.payments`
) WHERE order_num = 1
)


SELECT p.customer_id, b.amount AS first_revenue, SUM(p.amount) AS total_revenue, ROUND((b.amount / SUM(p.amount)) * 100,2) || '%' AS first_payment_pct
FROM `jrjames83-1171.sampledata.payments` p
  JOIN base_table b ON p.customer_id = b.customer_id
GROUP BY 1,2
ORDER BY 1
```


![instructors-query](/pictures/BigQuery/customer-order-summary/instructors-query.PNG "instructor's query")


![instructors-way](/pictures/BigQuery/customer-order-summary/instructors-way.PNG "instructor's way")



## Customer Order Summary using Correlated Subquery

- Now, we will look at what each customer spent within their first 30, 60, and 90 days of their first payment.


- If you have a marketing spend which is predicated on life time value (LTV), seeing the first 30, 60, and 90 days values and how they are trending over time can help you establish benchmarks that you can use.


```
WITH base_table AS (
SELECT * FROM (
SELECT customer_id, amount, payment_date, ROW_NUMBER() OVER(PARTITION BY customer_id ORDER BY payment_date) AS order_num
FROM `jrjames83-1171.sampledata.payments`)
WHERE order_num = 1
),

summary_so_far AS (
SELECT p.customer_id, b.amount AS first_order_amount, b.payment_date AS first_payment, SUM(p.amount) as total_revenue
FROM `jrjames83-1171.sampledata.payments` p
  JOIN base_table b ON b.customer_id = p.customer_id
GROUP BY 1,2,3
ORDER BY 1
)

SELECT sf.*, (
  SELECT SUM(p2.amount)
  FROM `jrjames83-1171.sampledata.payments` p2
  WHERE sf.customer_id = p2.customer_id
  AND DATE(p2.payment_date) BETWEEN DATE(sf.first_payment) AND DATE(sf.first_payment) + 30
) AS first_30_days_LTV,
(
  SELECT SUM(p3.amount)
  FROM `jrjames83-1171.sampledata.payments` p3
  WHERE sf.customer_id = p3.customer_id
  AND DATE(p3.payment_date) BETWEEN DATE(sf.first_payment) AND DATE(sf.first_payment) + 60
) AS first_60_days_LTV,
(
  SELECT SUM(p4.amount)
  FROM `jrjames83-1171.sampledata.payments` p4
  WHERE p4.customer_id = sf.customer_id
  AND DATE(p4.payment_date) BETWEEN DATE(sf.first_payment) AND DATE(sf.first_payment) + 90
) AS first_90_days_LTV
FROM summary_so_far sf
```


![correlated-subquery](/pictures/BigQuery/customer-order-summary/correlated-subquery.PNG "correlated subquery")


![first-30-day-LTV](/pictures/BigQuery/customer-order-summary/first-30-day-LTV.PNG "first 30 day LTV")
