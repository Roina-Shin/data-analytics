### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Working with Types and NA values

- Let's now learn how to change data type of a column. If you look at the Titanic dataframe, the age column is neither integer nor float. But to change the data type of this column from object to float, it will require us to change the "?" mark within the column first.


- To replace the "?" mark into something else, one option is the replacement method.

```
titanic["age"].replace(["?"], [None], inplace=True)
```


![replace-value-in-column](/pictures/python/working-with-types/replace-value-in-column.PNG "replace value in columns")



- Then if we count the number of values in the column, we see that the question mark is now replaced with None value.


![value-counts-dropna-false](/pictures/python/working-with-types/value-counts-dropna-false.PNG "value counts dropna=false")


- To actually cast the series into type float, you can use **astype()** function.


- But this change is not in place now. To make that change in place, we need to assign this astype() command line to the dataframe's column.


```
titanic["age"] = titanic["age"].astype("float")
```


![astype-float](/pictures/python/working-with-types/astype-float.PNG "astype float")



## Data Type - Category

- There's also a data type called Category.


- We can first begin by changing a column's data type from object to Category.


```
titanic["sex"] = titanic["sex"].astype("category")
```


- And category works for columns that you already know that there are only a finite number of values, such as "female" or "male". And it's good to know as it possibly decreases the memory size a bit by changing the data type into Category.



## to_numeric() method

- to_numeric() method is another option to change a series' data type into something else. But what's special about to_numeric() method is that you can provide a dataset that is not **always castable** like the age column with question mark in it. And you can just use its parameter called **coerce** to parse its invalid value to NaN value. 


![coerce-errors](/pictures/python/working-with-types/coerce-errors.PNG "coerce errors")


- This parameter will basically **coerce those error occuring values into NaN values**.


![error-coerce-in-to_numeric](/pictures/python/working-with-types/error-coerce-in-to-numeric.PNG "error=coerce in to_numeric")



## isna() and dropna()

- isna() and dropna() are ways to find NA values or drop NA values.


![isna-and-dropna](/pictures/python/working-with-types/isna-and-dropna.PNG "isna and dropna")


- If we use dropna() on a dataframe, then it will give us only the rows without any NaN value on it. 


![dropna-on-dataframe](/pictures/python/working-with-types/dropna-on-dataframe.PNG "dropna on dataframe")


- But if we want to drop only the row that its values are all Null, we can use a parameter called **how="all"**.


![parameter-how-all](/pictures/python/working-with-types/parameter-how-all.PNG "parameter how all")



## fillna()

- fillna() method enables us to replace NA values with other values.


```
stats.fillna(0)
```


- Say we want to replace the NA values in a specific column. In that case, we will select dataframe and column first and use fillna() method to replace NA values with other values.


![fillna-method](/pictures/python/working-with-types/fillna-method.PNG "fillna method")



## Excercise

- First, read the Netflix data while setting the "|" as the separator and the first column as the index column.


![exercise-image](/pictures/python/working-with-types/exercise.PNG "exercise")



- Then find the rows with no country in it.


![isna-country](/pictures/python/working-with-types/isna-country.PNG "isna country")


- Then find the rows with no director, no cast, and no country listed.


```
netflix[netflix.country.isna() & netflix.director.isna() & netflix.cast.isna()]
```


![isna-and-operator](/pictures/python/working-with-types/column-isna-method.PNG "column isna method")


- Drop all columns in the dataframe that contain at least one NA value, not inplace!


```
# axis = 'index' or axis = 0

# axis = 'columns' or axis = 1
```


![dropna-axis-1](/pictures/python/working-with-types/dropna-axis-1.PNG "dropna axis=1")



- Then drop all rows that have a missing director or cast (not in place).


```
netflix.dropna(subset = ["director", "cast"])
```


[dropna-subset](/pictures/python/working-with-types/dropna-subset.PNG "dropna subset")



- Find the rows that are missing a "rating" and replace that NA ratings with "TV-MA" using **.loc[]**.


- First, use loc to find only the index and rating that has NaN values.


```
netflix.loc[netflix["rating"].isna(), "rating"]
```


![replace-rating-with-tvma](/pictures/python/working-with-types/replace-rating-with-tvma.PNG "replace rating with tvma")



- Fill the missing country values with the most common (mode) country values, in place!


- First, find the mode value of the country.


```
netflix["country"].mode()
```


![mode-of-country](/pictures/python/working-with-types/mode-of-country.PNG "mode of country")


- And as the mode() returns a series, not a single value, because there could be multiple mode values, you need to specfiy **[0]** to get only the first mode in the dataset.


```
mode_country = netflix["country"].mode()[0]
netflix.fillna({"country": mode_country}, inplace=True)
netflix.country.isna().value_counts()
```


![mode-country-and-validation](/pictures/python/working-with-types/mode-country-and-validation.PNG "mode country and validation")