### [Source of this study material : Applied SQL For Data Analytics / Data Science With BigQuery by Jeff James](https://www.udemy.com/course/applied-sql-for-data-analytics-data-science-with-bigquery/)


## Full outer join use case

- Full outer join combines two tables and pull all elements from both tables regardless. When you do the full outer join, notice what happens to the output.


```
WITH took_offer as (
SELECT 1 as customer_id, 24.89 as spend UNION ALL
SELECT 2, 21.99 UNION ALL
SELECT 3, 179.0 UNION ALL
SELECT 4, .99 UNION ALL
SELECT 5, 1299.00
),

loyalty_club as (
SELECT 3 as customer_id, 1 as status UNION ALL
SELECT 4, 1 UNION ALL
SELECT 8, 2 UNION ALL
SELECT 12, 1 UNION ALL
SELECT 10, 2
)

SELECT *
FROM took_offer t
  FULL OUTER JOIN loyalty_club lc on t.customer_id = lc.customer_id 
```


![full-outer-join-result](/pictures/BigQuery/full-outer-join/ "full outer join result")



- Full outer join lets you see the full picture by showing you not only the overlapping data but also the outer parts.


- Now, we will add conditional statement to the SQL code to categorize the customers depending on their presence in loyalty as well as their offer acceptance.


```
WITH took_offer as (
SELECT 1 as customer_id, 24.89 as spend UNION ALL
SELECT 2, 21.99 UNION ALL
SELECT 3, 179.0 UNION ALL
SELECT 4, .99 UNION ALL
SELECT 5, 1299.00
),

loyalty_club as (
SELECT 3 as customer_id, 1 as status UNION ALL
SELECT 4, 1 UNION ALL
SELECT 8, 2 UNION ALL
SELECT 12, 1 UNION ALL
SELECT 10, 2
),

base_table as (
SELECT t.customer_id, t.spend, lc.customer_id as loyalty_customer_id, lc.status
FROM took_offer t
  FULL OUTER JOIN loyalty_club lc on t.customer_id = lc.customer_id 
)

SELECT *,
  CASE
    WHEN customer_id IS NOT NULL AND loyalty_customer_id IS NULL THEN 'took_offer_not_loyalty'
    WHEN customer_id IS NOT NULL AND loyalty_customer_id IS NOT NULL THEN 'took_offer_in_loyalty'
    WHEN customer_id IS NULL AND loyalty_customer_id IS NOT NULL THEN 'no_offer_in_loyalty'
  END AS row_type

FROM base_table
```


![case-statement-result](/pictures/BigQuery/full-outer-join/case-statement-result.PNG "case statement result")


- In real business cases, you might want only the customers who took offer but not in loyalty to send them emails. In that case, you can write the query like below:


```
WITH took_offer as (
SELECT 1 as customer_id, 24.89 as spend UNION ALL
SELECT 2, 21.99 UNION ALL
SELECT 3, 179.0 UNION ALL
SELECT 4, .99 UNION ALL
SELECT 5, 1299.00
),

loyalty_club as (
SELECT 3 as customer_id, 1 as status UNION ALL
SELECT 4, 1 UNION ALL
SELECT 8, 2 UNION ALL
SELECT 12, 1 UNION ALL
SELECT 10, 2
),

base_table as (
SELECT t.customer_id, t.spend, lc.customer_id as loyalty_customer_id, lc.status
FROM took_offer t
  FULL OUTER JOIN loyalty_club lc on t.customer_id = lc.customer_id 
)

SELECT * FROM (
SELECT *,
  CASE
    WHEN customer_id IS NOT NULL AND loyalty_customer_id IS NULL THEN 'took_offer_not_loyalty'
    WHEN customer_id IS NOT NULL AND loyalty_customer_id IS NOT NULL THEN 'took_offer_in_loyalty'
    WHEN customer_id IS NULL AND loyalty_customer_id IS NOT NULL THEN 'no_offer_in_loyalty'
  END AS row_type

FROM base_table
) WHERE row_type = 'took_offer_not_loyalty'
```


![customers_took_offer_not_loyalty](/pictures/BigQuery/full-outer-join/customers_took_offer_not_loyalty.PNG "customers took offer not loyalty")

