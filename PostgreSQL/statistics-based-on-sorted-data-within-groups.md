## Statistics based on sorted data within groups

- In statistics, there are multiple ways to evaluate datasets and determine the average values, or more specifically, find the central tendencies. You may know this by the terms, mean/median/mode, etc.


- The **median** value in a dataset can be found by sorting all the numeric values from low to high, and find the value at the mid-point in the list.


- For this, we will use people_heights table.


![people-hegiths](/pictures/PostgreSQL/statistics-on-sorted-data/people-heights.PNG "people hegiths")


- Now, the height values are numerically sorted and we can find the median value.


![numerically-sorted](/pictures/PostgreSQL/statistics-on-sorted-data/numerically-sorted.PNG "numerically sorted")


- Since there are **even** number of data points here (which are 400 rows in total), technically there isn't a value at the exact center of this list. The rows that split the dataset into an equal number of values above and below would be on rows 200 and 201.


- For datasets with even number of measurements, there are 2 ways to handle it: 1. **Discrete median**. It will find the first number in the middle. For our dataset, that would be on row 200, 66.74.


- The other way to find the median is to get the average of the two middle values, called the **Continuous median**.


- First, we will find the **discrete median**. In parentheses, we need 0.5. This is the pecentage that tells the function to look for the value at 50% of the way through the set. And then the syntax is **group by** followed by a parentheses and **order by** with the column we want to sort on.


```
select
percentile_disc(0.5) within group (order by height_inches)
from
public.people_heights;
```


- This way, we can calculate the **continuous median height** value.


```
select 
percentile_disc(0.5) within group (order by height_inches) as discrete_median_height,
percentile_cont(0.5) within group (order by height_inches) as continuous_median_height
from public.people_heights;
```


![percentile-disc-cont-within-group-order-by](/pictures/PostgreSQL/statistics-on-sorted-data/percentile-discrete-cont-within-group.PNG "percentile disc percentile cont within group order by")



- If you select gender and **group by gender**, then it will give you median valumes for each group seprately.


```
select gender,
percentile_disc(0.5) within group (order by height_inches) as discrete_median_height,
percentile_cont(0.5) within group (order by height_inches) as continuous_median_height
from public.people_heights
group by gender
```


![group-by-gender](/pictures/PostgreSQL/statistics-on-sorted-data/group-by-gender.PNG "group by gender")


- If you add **rollup** right after **group by** clause, it will give you back the overall medial values for the entire dataset.


![group-by-rollup-parentheses](/pictures/PostgreSQL/statistics-on-sorted-data/group-by-rollup-parentheses.PNG "group by rollup parentheses")


- This time, we will calculate the 1st, 2nd, and 3rd **quartile** height values using **percentile_cont** function.


```
select gender,
		percentile_cont(0.25) within group (order by height_inches) as first_quartile,
		percentile_cont(0.5) within group (order by height_inches) as second_quartile,
		percentile_cont(0.75) within group (order by height_inches) as third_quartile
from public.people_heights
group by gender
```


![first-second-third-quartile](/pictures/PostgreSQL/statistics-on-sorted-data/first-second-third-quartile.PNG "first second third quartile")


- **ntile** function doesn't give us statitistical quartiles but it creates nth number of even groups.


```
select name,
		height_inches,
		ntile(4) over(order by height_inches)
from public.people_heights
order by height_inches;
```


![ntile-4](/pictures/PostgreSQL/statistics-on-sorted-data/ntile-4.PNG "ntile 4")


- Now, we will study the mode function. 


![mode-result](/pictures/PostgreSQL/statistics-on-sorted-data/mode-result.PNG "mode result")


- But when we sort out the data with the count of height_inches, we find a problem with the mode function.


```
select
		height_inches,
		count(height_inches)
from public.people_heights
group by height_inches
order by count(height_inches) desc;
```


![order-by-count-of-heights](/pictures/PostgreSQL/statistics-on-sorted-data/order-by-count-of-heights.PNG "order by count of heights")


- We have multiple heights with the same occurrences, which is 3. The mode function only returns one value. And it doesn't indicate that other values occur with an equal frequency. That's the biggest problems of mode function in PostgreSQL.


- If you read through the documentation, it clearly indicates that when multiple equally frequent values occur the function will arbitrarily choose one result. And this arbitrary nature of the result can be a big red flag in your book. And this arbitrary nature of the calculation needs to be fully understood before using this function in an analysis. 


- Finally, we can calculate the height range across the entire dataset as well as by gender.


```
select
max(height_inches) - min(height_inches) as height_range
from public.people_heights;
```

```
select gender,
		max(height_inches) - min(height_inches) as height_range
from public.people_heights
group by rollup (gender);
```


![group-by-rollup-gender](/pictures/PostgreSQL/statistics-on-sorted-data/group-by-rollup-gender.PNG "group by rollup gender")


## Coding Challenge Answer

- Obtain statistical information about product pricing.


```
SELECT CATEGORY,
		MIN(PRICE) AS min_price,
		percentile_cont(0.25) within group(order by price) as first_quartile,
		percentile_cont(0.5) within group(order by price) as second_quartile,
		percentile_cont(0.75) within group (order by price) as third_quartile,
		MAX(PRICE) AS max_price,
		MAX(PRICE) - MIN(PRICE) AS price_range
FROM products
GROUP BY CATEGORY
```


![coding-challenge](/pictures/PostgreSQL/statistics-on-sorted-data/coding-challenge.PNG "coding challenge")


