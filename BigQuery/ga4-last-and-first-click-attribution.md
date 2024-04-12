### [Source of this study material : Applied SQL For Data Analytics / Data Science With BigQuery by Jeff James](https://www.udemy.com/course/applied-sql-for-data-analytics-data-science-with-bigquery/)


## Last and First Click Attribution using GA4 and BigQuery

- In this practice, we will figure out the last attribution model using source medium so that we find revenue to the last channel that users interacted with. If we don't have revenue, identify transactions. 


- But first things first, let's review where we left off.


```
SELECT * FROM (
SELECT 
    traffic_source.source,
    traffic_source.medium,
    event_name,
    (SELECT value.string_value FROM UNNEST(event_params) WHERE key = 'page_location') AS landing_page,
    COUNT(DISTINCT user_pseudo_id) AS num_of_users,
    COUNT(*) AS num_of_transactions
FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_20210131`
GROUP BY 1,2,3,4)
WHERE event_name = 'purchase'
Order by 6 DESC
```


- But this code can be expressed in other ways, using conditional logic to speed up the processing of BigQuery. 


```
SELECT * FROM (
SELECT 
    traffic_source.source,
    traffic_source.medium,
    event_name,
    (SELECT value.string_value FROM UNNEST(event_params) WHERE key = 'page_location') AS landing_page,
    COUNTIF(event_name = 'purchase') AS purchases,
    COUNTIF(event_name = 'session_start') AS sessions,
    COUNT(DISTINCT CASE WHEN event_name = 'session_start' THEN user_pseudo_id ELSE NULL END) AS unique_users
FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_20210131`
GROUP BY 1,2,3,4)
WHERE event_name IN ('session_start', 'purchase')
Order by 5 DESC,6 DESC
```


![refactored-code](/pictures/BigQuery/last-and-first-click-attribution/refactored-code.PNG "refactored code")


- While the above information shows you the number of purchases per source medium, how do we go about building the first click attribution model?


```
SELECT
  user_pseudo_id,
  event_timestamp,
  ROW_NUMBER() OVER(PARTITION BY user_pseudo_id ORDER BY event_timestamp) AS row_num,
  (SELECT value.int_value FROM UNNEST(event_params) WHERE key = 'ga_session_id') AS session_id,
  traffic_source.source || ', ' || traffic_source.medium AS source_medium
FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`
WHERE
  event_name = 'session_start'
  AND _TABLE_SUFFIX BETWEEN '20210125' AND '20210131'
ORDER BY 1,2
```


![table-suffix-between](/pictures/BigQuery/last-and-first-click-attribution/table_suffix.PNG "table_suffix-between")


- To crunch only the first user sessions from the above table, you can run:


```
WITH base_table AS (
SELECT
  user_pseudo_id,
  event_timestamp,
  ROW_NUMBER() OVER(PARTITION BY user_pseudo_id ORDER BY event_timestamp) AS row_num,
  (SELECT value.int_value FROM UNNEST(event_params) WHERE key = 'ga_session_id') AS session_id,
  traffic_source.source || ', ' || traffic_source.medium AS source_medium
FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`
WHERE
  event_name = 'session_start'
  AND _TABLE_SUFFIX BETWEEN '20210125' AND '20210131'
ORDER BY 1,2)

SELECT *
FROM base_table
WHERE row_num = 1
```


![first-session-table](/pictures/BigQuery/last-and-first-click-attribution/first-session-table.PNG "first session table")


- Finally, here is my version of tackling our initial question: what's the first click attribution arranged by the number of purchases? In the below table, we can see that the referral was the most effective medium in terms of the number of purchases, followed by organic search in Google.


```
SELECT 
  user_pseudo_id,
  traffic_source.source,
  traffic_source.medium,
  COUNT(*) AS num_of_purchases
FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`
WHERE
  event_name = 'purchase'
  AND _TABLE_SUFFIX BETWEEN '20210125' AND '20230131'
GROUP BY 1,2,3
ORDER BY 4 DESC
```


![table-manipulation](/pictures/BigQuery/last-and-first-click-attribution/table-manipulation.PNG "table manipulation")

