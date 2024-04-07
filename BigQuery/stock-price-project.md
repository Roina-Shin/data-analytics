### [Source of this study material : Applied SQL For Data Analytics / Data Science With BigQuery by Jeff James](https://www.udemy.com/course/applied-sql-for-data-analytics-data-science-with-bigquery/)


## Stock Price Project

- We will analyze Google's stock price data. We will select * from the dataset and copy it to the clipboard so that we can scrutinize the dataset with more space. When you are dealing with a time-series dataset, be sure to order it before you start analyzing it.


![time-series-data](/pictures/BigQuery/stock-price-project/time-series-data.PNG "time series data")


- Then we will calculate the 50 day moving average by highlighting the first 50 rows and calculating the average of the prices.


![50-day-moving-average](/pictures/BigQuery/stock-price-project/50-day-moving-average.PNG "50 day moving average")



- Calculate the 200 day moving average in the same way and select all to insert a chart. When we take a look, we can see the red line(50 day MA) oscilates more than the 200 day MA.


![insert-a-chart](/pictures/BigQuery/stock-price-project/insert-a-chart.PNG "insert a chart")


- Now, go back to BigQuery and run the following query. We will give index number to each Date using ROW_NUMBER() function and calculate the AVG over that index number **ranging 5 preceding and current row**.


```
WITH base_table AS (
SELECT sp.*, ROW_NUMBER() OVER(ORDER BY sp.Date) AS index_num
FROM `jrjames83-1171.sampledata.stock_prices` sp
ORDER BY 1
)

SELECT bt.*, AVG(bt.Close) OVER(ORDER BY bt.index_num RANGE BETWEEN 5 PRECEDING AND CURRENT ROW)
FROM base_table bt
```


![window-function](/pictures/BigQuery/stock-price-project/window-function.PNG "window function")


- When we go back to the Google sheet and validate the result, it looks that the Moving Average is calculated correctly. The moving average is predicated on the 5 preceding rows and the current row. So, if it is 5 day moving average, then you need to calculate the **RANGE BETWEEN 4 PRECEDING AND CURRENT ROW**.


- Based on this knowledge, we can calculate the 50 day and 200 day Moving Averages.


```
WITH base_table AS (
SELECT sp.*, ROW_NUMBER() OVER(ORDER BY sp.Date) AS index_num
FROM `jrjames83-1171.sampledata.stock_prices` sp
ORDER BY 1
)

SELECT bt.*, AVG(bt.Close) OVER(ORDER BY bt.index_num RANGE BETWEEN 49 PRECEDING AND CURRENT ROW) AS fifty_day_MA,
  AVG(bt.Close) OVER(ORDER BY bt.index_num RANGE BETWEEN 199 PRECEDING AND CURRENT ROW) AS two_hundred_day_MA,
FROM base_table bt
```


![50-and-200-day-ma](/pictures/BigQuery/stock-price-project/50-and-200-day-ma.PNG "50 and 200 day MA")


- Then we will generate a buy or sell signal column by seeing whether the 50 day is greater than 200 day MA. If 200 day MA is greater than 50 day MA, then we will sell. Otherwise, we will stay.


```
WITH base_table AS (
SELECT sp.*, ROW_NUMBER() OVER(ORDER BY sp.Date) as index_num
FROM `jrjames83-1171.sampledata.stock_prices` sp
ORDER BY 1
),

ma_table AS (
SELECT bt.*, 
AVG(bt.Close) OVER(ORDER BY bt.index_num RANGE BETWEEN 49 PRECEDING AND CURRENT ROW) as fifty_day_MA,
AVG(bt.Close) OVER(ORDER BY bt.index_num RANGE BETWEEN 199 PRECEDING AND CURRENT ROW) as two_hundred_MA
FROM base_table bt
)

SELECT mt.*, 
CASE
  WHEN mt.fifty_day_MA > mt.two_hundred_MA THEN 'buy'
  WHEN mt.fifty_day_MA < mt.two_hundred_MA THEN 'sell'
  ELSE 'stay'
END AS signal
FROM ma_table mt
```


![signal-table](/pictures/BigQuery/stock-price-project/signal-table.PNG "signal table")


- You can use IF statment to do the same.


```
WITH base_table AS (
SELECT sp.*, ROW_NUMBER() OVER(ORDER BY sp.Date) as index_num
FROM `jrjames83-1171.sampledata.stock_prices` sp
ORDER BY 1
),

ma_table AS (
SELECT bt.*, 
AVG(bt.Close) OVER(ORDER BY bt.index_num RANGE BETWEEN 49 PRECEDING AND CURRENT ROW) as fifty_day_MA,
AVG(bt.Close) OVER(ORDER BY bt.index_num RANGE BETWEEN 199 PRECEDING AND CURRENT ROW) as two_hundred_MA
FROM base_table bt
)

SELECT mt.*, IF(mt.fifty_day_MA > two_hundred_MA, 'buy', 'sell') AS signal
FROM ma_table mt
```


![if-statement](/pictures/BigQuery/stock-price-project/if-statement.PNG "if statement")


- Now, we will select only the rows that the signal changed from buy to sell or sell to buy.


```
WITH base_table AS (
SELECT sp.*, ROW_NUMBER() OVER(ORDER BY sp.Date) as index_num
FROM `jrjames83-1171.sampledata.stock_prices` sp
ORDER BY 1
),

ma_table AS (
SELECT bt.*, 
AVG(bt.Close) OVER(ORDER BY bt.index_num RANGE BETWEEN 49 PRECEDING AND CURRENT ROW) as fifty_day_MA,
AVG(bt.Close) OVER(ORDER BY bt.index_num RANGE BETWEEN 199 PRECEDING AND CURRENT ROW) as two_hundred_MA
FROM base_table bt
),

signal_table AS (
SELECT mt.*, IF(mt.fifty_day_MA > two_hundred_MA, 'buy', 'sell') AS signal
FROM ma_table mt
)

SELECT * FROM (
SELECT st.*, LAG(st.signal) OVER(ORDER BY st.Date) AS prev_signal
FROM signal_table st
ORDER BY 1
) WHERE prev_signal != signal 
```


![lag-function-and-order-by](/pictures/BigQuery/stock-price-project/lag-function-and-order-by.PNG "lag function and order by")


- When you use LAG() function, be aware to order the table correctly. Otherwise, BigQuey will try to match the signal column and prev_signal column (where they are the same value) and return a messy table. So, be sure to order by the time or index number.


![query-results](/pictures/BigQuery/stock-price-project/query-results.PNG "query results")




