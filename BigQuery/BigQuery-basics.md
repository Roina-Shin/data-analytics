### [Source of this study material : Applied SQL For Data Analytics / Data Science With BigQuery by Jeff James](https://www.udemy.com/course/applied-sql-for-data-analytics-data-science-with-bigquery/)


## BigQuery setup

- To code along with the above course, you first need to see the instructor's project including the sample dataset on the left side. To do that click on **ADD** and then select **Star a project by name**. Type in the name of the project and click enter and it will show up for you in your explorer.


![add-instructors-project](/pictures/BigQuery/BigQuery-basics/add-instructors-project.PNG "add instructor's project")


- Then you can see the project and the dataset as below.


![project-added](/pictures/BigQuery/BigQuery-basics/project-added.PNG "project added")


## Getting started with BigQuery statements

- If you want to join 2 tables (payments and rental tables) on a common column (rental_id) you can do this by running the following:


```
SELECT p.payment_id, p.rental_id, r.inventory_id
FROM `jrjames83-1171.sampledata.payments` p
  JOIN `jrjames83-1171.sampledata.rental` r ON p.rental_id = r.rental_id
WHERE p.amount > 0
```


![first-query](/pictures/BigQuery/BigQuery-basics/first-query.PNG "first query")


- Now, we can ask a couple questions on the question table first and run some queries to anwer them.


- First, our question is **for each tag, how many quesion does it have?**. This question is also about how many unique tag we have. We can know that by looking at the number of rows returned (19852):


```
-- for each tag, how many questions does it have?
-- can a tag belong to multiple questions?

SELECT tag, COUNT(*) -- number of tag appears in
FROM `jrjames83-1171.sampledata.top_questions`
GROUP BY tag
```


![unique-tag](/pictures/BigQuery/BigQuery-basics/unique-tag.PNG "unique tag")


- We can name the count row as "number_of_rows" and order the table by tag:


```
-- for each tag, how many questions does it have?
-- can a tag belong to multiple questions?

SELECT tag, COUNT(*) as number_of_rows -- number of tag appears in
FROM `jrjames83-1171.sampledata.top_questions`
GROUP BY tag
ORDER BY tag
```


![order-by](/pictures/BigQuery/BigQuery-basics/order-by.PNG "order by")


- If you are curious about which tags are most popular, you can order them by 2 (which is by number_of_rows) in descending order.


```
-- for each tag, how many questions does it have?
-- can a tag belong to multiple questions?

SELECT tag, COUNT(*) as number_of_rows -- number of tag appears in
FROM `jrjames83-1171.sampledata.top_questions`
GROUP BY tag
ORDER BY 2 DESC
```

![desc-order](/pictures/BigQuery/BigQuery-basics/desc-order.PNG "desc order")


- Next, we will answer the second question **can a tag belong to multiple questions?**. To do this, we will copy the first query statement and comment out the first one. We will work on the second query statement. To comment it out, you can select the text aand use **ctrl + /**.


![comment-out](/pictures/BigQuery/BigQuery-basics/comment-out.PNG "comment out")


```
SELECT tag, COUNT(DISTINCT id) as n_unique_questions -- number of unique questions
FROM `jrjames83-1171.sampledata.top_questions`
GROUP BY tag
ORDER BY 2 DESC
```

- This will return the number of unique questions that belong to a tag.


![unique-questions](/pictures/BigQuery/BigQuery-basics/unique-questions.PNG "unique questions")


- Now, we will figure out **can a question belong to multiple tags?**. 


```
SELECT title, COUNT(DISTINCT tag) as number_of_tags -- number of unique questions
FROM `jrjames83-1171.sampledata.top_questions`
GROUP BY title
ORDER BY 2 DESC
```


![question-belong-to-multiple-tags](/pictures/BigQuery/BigQuery-basics/question-belong-to-multiple-tags.PNG "question belong to multiple tags")


- The above will work just fine, but if you want to see which specific tags are for each question, you can use this **ARRA_AGG** like below. And there are some questions with multiple tags:


```
SELECT title, id, ARRAY_AGG(DISTINCT tag) as question_tags
FROM `jrjames83-1171.sampledata.top_questions`
GROUP BY title, id
```


![question-with-multiple-tags](/pictures/BigQuery/BigQuery-basics/question-with-multiple-tags.PNG "question with multiple tags")


- Now, we will narrow it down to a table **having** the number of unique tags is greater than 1. You cannot use a WHERE clause to do so because BigQuery doesn't allow you to put aggregates on WHERE clause.


```
SELECT title, id, ARRAY_AGG(DISTINCT tag) as question_tags, ARRAY_LENGTH(ARRAY_AGG(DISTINCT tag)) as num_tags
FROM `jrjames83-1171.sampledata.top_questions`
GROUP BY title, id
HAVING ARRAY_LENGTH(ARRAY_AGG(DISTINCT tag)) > 1
```


![array-agg-and-array-length](/pictures/BigQuery/BigQuery-basics/array-agg.PNG "array_agg and array_length")


- If we want to see how many questions are about 'bigquery', you can use **WHERE tile LIKE '%bigquery%'**.


```
SELECT title, id, ARRAY_AGG(DISTINCT tag) as question_tags, ARRAY_LENGTH(ARRAY_AGG(DISTINCT tag)) as num_tags
FROM `jrjames83-1171.sampledata.top_questions`
WHERE title LIKE '%bigquery%'
GROUP BY title, id
HAVING ARRAY_LENGTH(ARRAY_AGG(DISTINCT tag)) > 1
```


![where-like](/pictures/BigQuery/BigQuery-basics/where-like.PNG "where like")


- If you are worried about the case of the text, you can make the title all lower-case and search it again. TRIM function also makes the title wihout any trailing spaces.


```
SELECT title, id, ARRAY_AGG(DISTINCT tag) as question_tags, ARRAY_LENGTH(ARRAY_AGG(DISTINCT tag)) as num_tags
FROM `jrjames83-1171.sampledata.top_questions`
WHERE TRIM(LOWER(title)) LIKE '%bigquery%'
GROUP BY title, id
HAVING ARRAY_LENGTH(ARRAY_AGG(DISTINCT tag)) > 1
```


## Subquery

- You can create a table from a query and SELECT everything from that table. We call that table query inside the query as a subquery.


```
SELECT * FROM 
    (SELECT title, id, ARRAY_AGG(DISTINCT tag) as question_tags, ARRAY_LENGTH(ARRAY_AGG(DISTINCT tag)) as num_tags
    FROM `jrjames83-1171.sampledata.top_questions`
    WHERE TRIM(LOWER(title)) LIKE '%bigquery%'
    GROUP BY title, id)
```

- And instead of using a HAVING clause, you can use this subquery and WHERE clause outside the subquery to query again on the table. You can indent the code by using "ctrl + ]". For outdenting the code, you can use "ctrl + [".


```
SELECT * FROM 
    (SELECT title, id, ARRAY_AGG(DISTINCT tag) as question_tags, ARRAY_LENGTH(ARRAY_AGG(DISTINCT tag)) as num_tags
    FROM `jrjames83-1171.sampledata.top_questions`
    WHERE TRIM(LOWER(title)) LIKE '%bigquery%'
    GROUP BY title, id)
WHERE ARRAY_LENGTH(question_tags) > 1
```

- And this is the preferred way of creating a subquery by using WITH.


```
WITH title_and_tags AS (
  (SELECT title, id, ARRAY_AGG(DISTINCT tag) AS tags
  FROM `jrjames83-1171.sampledata.top_questions`
  GROUP BY title, id)
)
SELECT * FROM title_and_tags WHERE ARRAY_LENGTH(tags) > 1
```


- Finally, let's find the most popular tags that have the **more than average number of questions**.


```
WITH base_table AS (
  SELECT tag, ARRAY_AGG(DISTINCT title) as questions, ARRAY_LENGTH(ARRAY_AGG(DISTINCT title)) as num_of_questions
  FROM `jrjames83-1171.sampledata.top_questions`
  GROUP BY tag
  ORDER BY num_of_questions DESC
)

SELECT * FROM base_table
WHERE num_of_questions >
  (SELECT AVG(num_of_questions) FROM base_table)
```


![with-base-table-as](/pictures/BigQuery/BigQuery-basics/with-base-table-as.PNG "with base_table as")



