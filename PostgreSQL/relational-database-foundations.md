## The structure of a relational database

- We will create a table inside the fruit database using pgAdmin GUI.


![create-table-gui](/pictures/PostgreSQL/relational-database/create-table-using-gui.png "create table using GUI")


- Give it a name 'fruit' and switch over to the Columns tab. Then create columns by entering the column names and proper data types. For the price column, we used **5 for the Length/Precision** and **2 for the Scale**. This means that we can enter the price up to 999.99 $.


![create-columns](/pictures/PostgreSQL/relational-database/create-columns.PNG "create columns")


- When you click save and go to fruit table > columns, you will see the columns we created.


![columns-created](/pictures/PostgreSQL/relational-database/columns-created.PNG "columns created")


- To insert data, right click on the fruit table and click insert > insert script.


![insert-script](/pictures/PostgreSQL/relational-database/insert-script.png "insert script")


- Insert the values accordingly and execute the query:


![execute-query](/pictures/PostgreSQL/relational-database/execute-query.PNG "execute query")


- Through a quick view, we can see that all the values were successfully inserted to the table.


![quick-view](/pictures/PostgreSQL/relational-database/quick-view.PNG "quick view")


- Until now, we have dealth only with **public schemas** which are open to any user in the organization. This time, we will create a new database called 'kineteco' and create a new schema. Right click on the schema and choose create schema. 


![create-schema](/pictures/PostgreSQL/relational-database/create-schema.png "create schema")


- Give it a name 'manufacturing' and switch over to the Security tab. This is where you assign a user account to **grant access permission**.


![security-tab](/pictures/PostgreSQL/relational-database/security-tab.PNG "security tab")


- After saving it, click Query Tool on the Browser menu. We will create another schema using the Query Tool.


![create-another-schema](/pictures/PostgreSQL/relational-database/create-another-schema.PNG "create another schema")


- If you right click the Schemas and choose refresh, then you will the change in your left pane.


![change-after-refresh](/pictures/PostgreSQL/relational-database/change-refresh.PNG "change after refresh")


- In the manufacturing schema, we will create a table called products.


![create-columns-foreign-key](/pictures/PostgreSQL/relational-database/crate-colums-foreign-key.PNG "create columns foreign key")


- Now, we have products columns created. Let's go ahead and create one more table in pgAdmin interface.


![products-columns-created](/pictures/PostgreSQL/relational-database/products-columns-created.PNG "products columns created")


- We will go back to manufacturing schema and right click it. And choose create table named 'categories'.


![manufacturing-schema-right-click](/pictures/PostgreSQL/relational-database/manufacturing-schema-right-click.png "manufacturing schema right click")


- Then create columns accordingly. category_id will be a primary key for this table. Now, we have two tables. Let's use the Query Tool to query the entire table.


![query-results](/pictures/PostgreSQL/relational-database/query-results.PNG "query results")


- Right click on the products table and click 'properties'. Then go to Contraints tab. There, click Foreign Key. We will make a change here. You will see Definition in the below and toggle the **validated?** option to ON so that any existing data will follow the same rule. With this option off, any changes we make won't apply to data that already existing in the table. 


![constraint-validated-option](/pictures/PostgreSQL/relational-database/constraint-validated-option.PNG "constraint -validated option")


- Then choose tables and columns to establish a relationship on. Click add button and it will create a relationship.


![relationship-add](/pictures/PostgreSQL/relational-database/relationship-add.PNG "relationship add")


- Then change the Action setting. Change the On update from **no action** to **cascade** so that whenever a value in the category_id is updated, the other table's products information will also point to the changed category_id.


![on-update-cascade](/pictures/PostgreSQL/relational-database/on-update-cascade.PNG "on update cascade")


- After creating the relationship, you can go to the table and query the categories table. For data entry, you can use the pgAdmin GUI to add rows.


![data-output-using-gui](/pictures/PostgreSQL/relational-database/data-entry-using-gui.PNG "data entry using gui")


- I have entered several more rows into the table.


![data-entry](/pictures/PostgreSQL/relational-database/data-entry.PNG "data entry")


- Don't forget to save all the data changes you made by clicking the drum icon.  Now, add data to products table also.


![add-data-to-products](/pictures/PostgreSQL/relational-database/add-data-to-products.PNG "add data to products")


- If you save the data, you get an error message. Because you violated the foreign key constraints you set up previously. You entered 10 for cagtegory_id and that category_id is not present in the categories table. So, it rejected to accept the data.


![foreign-key-constraints](/pictures/PostgreSQL/relational-database/foreign-key-constraint.PNG "foreign key constraint")


- If you change the category_id from 10 to 1, then the database accepts the value as it alighns with the foreign key constraints.


![data-saved-successfully](/pictures/PostgreSQL/relational-database/data-saved-successfully.PNG "data saved successfully")


- When we make a change to the **categories** table in the existing rows, that change will **cascade** to the related **products** table and the data will be updated there as well.


![cascade-change](/pictures/PostgreSQL/relational-database/cascade-change.PNG "cascade change")


- We changed the category_id of categories table from 1 to 100. Then save the change. Requery the products table and you will that the change is also reflected here.


![requery-table](/pictures/PostgreSQL/relational-database/requery-table.PNG "requery table")

