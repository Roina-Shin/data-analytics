### [Source of this study material : Applied SQL For Data Analytics / Data Science With BigQuery by Jeff James](https://www.udemy.com/course/applied-sql-for-data-analytics-data-science-with-bigquery/)


## UNNEST & LAG & Correlated Subquery

- UNNEST is BigQuery specific but it's so useful to learn. We will talk about it in conjunction with a correlated subquery.


- When you run this as below, you get a one row and repeated fields (1, 2, 3, 4, 5).


```
SELECT GENERATE_ARRAY(1, 5)
```


![generate-array](/pictures/BigQuery/unnest-and-lag/generate_array.PNG "generate_array")


- To make it multiple rows instead of repeated fields, you can do the following:


```
SELECT val 
FROM UNNEST(GENERATE_ARRAY(1, 5)) AS val
```


- We can use a window function to calculate a global sum which is the sum of every value in the table.


```
SELECT val, SUM(val) OVER() AS global_sum
FROM UNNEST(GENERATE_ARRAY(1, 5)) AS val
```


![global_sum](/pictures/BigQuery/unnest-and-lag/global_sum.PNG "global sum")


- Then, we will calculate the **running total** which is the sum of values at every step of the way (1, 1+2, 1+2+3 ...). The first step to do that is to create an index number for each row. Like in many programming languages, indexing starts at 0.


```
SELECT val, index
FROM UNNEST(GENERATE_ARRAY(1, 5)) val WITH OFFSET AS index
```


![with-offset-as-index](/pictures/BigQuery/unnest-and-lag/with-offset-as-index.PNG "With offset as index")


- Next, we need to get the running total based on that index number. We will use something called a correlated query to do that.


```
WITH base_table as (
SELECT val, index
FROM UNNEST(GENERATE_ARRAY(1, 5)) val WITH OFFSET AS index
)

SELECT bt.*, (
  SELECT SUM(val)
  FROM base_table bt2
  WHERE bt2.index <= bt.index
)
FROM base_table bt
```


![correlated-subquery](/pictures/BigQuery/unnest-and-lag/correlated-subquery.PNG "correlated subquery")



- This time, we will utilize the rudiment of this strategy to calculate the moving average in the dataset.


- First, here's how you get the global average.


```
WITH base_table as (
SELECT num, index
FROM UNNEST(GENERATE_ARRAY(1,10)) num WITH OFFSET AS index
)

SELECT *, AVG(num) OVER() AS global_avg
FROM base_table
```


![global_avg](/pictures/BigQuery/unnest-and-lag/global_avg.PNG "global average")



- We will use **OVER clause in conjunction with AVG() function** to calculate the moving average.


```
WITH base_table as (
SELECT num, index
FROM UNNEST(GENERATE_ARRAY(1,10)) num WITH OFFSET AS index
)

SELECT *, AVG(num) OVER(ORDER BY index ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS three_day_ma
FROM base_table
```


![moving-average](/pictures/BigQuery/unnest-and-lag/moving-average.PNG "moving average")



- To answer the question that the number in the table is always increasing, we can use **LAG() function** again. If my_check is true in each row, then the number is always increasing.


```
WITH base_table as (
SELECT num, index
FROM UNNEST(GENERATE_ARRAY(1,10)) num WITH OFFSET AS index
)

SELECT *, num > LAG(num) OVER(ORDER BY index) AS my_check
FROM base_table
```


![my_check](/pictures/BigQuery/unnest-and-lag/my_check.PNG "my_check")


- And to check if there is only TRUE in there, you can use the following:


```
WITH base_table as (
SELECT num, index
FROM UNNEST(GENERATE_ARRAY(1,10)) num WITH OFFSET AS index
)

SELECT DISTINCT my_check FROM (
SELECT *, num > LAG(num) OVER(ORDER BY index) AS my_check
FROM base_table)
WHERE my_check IS NOT NULL
```


![check-if-all-true](/pictures/BigQuery/unnest-and-lag/check-if-all-true.PNG "check if all true")


