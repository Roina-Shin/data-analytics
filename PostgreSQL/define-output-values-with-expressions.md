## Define output values with conditional expressions

- CASE statements allow you to add **if, then, else** logic into your queries. This allow the query to evaluate a codition and return a different value depending on the result of that evaluation. 


- In the below case, we are trying to get category_description instead of category_id. And we can do this by using CASE statements.


```
select sku, product_name,
	case
		when category_id = 1 then 'Olive Oils'
		when category_id = 2 then 'Flavor Infused Oils'
		when category_id = 3 then 'Bath and Beauty'
	END as category_description,
	size,
	price
from inventory.products;
```


![basic-case-statements](/pictures/PostgreSQL/case-statements/basic-case-statements.PNG "basic case statements")


- The **coalesce function** can be used to choose an output from multiple columns, if those columns may contain null values.


- The coalesce function returns the first **non-null** value from the options.


```
select coalesce ('A','B','C')
```


![how-coalesce-function-works](/pictures/PostgreSQL/case-statements/how-coalesce-function-works.PNG "how coalesce function works")


- When we execute the query, it outputs the first value that isn't NULL or empty. In this case, that's the letter A.


- We will change the first value to NULL and reexecute the query.


```
select coalesce (NULL, 'B', 'C')
```


![first-value-is-null](/pictures/PostgreSQL/case-statements/when-null-is-first-value.PNG "when first value is null")


- Then it skips over to the second value and outputs 'B'. 


- Let's see how this can be useful in a query. Take a look at the query below. We inserted new values to the categories table.


```
insert into inventory.categories values (4, null, 'Gift Baskets');
```


![values-inserted](/pictures/PostgreSQL/case-statements/values-inserted.PNG "values inserted")


- Then we will use **coalesce function** and write a query that will substitute the product_line text for the description anywhere that encounters a null value. 


- Create a new column using the coalesce function whie prioritizing the category_description column. But if that one is null, then we will use product_line text. 


![substitute-null-values](/pictures/PostgreSQL/case-statements/substitute-null-values.PNG "substitute null values")


- The coalesce function allows us to effectively patch the hole in the data by substituting another value for the null.


- **nullif** does exactly the opposite. It replaces a specified value into null.


```
select sku,
		product_name,
		category_id,
		nullif(size, 32) as size,
		price
from inventory.products;
```

![nullif](/pictures/PostgreSQL/case-statements/nullif.PNG "nullif")


- This is a bit of a contrived example. But here and there, nullif has some good uses. 


- In scientific data collection, it's not uncommon to have a dataset with something called a **canary value**.


- For instance, an automated thermometer probe might collect temperature data every hour. If the sensor malfunctions, the reported temperature may get stored as something like -999 degrees. This value is so far outside of the expected range that it's instantly recognizable by an analyst as a malfunction and not a true measurement. 


![canary-in-a-coalmine](/pictures/PostgreSQL/case-statements/canary-in-the-coalmine.PNG "canary in a coal mine")


- The types of data anamolies are something called **canary values** because like the proverbial canary in a coal mine, they can provide early warning signs of a potential problem.