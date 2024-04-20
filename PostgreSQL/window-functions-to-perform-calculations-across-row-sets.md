## Use window functions to perform calculations across row sets

- Window function allows you to report the calculated values right alongside each row of data that they correspond with. Window function is often used with **over** clause.


```
SELECT sku,
		product_name,
		size,
		price,
		avg(price) over()
from inventory.products;
```

- For example, the above query shows you the average price of the entire table as a column. So each row of this column has the same value. It can be useful if you want to compare the price of each product with the overall average price.


![over-clause](/pictures/PostgreSQL/window-function/over-clause.PNG "over clause")


- We can create **PARTITION** within this window function in order to control the rows that the aggregate calculation operates on.


```
select sku,
		product_name,
		size,
		category_id,
		price,
		avg(price) over(PARTITION BY size) as avg_price_size
FROM inventory.products
ORDER BY sku, size;
```


![partition-by](/pictures/PostgreSQL/window-function/partition-by.PNG "partition by")


- The window partition function calculates the average price for each of different size groups.


- Finally, we will wrap this query section by calculating the difference between the original product price and the average price of the product size classification.


```
SELECT sku,
		product_name,
		size,
		category_id,
		price,
		avg(price) over(partition by size) as avg_price_size,
		price - (avg(price) over(partition by size)) as price_difference_org_avg_size
FROM inventory.products
ORDER BY sku, size;
```

![price-difference](/pictures/PostgreSQL/window-function/price-difference.PNG "price difference")


- This time, we will query the information about our products and their sales by using **INNER JOIN**.


![INNER-JOIN](/pictures/PostgreSQL/window-function/inner-join.PNG "inner join")


- We can calculate a running total per each order_id by using **ORDER BY** inside the OVER clause.


![calculate-running-total](/pictures/PostgreSQL/window-function/calculate-running-total.PNG "calculate running total")


- Now, we will take a look at how **moving sum** works by using window function and order by clause.


```
select order_id,
		sum(order_id) over(order by order_id rows between 0 preceding and 2 following) as moving_sum_three
FROM sales.order_lines;
```


![rows-between-0-preceding-and-2-following](/pictures/PostgreSQL/window-function/rows-between-0-preceding-and-2-following.PNG "rows between 0 preceding and 2 following")


- You can also add multiple columns for leading/trailing sum and average for each 3 rows.


```
select order_id,
	sum(order_id) over(order by order_id rows between 2 preceding and 0 following) as trailing_sum,
	sum(order_id) over(order by order_id rows between 0 preceding and 2 following) as leading_sum,
	avg(order_id) over(order by order_id rows between 2 preceding and 0 following) as trailing_avg,
	avg(order_id) over(order by order_id rows between 0 preceding and 2 following) as leading_avg
from sales.order_lines;
```


![order-by-rows-between](/pictures/PostgreSQL/window-function/order-by-rows-between.PNG "order by rows between")


- We will experiment with the **order by** clause inside the window function. This time, it will be about text values rather than numerical values. The first_value, last_value, and nth_value will respectively give you the first, last, and nth value over the column you selected. But then, when you run the following query, you get somewhat unexpected results. The output doesn't show you the correct last and nth value as the window function is **ordered by** company. And it works like running total or moving average and only shows the last or nth value for that particular row. 


```
select company,
		first_value(company) over(order by company),
		last_value(company) over(order by company),
		nth_value(company, 3) over(order by company)
from sales.customers
order by company;
```


![unexpected-result](/pictures/PostgreSQL/window-function/unexpected-result.PNG "unexpected result")


- To fix this, you can use **ROWS BETWEEN**. It will not be with specific numbers, but with **UNBOUNDED** keywords.


```
SELECT company,
	first_value(company) over(order by company
	rows between unbounded preceding and unbounded following),
	last_value(company) over(order by company
	rows between unbounded preceding and unbounded following),
	nth_value(company, 3) over(order by company
	rows between unbounded preceding and unbounded following)
FROM sales.customers
ORDER BY company;
```

![rows-between-unbounded-preceding-and-unbounded-following](/pictures/PostgreSQL/window-function/rows-between-unbounded-preceding.PNG "rows between unbounded preceding and unbounded following")


- Unbounded tells the query to take all rows into consideration instead of just the beginning and current rows.


```
select distinct customer_id,
		first_value(order_date) over(partition by customer_id order by order_date
		rows between unbounded preceding and unbounded following) as first_order_date,
		last_value(order_date) over(partition by customer_id order by order_date
		rows between unbounded preceding and unbounded following) as latest_order_date
FROM sales.orders
ORDER BY customer_id
```


![first-and-last-order-date](/pictures/PostgreSQL/window-function/first-and-last-order-date.PNG "first and last order dates")



## Coding Challenge Answer

- Obtain product price comparisons within the same category classification.


```
select PRODUCT_NAME,
	CATEGORY,
	PRICE,
	min(PRICE) over(partition by CATEGORY) AS cat_min_price,
	max(PRICE) over(partition by CATEGORY) AS
	cat_max_price,
	avg(PRICE) over(partition by CATEGORY) AS
	cat_avg_price,
	count(PRICE) over(partition by CATEGORY) AS
	cat_count
from products
```


![min-max-avg-count-over](/pictures/PostgreSQL/window-function/min-max-avg-count-over.PNG "min max avg count over")


- Using the **window** clause makes your code much cleaner.


```
SELECT PRODUCT_NAME,
	CATEGORY,
	PRICE,
	min(PRICE) over (w) as cat_min_price,
	max(PRICE) over(w) as cat_max_price,
	avg(PRICE) over(w) as cat_avg_price,
	count(PRICE) over(w) as cat_count
FROM products
window w as (partition by CATEGORY)
ORDER BY CATEGORY, PRICE;
```


![window-clause](/pictures/PostgreSQL/window-function/window-clause.PNG "window clause")