## Ranking Data with Windows and Hypothetical sets

- We rank things from top to bottom all the time. In PostgreSQL ranking function can be used in multiple different ways.


```
select name, height_inches,
	rank() over(order by height_inches desc),
	dense_rank() over(order by height_inches desc)
from public.people_heights
order by height_inches desc;
```


![ranking-data-with-window-function](/pictures/PostgreSQL/ranking-data/ranking-data-with-window-function.PNG "ranking data with window function")


- The difference between rank() and dense_rank() functions are the ways they handle the exact same values while ranking. Dense rank **doesn't skip the rank** when there are multiple values at the **same rank**. Rank function does.


![difference-between-rank-and-dense-rank](/pictures/PostgreSQL/ranking-data/difference-between-rank-and-dense-rank.PNG "difference between rank and dense rank")


- We can also partition the data within window function. If you want to rank height by the same biological gender, you can add 'partition by' to the OVER clause.


```
select name,
		height_inches,
		rank() over(partition by gender order by height_inches desc),
		dense_rank() over(partition by gender order by height_inches desc)
from public.people_heights
order
```


![rank-over-partition-by](/pictures/PostgreSQL/ranking-data/rank-over-partition-by.PNG "rank over partition by")


- If you add gender column and order by gender too, then the data looks more clean.


```
select name,
		gender,
		height_inches,
		rank() over(partition by gender order by height_inches desc),
		dense_rank() over(partition by gender order by height_inches desc)
from public.people_heights
order by gender, height_inches desc;
```


![order-by-gender](/pictures/PostgreSQL/ranking-data/order-by-gender.PNG "order by gender")


- Using rank and dense_rank as a window function with an OVER clause will add numeric rankings in line with your other data columns. This would allows you to answer a hypothetical question. You may be wondering, 'what exactly is a hypothetical question?'. With this technique, we could get rankings for values that don't actually exist within the data and find out, hypothetically speaking, where they would rank if they were in the dataset.


- We can give a hypothetical height data to the rank function and ask what would be the rank of that height within the dataset.


```
select gender,
rank(72) within group (order by height_inches desc)
from public.people_heights
group by gender;
```


![hypothetical-height-rank](/pictures/PostgreSQL/ranking-data/hypothetical-height-rank.PNG "hypothetical height rank")


- Finally, we can add **rollup** keyword after the GROUP BY clause so that we can see how someone with **72 inches** height will rank within each gender group and across the entire dataset.


![add-rollup-keyword](/pictures/PostgreSQL/ranking-data/add-rollup-keyword.PNG "add rollup keyword")


- percent_rank() offers the pecentile of the value within the dataset. For example, Marcellus has **0% of rows above him** as he is the tallest person in the list.


```
select name, gender,
		percent_rank() over (order by height_inches desc)
from public.people_heights;
```


![percent-rank](/pictures/PostgreSQL/ranking-data/percent-rank.PNG "percent rank")


- Now, we can add CASE statements to divide the data into different quartile rankings.


```
select name,
		gender,
		height_inches,
		percent_rank() over(order by height_inches desc),
		CASE
			WHEN percent_rank() over(order by height_inches desc) <= 0.25 THEN 'first_quartile'
			WHEN percent_rank() over(order by height_inches desc) <= 0.5 then 'second_quartile'
			WHEN percent_rank() over(order by height_inches desc) <= 0.75 THEN 'third_quartile'
			else 'fourth_quartile'
		END AS quartile_rank
from public.people_heights
order by height_inches desc;
```


![case-statements-using-percent-rank](/pictures/PostgreSQL/ranking-data/case-statements-using-rank-percent.PNG "case statements using percent rank")



## Coding Challenge Answer


- Rank products by price overall, segmented by category, and segmented by size.


```
select PRODUCT_NAME,
		CATEGORY,
		SIZE,
		PRICE,
		DENSE_RANK() OVER(ORDER BY PRICE desc) as overall_rank,
		DENSE_RANK() OVER(PARTITION BY CATEGORY ORDER BY PRICE desc) as category_rank,
		DENSE_RANK() OVER(PARTITION BY SIZE ORDER BY PRICE desc) as size_rank
from products
ORDER BY CATEGORY, PRICE desc;
```


![coding-challenge](/pictures/PostgreSQL/ranking-data/coding-challenge.PNG "coding challenge")