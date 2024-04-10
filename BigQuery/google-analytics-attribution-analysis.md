### [Source of this study material : Applied SQL For Data Analytics / Data Science With BigQuery by Jeff James](https://www.udemy.com/course/applied-sql-for-data-analytics-data-science-with-bigquery/)


## Google Analytics Attribution Analysis

- Search 'google analytics demo account' and access the demo account. It's one of top tools to measure user behaviors on your website and do ROI (Return on Investment) analysis on your marketing spend.


- Now, you see this dataset of attribution modeling. And we will first need to **split** this up to get useful data structure and find the **first and last** step in it.


![attribution-modeling](/pictures/BigQuery/google-analytics-attribution-analysis/attribution-modeling.PNG "attribution modeling")


- To only extract individual steps in each path, we use the following:


```
WITH base_table AS (
SELECT split(path, '>')  AS step_in_path
FROM `jrjames83-1171.sampledata.conversion_paths`)

SELECT step_in_path
FROM base_table, UNNEST(step_in_path)
```


![using-unnest-and-split](/pictures/BigQuery/google-analytics-attribution-analysis/using-unnest-and-split.PNG "usign unnest and split")


- And then we will clean the data up a bit with lower() and trim() functions.


```
WITH base_table AS (
SELECT split(path, '>')  AS step_in_path
FROM `jrjames83-1171.sampledata.conversion_paths`)

SELECT trim(lower(steps))
FROM base_table, UNNEST(step_in_path) as steps
```


- We will use **WITH OFFSET AS** clause to get the index of values in repeated fields.


```
WITH base_table AS (
SELECT split(path, '>')  AS step_in_path
FROM `jrjames83-1171.sampledata.conversion_paths`)

SELECT trim(lower(steps)) as row, index_num
FROM base_table, UNNEST(step_in_path) as steps WITH OFFSET AS index_num
```


![with-offset-as-clause-to-get-index](/pictures/BigQuery/google-analytics-attribution-analysis/with-offset-as-to-get-index.PNG "WITH OFFSET AS clause to get index for each repeated field")


- You can also use **ARRAY_LENGTH()** function to have some knowledge about how many indices you have in each path like below:


```
WITH base_table AS (
SELECT split(path, '>')  AS step_in_path
FROM `jrjames83-1171.sampledata.conversion_paths`)

SELECT trim(lower(steps)) as row, index_num, ARRAY_LENGTH(step_in_path) as num_in_path
FROM base_table, UNNEST(step_in_path) as steps WITH OFFSET AS index_num
```


![array-length-function](/pictures/BigQuery/google-analytics-attribution-analysis/array-length-function.PNG "array length function")


- So, going back to our initial question: which are the first and last touch? It's my version of answering the question using CASE statements.


```
WITH base_table AS (
SELECT split(path, '>')  AS step_in_path
FROM `jrjames83-1171.sampledata.conversion_paths`),

individual_path_table AS (
SELECT trim(lower(steps)) as row, index_num, ARRAY_LENGTH(step_in_path) as num_in_path
FROM base_table, UNNEST(step_in_path) as steps WITH OFFSET AS index_num)

SELECT ip.*,
CASE
  WHEN num_in_path = 1 THEN 'first touch'
  WHEN num_in_path > 1 AND index_num = 0 THEN 'first touch'
  ELSE NULL
END AS first_touchpoint,
CASE
  WHEN num_in_path = 1 THEN 'last touch'
  WHEN num_in_path > 1 AND index_num = num_in_path - 1 THEN 'last touch'
END AS last_touchpoint
FROM individual_path_table ip
```


![case-statements](/pictures/BigQuery/google-analytics-attribution-analysis/case-statements.PNG "case statements")


![query-results](/pictures/BigQuery/google-analytics-attribution-analysis/query-results.PNG "query results")


-  But what if your customers only want the path having more than one step and see the touch types in a single columm? Let's construct the table right now.


```
WITH base_table AS (
SELECT steps, index_num, ARRAY_LENGTH(split_path) as num_in_path FROM (
SELECT split(path, ' > ') as split_path
FROM `jrjames83-1171.sampledata.conversion_paths`), UNNEST(split_path) steps WITH OFFSET AS index_num),

multiple_step_path_table as (
SELECT *
FROM base_table
WHERE num_in_path > 1)

SELECT mt.*,
CASE
  WHEN mt.index_num = 0 THEN 'first_touch'
  WHEN mt.index_num = mt.num_in_path - 1 THEN 'last_touch'
  ELSE 'middle_touch'
END AS touch_type
FROM multiple_step_path_table mt
```


![some-other-cases-query](/pictures/BigQuery/google-analytics-attribution-analysis/some-other-cases.PNG "some other cases")


![query-results-for-some-other-cases](/pictures/BigQuery/google-analytics-attribution-analysis/query-reuslt-for-other-cases.PNG "query results for other cases")



- But we forgot something crucial in our table. The **conversions**. Let's think about how to put that into our table.


```
WITH base_table AS (
SELECT steps, index_num, ARRAY_LENGTH(split_path) as num_in_path, conversions FROM (
SELECT split(path, ' > ') as split_path, conversions
FROM `jrjames83-1171.sampledata.conversion_paths`), UNNEST(split_path) steps WITH OFFSET AS index_num),

multiple_step_path_table as (
SELECT *
FROM base_table
WHERE num_in_path > 1)

SELECT mt.*,
CASE
  WHEN mt.index_num = 0 THEN 'first_touch'
  WHEN mt.index_num = mt.num_in_path - 1 THEN 'last_touch'
  ELSE 'middle_touch'
END AS touch_type
FROM multiple_step_path_table mt
```


![conversions-repeat](/pictures/BigQuery/google-analytics-attribution-analysis/conversions-repeat.PNG "conversions repeat")


- If we look at the query results, the conversions repeat. But it's not a big issue for this case as the touch_type is either first_touch or middle_touch or final_touch and they all can have the same conversion value as they contributed to our revenue in some way.


- To get a more precise outcome, we will extract data that only includes first_touch and last_touch. And we will show the sum of conversions grouped by steps and touch_type. We will finally arrange the table with steps and the number of conversions in a descending order.


```
WITH base_table AS (
SELECT steps, index_num, ARRAY_LENGTH(split_path) as num_in_path, conversions FROM (
SELECT split(path, ' > ') as split_path, conversions
FROM `jrjames83-1171.sampledata.conversion_paths`), UNNEST(split_path) steps WITH OFFSET AS index_num),

multiple_step_path_table as (
SELECT *
FROM base_table
WHERE num_in_path > 1),

summary_table AS (
SELECT mt.*,
CASE
  WHEN mt.index_num = 0 THEN 'first_touch'
  WHEN mt.index_num = mt.num_in_path - 1 THEN 'last_touch'
  ELSE 'middle_touch'
END AS touch_type
FROM multiple_step_path_table mt)

SELECT steps, touch_type, SUM(conversions) AS num_of_conversions
FROM (
SELECT *
FROM summary_table
WHERE touch_type = 'first_touch' OR touch_type = 'last_touch')
GROUP BY steps, touch_type
ORDER BY 1, 3 DESC
```


![entirety-of-table](/pictures/BigQuery/google-analytics-attribution-analysis/entirety_of_table.PNG "entirety of query")


![final-table](/pictures/BigQuery/google-analytics-attribution-analysis/final-table.PNG "final table")
