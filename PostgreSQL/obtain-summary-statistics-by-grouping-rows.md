## Obtain Summary Statistics by Grouping Rows

- Let's see how to aggregate data using group by, count, and having clause.


```
SELECT size, count(*) as num_of_size
FROM inventory.products
GROUP BY size
HAVING count(*) > 10 
ORDER BY size
```


![having-clause](/pictures/PostgreSQL/summary-statistics/having-clause.PNG "having clause")


- You can group the products and calculate the numter of products, maximum price and maximum size per product group, and average size per product group.


```
SELECT product_name, 
	COUNT(*) as num_of_products, 
	MAX(price) as max_price,
	MAX(size) as max_size,
	AVG(price) as avg_size
FROM inventory.products
GROUP BY product_name;
```


![general-aggregate-statistics](/pictures/PostgreSQL/summary-statistics/general-aggregate-statistics.PNG "general aggregate statistics")


- Use bool_and and bool_or function to see which state has all the customer with their newsletter option ON or at least one of the customers subscribing to the newsletter.


```
SELECT state, 
	count(*) as num_of_customers,
	bool_and(newsletter),
	bool_or(newsletter)
FROM sales.customers
GROUP BY state;
```


![bool-and-or-function](/pictures/PostgreSQL/summary-statistics/bool-and-or-function.PNG "bool and/or function")


- We will use people_heights data to use aggregation functions.


```
SELECT gender, count(*), avg(height_inches) as avg_height, 
		min(height_inches) as min_height,
		max(height_inches) as max_height
FROM public.people_heights
GROUP BY gender
```


![aggregation-by-gender](/pictures/PostgreSQL/summary-statistics/aggregation-by-gender.PNG "aggregation by gender")



- We can calculate the standard deviation of the sample, standard deviation of the population, variance of the sample, and variance of the population.


```
SELECT gender, count(*), avg(height_inches) as avg_height, 
		min(height_inches) as min_height,
		max(height_inches) as max_height,
		stddev_samp(height_inches),
		stddev_pop(height_inches),
		var_samp(height_inches),
		var_pop(height_inches)
FROM public.people_heights
GROUP BY gender
```


![standard-deviation](/pictures/PostgreSQL/summary-statistics/standard-deviation.PNG "standard deviation")


- We will use products table to calculate the count of products and MIN/MAX/AVG value of each product group.


```
SELECT category_id,
		product_name,
		count(*),
		min(price) as min_price,
		max(price) as max_price,
		avg(price) as avg_price
from inventory.products
group by category_id, product_name
ORDER BY category_id, product_name;
```


![category-subgroup](/pictures/PostgreSQL/summary-statistics/category-subgroup.PNG "category subgroup")


- Now, in order to get **subtotals** and **overall totals**, we need to add **rollup** keyword right after the **GROUP BY** clause and wrap the group by category_id and product_name inside the parentheses.


```
SELECT category_id,
		product_name,
		count(*),
		MIN(price) as min_price,
		MAX(price) as max_price,
		AVG(price) as avg_price
FROM inventory.products
GROUP BY rollup (category_id, product_name)
ORDER BY category_id, product_name;
```


![group-by-rollup](/pictures/PostgreSQL/summary-statistics/group-by-rollup.PNG "group by rollup")


- Instead of **rollup** keyword, we can use **cube** keyword to break down the table to all possible subcategories. This result with cube is the same as grouping by just categories, grouping by just size, and the combinations of size and categories, plus a grand total row.


```
SELECT category_id,
	size,
	count(*),
	min(price) as min_price,
	max(price) as max_price,
	avg(price) as avg_price
FROM inventory.products
GROUP BY cube (category_id, size)
ORDER BY category_id, size
```


![group-by-cube](/pictures/PostgreSQL/summary-statistics/group-by-cube.PNG "group by cube")



- We can further segment the information using **filter** to the aggregate calculations. This will give lower segments of data.


```
SELECT category_id,
		count(*) as count_all,
		avg(price) as avg_price,
		count(*) filter (where size <= 16) as count_small,
		avg(price) filter (where size <= 16) as avg_price_small,
		count(*) filter (where size > 16) as count_large,
		avg(price) filter (where size > 16) as avg_price_large
FROM inventory.products
GROUP BY category_id
ORDER BY category_id
```


![filter-where-segmentation](/pictures/PostgreSQL/summary-statistics/filter-where-segmentation.PNG "filter where segmentation")


## Coding Challenge Answer


```
select customer,
		count(order_id) as total_orders,
		count(order_id) filter (where month(order_date) = '3') as march_orders,
		count(order_id) filter (where month(order_date) = '4') as april_orders
FROM orders
GROUP BY customer;
```

- Above is my answer which was correct. But the below answer might be more standard one, using **extract(month from order_date) = '3'**.


```
SELECT customer,
		count(order_id) as total_orders,
		count(order_id) filter (where extract(month from order_date) = '3') as march_orders,
		count(order_id) filter (where extract(month from order_date) = '4') as april_orders
FROM orders
GROUP BY customer;
```

- But the above two answers give you all the counts of fields regardless of the year. Fortunately, the dataset has only one year but when it has multiple years, we need to change our code so that it gives you the correct values.


```
SELECT customer,
		count(order_id) as total_orders,
		count(order_id) filter (where order_date between '2021-03-01' and '2021-03-31') as march_orders,
		count(order_id) filter (where order_date between '2021-04-01' and '2021-04-30') as april_orders
FROM orders
GROUP BY customer;
```


- Also, this is possible.


```
SELECT customer,
		count(order_id) as total_orders,
		count(order_id) filter (where order_date >= '2021-03-01' and order_date <= '2021-03-31') as march_orders,
		count(order_id) filter (where order_date >= '2021-04-01' and order_date <= '2021-04-30') as april_orders
FROM orders
GROUP BY customer;
```


![correct-answer](/pictures/PostgreSQL/summary-statistics/correct-answer.PNG "correct answer")


