## Adding and removing columns

- We can drop a column that is not needed from a dataframe using a **drop()** method. You also need to **specify an axis**, 0 being index and 1 being columns.


```
btc.drop(labels="Symbol", axis=1, inplace=True)
```


![drop-a-column](/pictures/python/filtering-dataframes/drop-a-column.PNG "drop a column")


- Now, we will continue with the drop() method and use it to delete rows as well. You can just change your axis to 0, meaning that we want to get rid of rows, not columns.


```
countries.drop(labels="Denmark", axis=0, inplace=True)
```


![axis-zero-means-index](/pictures/python/filtering-dataframes/axis-zero-means-index.PNG "axis zero means index")


- Now, let's learn how to delete rows based off of index. The following will delete the **first 3 rows**.


```
countries.drop(countries.index[0:3])
```


![delete-first-three-rows](/pictures/python/filtering-dataframes/delete-first-3-rows.PNG "delete first 3 rows")


- To create a new column, you need to specify the column name and then assign a value to that column.


```
titanic["species"] = "human"
```


![adding-column](/pictures/python/adding-and-removing-columns/adding-column.PNG "adding column")


- We can use **insert()** method to insert a column at a specific place.


```
houses.insert(0, "county", "King County")
```


![insert-column](/pictures/python/adding-and-removing-columns/insert-column.PNG "insert columns")


- We can copy the column and rename it by assigning a new column name to an existing column name.


```
houses["house_id"] = houses["id"]
```


![copy-a-column](/pictures/python/adding-and-removing-columns/copy-a-column.PNG "copy a column")


- This is another way to create a copy of the existing column and insert it into your desired place by using column index.


![copy-column-using-other-method](/pictures/python/adding-and-removing-columns/copy-column-using-other-method.PNG "copy columns using other method")


- You can add up 2 series from the dataframe and create a new column at the end of the columns.


```
titanic["num_relatives"] = titanic["sibsp"] + titanic["parch"]
```


![new-column-created](/pictures/python/adding-and-removing-columns/new-column-created.PNG "new column created")


- We calculated the number of passengers who traveled alone and their survival rate.


```
titanic[titanic["num_relatives"] == 0]["survived"].value_counts()
```


![survival-rate](/pictures/python/adding-and-removing-columns/survival-rate.PNG "survival rate")



