### [Source of this study material : Applied SQL For Data Analytics / Data Science With BigQuery by Jeff James](https://www.udemy.com/course/applied-sql-for-data-analytics-data-science-with-bigquery/)


## More Joins and Partitioning

- We will explore film rental business data where we will utilize 4 tables (film, payments, rental, inventory). We will see what movie ratings (e.g. PG-13) are the top 2 ratings for each customer based on their rental data.


```
WITH base_table AS (
SELECT p.customer_id, p.payment_id, p.amount, p.rental_id, i.film_id, f.title, f.rating
FROM `jrjames83-1171.sampledata.payments` p
  JOIN `jrjames83-1171.sampledata.rental` r on p.rental_id = r.rental_id
  JOIN `jrjames83-1171.sampledata.inventory` i on r.inventory_id = i.inventory_id
  JOIN `jrjames83-1171.sampledata.film` f on f.film_id = i.film_id
)

SELECT bt.customer_id, bt.rating, SUM(bt.amount) AS total_revenue
FROM base_table bt
GROUP BY bt.customer_id, bt.rating
ORDER BY 1, 3 DESC
```


![top-ratings-per-customer](/pictures/BigQuery/more-joins-and-partitioning/top-rating.PNG "top ratings per customer")


- With this table, we will get the rank of a customer's rating along revenue, so that rank = 1 means the most revenue for that rating.


```
WITH base_table AS (
SELECT p.customer_id, p.payment_id, p.amount, p.rental_id, i.film_id, f.title, f.rating
FROM `jrjames83-1171.sampledata.payments` p
  JOIN `jrjames83-1171.sampledata.rental` r on p.rental_id = r.rental_id
  JOIN `jrjames83-1171.sampledata.inventory` i on r.inventory_id = i.inventory_id
  JOIN `jrjames83-1171.sampledata.film` f on f.film_id = i.film_id
),

rating_table AS (
SELECT bt.customer_id, bt.rating, SUM(bt.amount) AS total_revenue
FROM base_table bt
GROUP BY bt.customer_id, bt.rating
ORDER BY 1, 3 DESC
)

SELECT rt.*, ROW_NUMBER() OVER(PARTITION BY rt.customer_id ORDER BY rt.total_revenue DESC) AS top_rating
FROM rating_table rt
ORDER BY rt.customer_id, total_revenue DESC
```


![rating-rank](/pictures/BigQuery/more-joins-and-partitioning/rating-rank.PNG "rating rank")


- Now, we will experiment with the partitioning function in BigQuery. We will first see the month each customer used the rental service and how much they spent per month and movie rating. You can use 2 fields to **partition by** clause within ROW_NUMBER() function so that you can **rank the movie rating inside the month** based on the rental revenue.


```
WITH base_table AS (
SELECT p.payment_id, p.customer_id, p.payment_date, p.amount,f.film_id, f.title, f.rating
FROM `jrjames83-1171.sampledata.payments` p
  JOIN `jrjames83-1171.sampledata.rental` r ON r.rental_id = p.rental_id 
  JOIN `jrjames83-1171.sampledata.inventory` i ON i.inventory_id = r.inventory_id
  JOIN `jrjames83-1171.sampledata.film` f ON f.film_id = i.film_id
),

rating_table AS (
SELECT bt.customer_id, bt.rating, bt.payment_date, SUM(bt.amount) AS total_revenue
FROM base_table bt
GROUP BY bt.customer_id, bt.rating, bt.payment_date
ORDER BY 1,3 DESC),

month_table AS (
SELECT rt.*, EXTRACT(month FROM rt.payment_date) as month
FROM rating_table rt
),

month_total_revenue AS (
SELECT mt.customer_id, mt.month, mt.rating, SUM(mt.total_revenue) as total_revenue
FROM month_table mt
GROUP BY 1,2,3
ORDER BY 1,2,4 DESC
)


SELECT mtr.*, ROW_NUMBER() OVER(PARTITION BY mtr.customer_id, mtr.month ORDER BY mtr.total_revenue DESC) AS rank_in_month
FROM month_total_revenue mtr
ORDER BY 1,2,5
```


![multiple-fields-within-row-number](/pictures/BigQuery/more-joins-and-partitioning/multiple-fields-within-row-number.PNG "multiple fields within row number")



![movie-rating-rank-in-month](/pictures/BigQuery/more-joins-and-partitioning/rank-in-month.PNG "movie rating rank in month")


- We can **control the granularity** when we combine the ranking with customer_id and month fields. In other words, the partitioning happens across these 2 fields to help us see how their preferences shifted over time. It gives you insight into the seasonality you can plan on to better serve your customers. 

