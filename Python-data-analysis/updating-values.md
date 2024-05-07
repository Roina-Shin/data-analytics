### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Renaming columns and index labels

- If we want to rename a column, we first make a dictionary and pass it in to rename() method under the columns parameter.


```
mapper = {"Regional indicator": "regional_indicator", "Ladder score": "ladder_score"}
countries.rename(columns=mapper)
```


![mapper-dictionary-for-renaming](/pictures/python/renaming-columns-index/mapper-dictionary-for-renaming.PNG "mapper dictionary for renaming")


- To make the change permanent on the original dataframe, you can use **inplace=True** option with the rename method.


- You can also rename index using the same method as renaming columns.


```
mapper = {"Finland": "finland", "Iceland": "iceland"}
countries.rename(index=mapper)
```


- replace() method allows you to replace values from one to another. First, select the column and specify the value in the column you want to replace, and put the new value to be replaced with.


```
titanic["sex"].replace("female", "F")
```

![replace-method](/pictures/python/renaming-columns-index/replace-method.PNG "replace method")


- To completely mutate the dataframe with the change, make sure to include inplace=True in rename method.


![mutate-table-with-change](/pictures/python/renaming-columns-index/mutate-table-with-change.PNG "mutate table with change")


- If you try to rename multiple values, you need to pass in a list of values you want to change and also a list of values that you want to plug in instead. And the order matters.


```
titanic["sex"].replace(["F", "male"], ["f", "m"], inplace=True)
```

![replace-list-of-values](/pictures/python/renaming-columns-index/replace-list-of-values.PNG "replace list of values")


- The titanic dataframe's age column has lots of NaN values and they are marked as "?" mark. We will replace these with NaN value, but if you just use replace method and put None as is, it will screw things up. So, you can put square brackets [ ] on the question mark and the None value in the replace method, so that Python recognizes those and does the operation correctly.


```
titanic["age"].replace(["?"], [None]).value_counts(dropna=False)
```


![replace-with-square-brackets](/pictures/python/renaming-columns-index/replace-with-square-brackets.PNG "replace with square brackets")



## Updating values using loc[]

- We previously used loc[] to find information corresponding to a specific index or indices. 


```
countries.loc["Brazil":"Cambodia", ["Ladder score", "Healthy life expectancy"]]
```


![using-loc-to-find-info](/pictures/python/renaming-columns-index/using-loc-to-find-info.PNG "using loc to find info")


- Now, imagine that you want to select 3 countries (Denmark, Sweden, Norway) and change their Regional indicator value from "Western Europe" to "Scandinavia".


![visualize-three-countries](/pictures/python/renaming-columns-index/visualize-three-countries.PNG "visualize three countries")


- Then choose the columns you desire to see.


```
countries.loc[["Denmark", "Sweden", "Norway"], ["Regional indicator"]]
```


- If you now assign this to a value, and re-query the table, you will see that the "Western Europe" for the three countries is now replaced with "scandinavia".


![assign-it-to-a-value](/pictures/python/renaming-columns-index/assign-it-to-a-value.PNG "assign it to a value")



- loc[] can be useful to find information that satifies a certain condition like below:


```
many_bedrooms = houses["bedrooms"] >= 10

houses.loc[many_bedrooms]
```


![loc-for-finding-information](/pictures/python/renaming-columns-index/loc-for-finding-rows.PNG "loc for finding information")


![select-columns](/pictures/python/renaming-columns-index/select-columns.PNG "select columns")


- loc[] allows you to specifically access the rows and columns based off of the conditions.


- Finally, we can combine two conditions after defining them using an ampersand (&) operator.


```
good_quality = houses["grade"] > 12
good_views = houses["view"] == 4
houses[good_quality & good_views]
```


![combine-two-conditions](/pictures/python/renaming-columns-index/combine-two-conditions.PNG "combine two conditions")


- To find a specific value in a certain column, you need to do this:


```
movies[movies["title"] == "Evil"]
```


![title-evil](/pictures/python/renaming-columns-index/title-evil.PNG "title evil")


- Then, use rename() method to change the index into other value.


```
movies.rename({"s1907": "s6666"}, inplace=True)

movies.loc["s6666"]
```


