### [Source of this study material : Applied SQL For Data Analytics / Data Science With BigQuery by Jeff James](https://www.udemy.com/course/applied-sql-for-data-analytics-data-science-with-bigquery/)


## Stack Overflow Mini Project

- The first goal is to identify the most common words used in Stack Overflow titles. If you are an SEO professional, you will often find most frequently occuring words in a text and that usually starts with one word analysis instead of phrase analysis. So, let's focus on finding one words now.


- To do that, we will query the top_questions table using regular expressions.


```
SELECT regexp_extract_all(title, r'\w+')
FROM `jrjames83-1171.sampledata.top_questions`
```

- '\w+' means one or more **special alphanumeric sequence**. You need to add in **r** before the regex to indicate that it is a string literal.


![regexp-extract-all](/pictures/BigQuery/stack-overflow-mini-project/regex-extract.PNG "regexp-extract-all")


- Notice how the regex extracted all single words found in a question. Instead of using regex, you can use BigQuery's built-in split function to do the same.


```
SELECT split(title, ' ') as words
FROM `jrjames83-1171.sampledata.top_questions`
```


![split-function](/pictures/BigQuery/stack-overflow-mini-project/split-function.PNG "split function")



- Words is a repeated field (which means 'array field'). If we want to access them individually, we can use the **UNNEST** keyword. Let's get an unnested version of the table.


```
WITH base_table as (
SELECT tag, title, id, split(title, ' ') as words
FROM `jrjames83-1171.sampledata.top_questions`
)

SELECT DISTINCT tag, title, word
FROM base_table, UNNEST(words) word
```

![unnest-words](/pictures/BigQuery/stack-overflow-mini-project/select-distinct-tag.PNG "unnest words")


- We got a word for each unique tag and title. The problem is that we also have some **NLP stop words** extracted that have no informational meaning. (e.g. a, that, is, etc.)


- First, we will count the number of words that appeared in each title per unique tag. We selected **DISTINCT tag** to avoid quadruple or quintuple counting across the redundant tags that each question has.


```
WITH base_table as (
SELECT tag, title, id, split(title, ' ') as words
FROM `jrjames83-1171.sampledata.top_questions`
),

output_table as (
SELECT DISTINCT tag, title, trim(lower(word)) as word
FROM base_table, UNNEST(words) word)

SELECT tag, word, COUNT(*)
FROM output_table
GROUP BY 1,2
```


- Then we will filter out some NLP stop words from the query result.


```
WITH base_table as (
SELECT tag, title, id, split(title, ' ') as words
FROM `jrjames83-1171.sampledata.top_questions`
),

output_table as (
SELECT DISTINCT tag, title, trim(lower(word)) as word
FROM base_table, UNNEST(words) word)

SELECT tag, word, COUNT(*) AS num_of_words
FROM output_table
WHERE word NOT IN ('a', 'are', 'the','that','this','in','to')
GROUP BY 1,2
```


![result-without-stop-words](/pictures/BigQuery/stack-overflow-mini-project/result-without-stop-words.PNG "result without stop words")



```
WITH base_table as (
SELECT tag, title, id, split(title, ' ') as words
FROM `jrjames83-1171.sampledata.top_questions`
),

output_table as (
SELECT DISTINCT tag, title, trim(lower(word)) as word
FROM base_table, UNNEST(words) word)

SELECT tag, word, COUNT(*) AS frequency
FROM output_table
WHERE word NOT IN ('a', 'are', 'the','that','this','in','to', 'what','with','for')
GROUP BY 1,2
HAVING COUNT(*) > 10
ORDER BY 1, 3 DESC
```


![unigram-table](/pictures/BigQuery/stack-overflow-mini-project/unigram-table.PNG "unigram table")


- Now, we will add **ROW_NUMBER()** to rank the tag word and order by that number.


```
WITH base_table as (
SELECT tag, title, id, split(title, ' ') as words
FROM `jrjames83-1171.sampledata.top_questions`
),

output_table as (
SELECT DISTINCT tag, title, trim(lower(word)) as word
FROM base_table, UNNEST(words) word)

SELECT tag, word, COUNT(*) AS frequency,
      ROW_NUMBER() OVER(PARTITION BY tag ORDER BY COUNT(*) DESC) AS tag_word_rank
FROM output_table
WHERE word NOT IN ('a', 'are', 'the','that','this','in','to', 'what','with','for', 'i', 'do')
GROUP BY 1,2
HAVING COUNT(*) > 10
ORDER BY 1, 3 DESC
```


![tag-word-rank](/pictures/BigQuery/stack-overflow-mini-project/tag_word_rank.PNG "tag word rank")


- So far, our task was to extract unigrams (one word) from the text data. But what if we want to extract bigrams (two words) or trigrams (three words) in a string, while incrementing by one each time?


```
-- "the dog ran quickly", ['the', 'dog', 'ran', 'quickly'] --unigrams
-- bigrams: ['the dog', 'dog ran', 'ran quickly']
-- trigrams: ['the dog ran', 'dog ran quickly']
```


- **BQML (BigQuery Machine Learning)** has an ngrams function that let you extract bigrams or trigrams from your text data. You pass in an array of words 


```
SELECT ML.NGRAMS(['the','dog','ran','quickly'], [1,2])
```


- The above query will show you both unigram and bigram as the range specifies that. If you put [1,1] then it will show only unigrams.


![ml-ngrams-only-unigrams](/pictures/BigQuery/stack-overflow-mini-project/ml-ngrams-only-unigrams.PNG "ML NGRAMS only unigrams")


- Now, we can use BigQuery's ML.NGRAMS() to extract from unigrams to trigrams in a speedy way.


```
WITH base_table AS (
SELECT tag, id, title, ML.NGRAMS(split(title, ' '), [1, 3]) as words
FROM `jrjames83-1171.sampledata.top_questions`),

output_table AS (
SELECT distinct tag,title, trim(lower(word)) as keyword
FROM base_table, UNNEST(words) word)

SELECT * FROM (
SELECT tag, title, keyword, COUNT(*) AS frequency, 
      ROW_NUMBER() OVER(PARTITION BY tag ORDER BY COUNT(*) DESC) AS tag_keyword_rank
FROM output_table
GROUP BY 1,2,3)
WHERE tag_keyword_rank < 4 AND
      keyword NOT IN ('the', 'a', 'that', 'i', 'this', 'what', 'to', 'am', 'to', 'into', 'or', 'and', 'in')
ORDER BY 1, 4 DESC
```


- To only see the trigrams from the data, you can use **ARRAY_LENGTH(split(keyword, ' ')) = 3** to set that threshold.


```
WITH base_table AS (
SELECT tag, id, title, ML.NGRAMS(split(title, ' '), [1, 3]) as words
FROM `jrjames83-1171.sampledata.top_questions`),

output_table AS (
SELECT distinct tag,title, trim(lower(word)) as keyword
FROM base_table, UNNEST(words) word)


SELECT * FROM (
SELECT tag, title, keyword, COUNT(*) AS frequency, 
      ROW_NUMBER() OVER(PARTITION BY tag ORDER BY COUNT(*) DESC) AS tag_keyword_rank
FROM output_table
GROUP BY 1,2,3)
WHERE keyword NOT IN ('the', 'a', 'that', 'i', 'this', 'what', 'to', 'am', 'to', 'into', 'or', 'and', 'in') AND
      tag_keyword_rank < 4 AND
      ARRAY_LENGTH(split(keyword, ' ')) = 3
ORDER BY 1, 4 DESC
```


![extracting-trigrams-using-array-length](/pictures/BigQuery/stack-overflow-mini-project/trigrams-using-array-length.PNG "trigrams using array_length function")