### [Source of this study material : Applied SQL For Data Analytics / Data Science With BigQuery by Jeff James](https://www.udemy.com/course/applied-sql-for-data-analytics-data-science-with-bigquery/)


## GA4 BigQuery - Analyzing Event Data

- We will use BigQuery's public data to analyze GA4's event data.


```
SELECT 
  event_date,
  event_name,
  COUNT(DISTINCT user_pseudo_id) AS unique_users
FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_20210131` 
GROUP BY 1,2
ORDER BY 1
```


![initial-query](/pictures/BigQuery/ga4-bigquery-analyzing-landing-page/initial-query.PNG "initial query")


- And as the event_params fields are in the form of array, we will **UNNEST** those and extract the data individually.


```
SELECT 
  event_date,
  user_pseudo_id,
  p
FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_20210131` , UNNEST(event_params) p
```


- This is a soure table.


![source-table](/pictures/BigQuery/ga4-bigquery-analyzing-landing-page/original-table.PNG "source table")


- And this is the query results.


![query-results](/pictures/BigQuery/ga4-bigquery-analyzing-landing-page/query-results.PNG "query results")


- We can selectively extract columns from the event_params cluster.


```
SELECT 
  event_date,
  user_pseudo_id,
  p.key,
  p.value.int_value
FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_20210131` , UNNEST(event_params) p
```


![event_params](/pictures/BigQuery/ga4-bigquery-analyzing-landing-page/event_params.PNG "event params")


- We will then use a window function to count the number of sessions (indicated by session ID - int_value) per unique pseudo_user_id.


```
SELECT 
  event_date,
  user_pseudo_id,
  p.key,
  p.value.int_value,
  COUNT(p.value.int_value) OVER(PARTITION BY user_pseudo_id ORDER BY event_date) as num_of_sessions
FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_20210131` , UNNEST(event_params) p
```


![num-of-sessions](/pictures/BigQuery/ga4-bigquery-analyzing-landing-page/num-of-sessions.PNG "num of sessions")


- And the reason why I did use a window function is because if don't count **distinct session ID** which is the int_value by using PARTITION BY clause, you will just be counting the session ID per every field we selected above. And that gives you only 1 as the num_of_sessions as we already have the granularity possible.


- Now, from the given dataset, we will figure out **the most common landing page** based on the number of sessions and users landing on it.


```
SELECT event_name, 
      (SELECT value.string_value FROM UNNEST(event_params) WHERE key = 'page_location') AS landing_page,
      COUNT(*) AS num_of_sessions,
      COUNT(DISTINCT user_pseudo_id) AS num_of_users
FROM bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_20210131
GROUP BY 1,2
```


- Dont' forget to put **DISTINCT** keyword on user_pseudo_id. Otherwise, the query will produce the same number of users and sessions. And each user should create at least one session, if not more.


![distinct-num-of-users](/pictures/BigQuery/ga4-bigquery-analyzing-landing-page/distinct-num-of-users.PNG "distinct num of users")


- This time, we will see the source and medium (by concatenating two fields) instead.


```
SELECT traffic_source.source || ', ' || traffic_source.medium,
      (SELECT value.string_value FROM UNNEST(event_params) WHERE key = 'page_location') AS landing_page,
      COUNT(*) AS num_of_sessions,
      COUNT(DISTINCT user_pseudo_id) AS num_of_users
FROM bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_20210131
GROUP BY 1,2
ORDER BY 4 DESC
```


![concatenate-source-and-medium](/pictures/BigQuery/ga4-bigquery-analyzing-landing-page/concatenate-source-and-medium.PNG "concatenate source and medium")


- Finally, we will see the number of transactions where the event_name is 'purchase'.


```
SELECT * FROM (
SELECT 
  event_name,
  traffic_source.source,
  traffic_source.medium,
  (SELECT value.string_value FROM UNNEST(event_params) WHERE key = 'page_location') AS landing_page,
  COUNT(*) AS num_of_transactions,
  COUNT(DISTINCT user_pseudo_id) AS num_of_users
FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_20210131`
GROUP BY 1,2,3,4
ORDER BY 4 DESC)
WHERE event_name = 'purchase'
```


![subquery](/pictures/BigQuery/ga4-bigquery-analyzing-landing-page/subquery.PNG "subquery")




