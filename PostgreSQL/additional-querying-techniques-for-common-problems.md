## Additional querying techniques for common problems

- When you execute a query, depending on graphical interface you're working with, you'll typically have a list of numbers down the left side of the result that indicate each row number.


![list-of-numbers](/pictures/PostgreSQL/additional-querying-techniques/list-of-numbers.PNG "list of numbers")


- These numbers aren't part of the actual data. However, PostgreSQL does have a **ROW NUMBER** function that will add this kind of sequence number as a column in the result set. 


- row_number() is a window function so it needs an OVER clause.


```
select sku,
	product_name,
	size,
	row_number() over (order by sku)
from inventory.products;
```


![row-number](/pictures/PostgreSQL/additional-querying-techniques/row-number.PNG "row number")


- This row number column is now an acutal part of data. And we can partition the window frame to create sequences that restart for every group. 


- When we partition by product_name and reexecute the query, it will give us a slightly different result. 


![reexecute-the-query](/pictures/PostgreSQL/additional-querying-techniques/reexecute-the-query.PNG "reexecute the query")


- Now, the row numbers count up within the same name classification. 


- When you execute a query, you will the data types of the output columns. These data types are the same as the ones established in the original table. This is made explicit in the column headers.


![explicit-data-types](/pictures/PostgreSQL/additional-querying-techniques/explicit-data-types.PNG "explicit data types")


- Date values for example uses a date data type. But sometimes we need to convert it into a basic string of characters. Two colons (::) for PostgreSQL is a **type cast operator**. It's used to convert data from one type into another. After the colon, fill in the name of data type that you want to convert to.


```
select order_id,
		order_date::text,
		customer_id
from sales.orders;
```


![type-cast-operator](/pictures/PostgreSQL/additional-querying-techniques/convert-data-type.PNG "type cast operator")


- The **lag function** will shift the value down within a column. This will allow us to compare dates for each customer's next order.


```
select order_id,
		customer_id,
		order_date,
		lag(order_date,1) over(partition by customer_id order by order_date) as prev_order_date
from sales.orders
order by customer_id, order_date;
```


![lag-function-partition-by-customer-id](/pictures/PostgreSQL/additional-querying-techniques/lag-function-partition-by-customer-id.PNG "lag function partition by customer_id")



- The **lead function** works exactly the same but it is used to find the next order_date, thus shifting value up in the column.


```
select order_id,
		customer_id,
		order_date,
		lead(order_date, 1) over(partition by customer_id order by order_date) as next_order_date
from sales.orders
order by customer_id, order_date;
```


![lead-function-partition-by-customer_id](/pictures/PostgreSQL/additional-querying-techniques/lead-function-partition-by-customer-id.PNG "lead function partition by customer_id")


- This way, we can calculate elapsed time between these order dates. We can do so by simply subtracting one value from another.


```
select order_id,
		customer_id,
		order_date,
		lag(order_date, 1) over(partition by customer_id order by order_date) as prev_order_date,
		order_date - lag(order_date,1) over(partition by customer_id order by order_date) as elapsed_time
from sales.orders
order by customer_id, order_date;
```


![calculating-elapsed-time](/pictures/PostgreSQL/additional-querying-techniques/calculating-elapsed-time.PNG "calculating elapsed time")


- **IN function** allows us to compare a column element with the items in a list.


```
select *
from inventory.products
where product_name IN ('Delicate', 'Bold', 'Light');
```


![in-function](/pictures/PostgreSQL/additional-querying-techniques/in-function.PNG "in function")


- And remember, when using **group by** clause, you can further filter your result to have only the products with the count equal to or over 5 by using **HAVING** clause.


```
select product_name, count(*)
from inventory.products
group by product_name
having count(*) >= 5;
```


![group-by-having-clause](/pictures/PostgreSQL/additional-querying-techniques/group-by-having-clause.PNG "group by having clause")


- Now, imagine that you want a table that only include these products with the count of 5 or over 5. You can include the entire query inside the in function and query on top of it.


```
select *
from inventory.products
where product_name in (
	select product_name
	from inventory.products
	group by product_name
	having count(*) >= 5
);
```


![query-inside-in-function](/pictures/PostgreSQL/additional-querying-techniques/query-in-function.PNG "query inside in function")


- **generate_series()** function generates a list of sequential numbers and you can customize the starting and ending number as well as the interval that numbers will be created. 


```
select generate_series(100, 120);
```


![generate-series-function](/pictures/PostgreSQL/additional-querying-techniques/generate-series-function.PNG "generate series function")


- You can actually add in a third parameter to control an interval. 


![third-parameter-to-control-an-interval](/pictures/PostgreSQL/additional-querying-techniques/third-parameter.PNG "third parameter to control an interval")


- You can do this with orders table:


```
select *
from sales.orders
where order_id in (
 select generate_series(0, 500, 10)
)
order by order_id;
```


![generate-series-in-operator](/pictures/PostgreSQL/additional-querying-techniques/generate-series-in-operator.PNG "generate series with in operator")


- You can work with dates by using the same technique.


```
select
	generate_series('2021-03-01'::timestamp, '2021-03-31'::timestamp, '3 days')
```


![generate-series-convert-into-timestamp](/pictures/PostgreSQL/additional-querying-techniques/generate-series-timestamp.PNG "generate_series convert into timestamp")



## Coding Challenge Answer


- Calculate how much taller they are than the person below.


```
select person_id,
	name,
	height_inches,
	lead(name, 1) over(order by height_inches desc) as is_taller_than,
	height_inches - lead(height_inches, 1) over(order by height_inches desc) as by_this_many_inches
from people_heights
order by height_inches desc;
```


![coding-challenge](/pictures/PostgreSQL/additional-querying-techniques/coding-challenge.PNG "coding challenge")


