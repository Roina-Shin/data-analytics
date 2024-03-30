### [Source of this study material : Applied SQL For Data Analytics / Data Science With BigQuery by Jeff James](https://www.udemy.com/course/applied-sql-for-data-analytics-data-science-with-bigquery/)


## CASE statements

- CASE statement is like an if statement. If the condition is TRUE, then we will create a new column called 'language' and call the value inside that column as python.

```
SELECT 
  title,
  CASE
    WHEN title LIKE '%python%' THEN 'python'
  END as language
FROM `jrjames83-1171.sampledata.top_questions`
```

- And this will yield the table where large amount of questions have language as "null". Our goal here is to find the questions including the language 'java', 'python', 'javascript', and 'sql'. Let's do that.


```
SELECT 
  title,
  CASE
    WHEN LOWER(title) LIKE '%python%' THEN 'python'
    WHEN LOWER(title) LIKE '%java%' THEN 'java'
    WHEN LOWER(title) LIKE '%javascript%' THEN 'javascript'
    WHEN LOWER(title) LIKE '%sql%' THEN 'sql'
    ELSE 'other languages'
  END as language
FROM `jrjames83-1171.sampledata.top_questions`
```


![case-statement](/pictures/BigQuery/case-statements/case-statement.PNG "case statement")


- But this will give you result with the repetitive titles. Use the DISTINCT keyword and order the table by title.


```
SELECT 
  DISTINCT title,
  CASE
    WHEN LOWER(title) LIKE '%python%' THEN 'python'
    WHEN LOWER(title) LIKE '%java%' THEN 'java'
    WHEN LOWER(title) LIKE '%javascript%' THEN 'javascript'
    WHEN LOWER(title) LIKE '%sql%' THEN 'sql'
    ELSE 'other languages'
  END as language
FROM `jrjames83-1171.sampledata.top_questions`
ORDER BY title
```


- Again, we need to know which language has which number of questions. To do that, run this:


```
WITH base_table AS (
  SELECT 
    DISTINCT title,
    CASE
      WHEN LOWER(title) LIKE '%python%' THEN 'python'
      WHEN LOWER(title) LIKE '%javascript%' THEN 'javascript'
      WHEN LOWER(title) LIKE '%sql%' THEN 'sql'
      WHEN LOWER(title) LIKE '%java%' THEN 'java'
      ELSE 'other languages'
    END as language
  FROM `jrjames83-1171.sampledata.top_questions`
  )

SELECT language, COUNT(language) AS num_of_questions
FROM base_table
GROUP BY language
ORDER BY num_of_questions DESC
```


![language-rank](/pictures/BigQuery/case-statements/language-rank.PNG "language rank")



- Now, we will see tag content as string with delimiter (white space) per id and question.


```
SELECT id, title, ARRAY_TO_STRING(ARRAY_AGG(DISTINCT tag), " ") AS tag_content
FROM `jrjames83-1171.sampledata.top_questions`
GROUP BY id, title
```


![array-to-string](/pictures/BigQuery/case-statements/array-to-string.PNG "array to string")



- We will then compare the tag and title and see if the language comes up both for title and tag or not by using case statements. Then we will see the number of questions for each language.


```
WITH base_table AS (
  SELECT id, title, ARRAY_TO_STRING(ARRAY_AGG(tag), " ") AS tag_content
  FROM `jrjames83-1171.sampledata.top_questions`
  GROUP BY id, title
), language_table AS (

  SELECT
    id,
    CASE
      WHEN LOWER(title) LIKE '%python%' AND tag_content LIKE '%python%' THEN 'both_in_python'
      WHEN LOWER(title) LIKE '%python%' AND tag_content NOT LIKE '%python%' THEN 'python_title_only'
      WHEN LOWER(title) NOT LIKE '%python%' AND tag_content LIKE '%python%' THEN 'python_tag_only'
      WHEN LOWER(title) LIKE '%sql%' AND tag_content LIKE '%sql%' THEN 'both_in_sql'
      WHEN LOWER(title) LIKE '%sql%' AND tag_content NOT LIKE '%sql%' THEN 'sql_title_only'
      WHEN LOWER(title) NOT LIKE '%sql%' AND tag_content LIKE '%sql%' THEN 'sql_tag_only'
      ELSE NULL
    END AS language
  FROM base_table
  ORDER BY 1
)

SELECT language, COUNT(*) AS num_of_questions
FROM language_table
GROUP BY language
ORDER BY language
```


![num-of-questions](/pictures/BigQuery/case-statements/num-of-questions.PNG "num of questions")


- And instead of NULL, you can display languages that are not python or sql as something else uisng **COALESCE** function. We will refer to NULL as "no_match" here.


```
WITH base_table AS (
  SELECT id, title, ARRAY_TO_STRING(ARRAY_AGG(tag), " ") AS tag_content
  FROM `jrjames83-1171.sampledata.top_questions`
  GROUP BY id, title
), language_table AS (

  SELECT
    id,
    CASE
      WHEN LOWER(title) LIKE '%python%' AND tag_content LIKE '%python%' THEN 'both_in_python'
      WHEN LOWER(title) LIKE '%python%' AND tag_content NOT LIKE '%python%' THEN 'python_title_only'
      WHEN LOWER(title) NOT LIKE '%python%' AND tag_content LIKE '%python%' THEN 'python_tag_only'
      WHEN LOWER(title) LIKE '%sql%' AND tag_content LIKE '%sql%' THEN 'both_in_sql'
      WHEN LOWER(title) LIKE '%sql%' AND tag_content NOT LIKE '%sql%' THEN 'sql_title_only'
      WHEN LOWER(title) NOT LIKE '%sql%' AND tag_content LIKE '%sql%' THEN 'sql_tag_only'
      ELSE NULL
    END AS language
  FROM base_table
  ORDER BY 1
)

SELECT COALESCE(language, "no_match"), COUNT(*) AS num_of_questions
FROM language_table
GROUP BY 1
ORDER BY 2 DESC
```


![coalesce-function](/pictures/BigQuery/case-statements/coalesce-function.PNG "coalesce function")



- Then we will see which language gained popularity over the years and which didn't.


```
WITH base_table AS (
SELECT 
  distinct
  case
    when title like '%python%' then 'python'
    when title like '%javascript%' then 'javascript'
    when title like '%sql%' then 'sql'
    when title like '%java%' then 'java'
    when title like '%++c%' then '++c'
    else 'other languages'
  END AS language, id, quarter, quarter_views
from `jrjames83-1171.sampledata.top_questions`
)

SELECT language, extract(year from quarter) as year, sum(quarter_views) as views
FROM base_table
where language is not null
GROUP BY language, year
ORDER BY language, year
```


![popularity-ver-years](/pictures/BigQuery/case-statements/popularity-over-years.PNG "popularity over years")


- If you click SAVE RESULTS > Copy to Clipboard and paste the data into a new Google sheet, it will be easier for you to analyze the small amount of data.


![google-sheet](/pictures/BigQuery/case-statements/google-sheet.PNG "google sheet")


- But we could have done this using SQL only. That is by using a **window function** like below. We will partition the table by language ordered by year and use **LAG** function to calculate the change of views year on year.


```
WITH base_table AS (
SELECT 
  distinct
  case
    when title like '%python%' then 'python'
    when title like '%javascript%' then 'javascript'
    when title like '%sql%' then 'sql'
    when title like '%java%' then 'java'
    when title like '%++c%' then '++c'
    else 'other languages'
  END AS language, id, quarter, quarter_views
from `jrjames83-1171.sampledata.top_questions`
), summary_table AS (

SELECT language, extract(year from quarter) as year, sum(quarter_views) as views
FROM base_table
where language is not null
GROUP BY language, year
ORDER BY language, year
)

SELECT st.*, LAG(views) OVER (PARTITION BY language ORDER BY year)
FROM summary_table st
```


- We will then change the **LAG(views)** to **( views / LAG(views) OVER (PARTITION BY language ORDER BY year) -1 )** to calculate the percent change year on year.


```
WITH base_table AS (
SELECT 
  distinct
  case
    when title like '%python%' then 'python'
    when title like '%javascript%' then 'javascript'
    when title like '%sql%' then 'sql'
    when title like '%java%' then 'java'
    when title like '%++c%' then '++c'
    else 'other languages'
  END AS language, id, quarter, quarter_views
from `jrjames83-1171.sampledata.top_questions`
), summary_table AS (

SELECT language, extract(year from quarter) as year, sum(quarter_views) as views
FROM base_table
where language is not null
GROUP BY language, year
ORDER BY language, year
)

SELECT st.*, (views / LAG(views) OVER (PARTITION BY language ORDER BY year) -1) * 100 AS pct_change_yoy
FROM summary_table st
```


![year-on-year-change](/pictures/BigQuery/case-statements/yoy-change.PNG "yoy change")


- To refine the result a bit, you can use || to concatenate the percent and the percent sign and round the result to 2 decimal places.


```
WITH base_table AS (
SELECT 
  distinct
  case
    when title like '%python%' then 'python'
    when title like '%javascript%' then 'javascript'
    when title like '%sql%' then 'sql'
    when title like '%java%' then 'java'
    when title like '%++c%' then '++c'
    else 'other languages'
  END AS language, id, quarter, quarter_views
from `jrjames83-1171.sampledata.top_questions`
), summary_table AS (

SELECT language, extract(year from quarter) as year, sum(quarter_views) as views
FROM base_table
where language is not null
GROUP BY language, year
ORDER BY language, year
)

SELECT st.*, round((views / LAG(views) OVER (PARTITION BY language ORDER BY year) -1) * 100, 2) || '%' AS pct_change_yoy
FROM summary_table st
```