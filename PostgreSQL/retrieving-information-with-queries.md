## Retrieving information with queries

- On the products table we previously created, we will right click it and choose import/export data.


![import-export-data](/pictures/PostgreSQL/retrieving-information/import-export-data.png "import/export data")


- Then choose the csv file you want to import.


![import-csv-file](/pictures/PostgreSQL/retrieving-information/import-csv-file.PNG "import csv file")


- And our encoding is UTF8. That is the text encoding method that we use. Next, take a look at the Options tab. This file has a header row so make sure you select the Header option. Then press the OK button to import the data into our data table.


![options-delimiter](/pictures/PostgreSQL/retrieving-information/options-delimiter.PNG "options delimter header")


- Then re-execute the query and the products table became a much larger data that we can explore.


![became-larger-data](/pictures/PostgreSQL/retrieving-information/became-larger-data.PNG "became larger data")


- Now, we are ready to join our tables.


```
SELECT products.product_id, products.product_name, products.manufacturing_cost, products.category_id, categories.name
FROM manufacturing.products
	JOIN manufacturing.categories ON manufacturing.categories.category_id = manufacturing.products.category_id
```


![join-tables](/pictures/PostgreSQL/retrieving-information/join-tables.PNG "join tables")



- If you want to save your query results in your database, you can run this:


```
CREATE VIEW manufacturing.product_details AS 
SELECT products.product_id, 
	products.product_name, 
	products.manufacturing_cost, 
	products.category_id, 
	categories.name,
	categories.market
FROM manufacturing.products
	JOIN manufacturing.categories ON manufacturing.categories.category_id = manufacturing.products.category_id;
```


- Now, we can find the view in the Views folder. There is the product_details view we just created. The views **act like** a table and you run queries against them.


![create-view](/pictures/PostgreSQL/retrieving-information/create-view.PNG "create view")


- Close out all the query tables and start a new query tool. And run the following:


![view-query-successful](/pictures/PostgreSQL/retrieving-information/view-query-successful.PNG "querying against the view successful")


- Finally, it's a challenge to review what we learned so far. I've created two tables: employees and departments. We will first import CSV data into the tables and build relationships.


![import-data-to-table](/pictures/PostgreSQL/retrieving-information/import-data-to-table.png "import data to table")


- Then we will find the data in our folder named 'departments.csv'.


![import-departments](/pictures/PostgreSQL/retrieving-information/import-departments-csv.PNG "import departments")


- Check if there's any discrepancies in Options and Columns tabs with the tables you created. If all looks ok, then click save.


![check-discrepancies](/pictures/PostgreSQL/retrieving-information/check-discrepancies.PNG "check discrepancies")


- Then do the same thing with the employees table.


![check-for-discrepancies-for-employees](/pictures/PostgreSQL/retrieving-information/check-discrepancies-for-employees.PNG "check for discrepancies for employees")


- Now, we will get all the employees information who works at the 'South' building.


```
SELECT employees.*, departments.department_name, departments.building
FROM human_resources.employees JOIN human_resources.departments ON employees.department_id = departments.department_id
WHERE departments.building = 'South';
```

![employees-in-south-building](/pictures/PostgreSQL/retrieving-information/employees-in-south-building.PNG "employees in south building")


- Then I establish the relationship between the two tables so that the change in the department table is also cascaded to the related employees table.


![referencing-table](/pictures/PostgreSQL/retrieving-information/referencing-table.PNG "referencing table")


