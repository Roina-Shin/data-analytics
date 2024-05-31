### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Indexes in depth

- Both dataframe and series are **indexed**. In other words, each row in a dataframe or series has a label. We call that the index.


- Index can be a number incrementing by 1 or a date or a name. 


- To experiment with indexing, we will read bitcoin csv file.


![read-bitcoin-csv-file](/pictures/python/indexing-and-sorting/read-bitcoin-csv-file.PNG "read bitcoin csv file")


- If we look at the index of the bitcoin file, it shows us:


![look-at-the-index-of-a-dataframe](/pictures/python/indexing-and-sorting/look-at-the-index-of-dataframe.PNG "look at the index of the dataframe")


- We can change the index by passing in the column that we want to make an index. That can be done by using set_index() method.


![set-index-method](/pictures/python/indexing-and-sorting/set-index-method.PNG "set_index method")


- We can save the change into a variable like below:


```
date_indexed_btc = bitcoin.set_index("Date")
```


- But we can also mutate the original dataframe by using **inplace=True** parameter in the set_index(). Usually, you will get the default **inplace=False** parameter in it and the changed index will be reflected in the original dataframe.


```
bitcoin.set_index("Date", inplace=True)

bitcoin
```

- When you look at the index of the dataframe or query the column "High" of the dataframe, you will see that its index is now "Date".


![changed-index](/pictures/python/indexing-and-sorting/changed-index.PNG "changed index")


- We can go ahead and change the index of 'word happiness report' csv file from a number to a country.



![country-index](/pictures/python/indexing-and-sorting/country-index.PNG "country index")


- As we query the "Healthy life expectancy" from the dataframe, we can see that the country index makes more sense than the original number index would have.


![healthy-life-expectancy-with-country-index](/pictures/python/indexing-and-sorting/healthy-life-expectancy-with-country-index.PNG "healthy life expectancy column with country index")


![plot-against-top-ten-countries](/pictures/python/indexing-and-sorting/plot-against-top-ten.PNG "plot against top ten countries in healthy life expectancy")


- We can also set index column by using **index_col = [column name OR column number]** like below:


![using-index-col-parameter](/pictures/python/indexing-and-sorting/using-index-col-parameter.PNG "using index_col parameter")



## Sort values

- sort_values() is a method that is used to sort the values in the dataframe or series. The default is to sort in ascending order. We use **ascending=False** to sort them in descending order. 


![ascending-is-faluse](/pictures/python/indexing-and-sorting/ascenidng-equals-false.PNG "ascending equals false")


- Now, we will experiment with houses dataframe. Imagine we first sort the data by bedrooms. Whenever there's a tie, we will then sort by bathrooms. How can we do that?


- Instead of passing in a single column, we will pass in a list of columns that we want to sort by.


```
houses.sort_values(["bedrooms", "bathrooms"], ascending=False)
```


![pass-in-list-to-sort-values](/pictures/python/indexing-and-sorting/pass-in-list-to-sort-values.PNG "passing in a list to sort values")


- But what if we want to sort non-numeric values? The default behavior is that the upper case letter comes before the lower case letter like below:


![upper-case-comes-before-lower-case](/pictures/python/indexing-and-sorting/upper-case-comes-before-lower-case.PNG "upper case letter comes before lower case letter")


- If we want to sort the name values regardless of the case, we can run the following:


```
titanic.sort_values("name", key=lambda col: col.str.lowercase())
```


![key-lambda-col-lowercase](/pictures/python/indexing-and-sorting/key-lambda-lowercase.PNG "key=lambda col: col.str.lowercase()")


- Also, you can sort the dataframe by its index using **sort_index()** method:


![sort-index-method](/pictures/python/indexing-and-sorting/sort-index-method.PNG "sort_index() method")


- We can also visualize the sorted values in conjuction with value_counts() and plot() methods.


![sort-values-and-plot](/pictures/python/indexing-and-sorting/sort-values-and-plot.PNG "sort values and plot")



## loc

- **.loc[ ]** allows us to access data by the label. We pass in a value or multiple for a given label.


![how-to-use-loc](/pictures/python/indexing-and-sorting/how-to-use-loc.PNG "how to use loc")


- Now it gives a list of the data going vertically by that label. But we can return the data by that label going horizontally in a dataframe format. That is by using additional square brackets inside the square brackets.


```
happiness.loc[["Yemen"]]
```


![using-loc-to-get-a-dataframe](/pictures/python/indexing-and-sorting/using-loc-to-get-a-dataframe.PNG "using loc to get a dataframe")


- If we want to see data by multiple labels, we can just pass in the list including those countries:


```
happiness.loc[["Canada", "United States", "Mexico"]]
```


![add-three-labels](/pictures/python/indexing-and-sorting/add-three-labels.PNG "add three labels")


- We can also pass in integer labels to the loc like below:


```
titanic.loc[[5, 88, 100]]
```

![integer-indices-to-loc](/pictures/python/indexing-and-sorting/integer-indices-to-loc.PNG "integer indices to loc")


- We can use slices to ask for a range of values starting from 10 to 100 by using colon (:).


```
titanic.loc[10:100]
```


![get-a-range-of-index-using-loc](/pictures/python/indexing-and-sorting/get-a-range-of-index-with-loc.PNG "get a range of index using loc")


- Let's experiment with happiness dataframe using loc. We will first use **sort_index()** to alphabetically sort the dataframe and then use to **inplace=True** parameter to make the change permanent to the original table.


![sort-index-inplace-true](/pictures/python/indexing-and-sorting/sort-index-inplace-true.PNG "sort_index and inplace=true")


- And then pass in the country labels you want to get the range of in a new dataframe:


![pass-in-the-country-labels](/pictures/python/indexing-and-sorting/pass-in-the-country-labels.PNG "pass in the country labels")



## iloc

- iloc gives you a series or a dataframe that is by the integer position of that row, and not by the dataframe's default index.


![using-iloc](/pictures/python/indexing-and-sorting/using-iloc.PNG "usng iloc")


- You can also use a slice method to get the rows by providing the range:


![slice-method](/pictures/python/indexing-and-sorting/slice-method.PNG "slice method")


- Not only can we specify the row to retrieve data out of the dataframe, but also we can then say exactly what columns we want back.


```
titanic.loc[1:10, ["pclass", "sex", "name"]]
```


![pass-in-a-list-of-columns-to-loc](/pictures/python/indexing-and-sorting/pass-in-list-of-columns-to-loc.PNG "pass in the list of columns to loc")


- We can also provide an interval at the end of the range.


```
titanic.loc[50:60:2, ["pclass", "sex", "name"]]
```


![pass-in-the-list-of-columns-to-loc](/pictures/python/indexing-and-sorting/pass-in-list-of-columns-to-loc.PNG "pass in the list of columns to loc")


- As both loc[ ] and iloc[ ] are looking at the position and not the value, you can also use them in this way:


```
houses["bedrooms"].value_counts().loc[3]
```

- This will first look at the statistics of the bedroom numbers and then look for the value that corresponds to the '3 bedrooms'.


![3-bedrooms](/pictures/python/indexing-and-sorting/3-bedrooms.PNG "3 bedrooms")


- As the course assignment, I first retrieve the rows of fish-like Pokemon and then create the bar chart based on their Attack values in ascending order.


```
fish_pokemon = ["Magikarp", "Goldeen", "Horsea", "Seaking", "Seadra", "Gyarados"]

pokemon.loc[fish_pokemon].Attack.sort_values(ascending=True).plot(kind="bar")
```


![doing-assignment](/pictures/python/indexing-and-sorting/doing-assignment.PNG "doing assignment")


- Also, we can find the average Speed of the 10 pokemon with the highest Speed.


```
pokemon["Speed"].nlargest(10).mean()
```


![nlargest-mean](/pictures/python/indexing-and-sorting/nlargest-mean.PNG "nlargest mean")


- Here is another way to get the same mean value for top 10 pokemon in their speed.


```
pokemon.sort_values("Speed", ascending=False)["Speed"].head(10).mean()
```


![another-way-to-retrieve-top10-pokemon-in-speed](/pictures/python/indexing-and-sorting/another-way-to-retrieve-top10-pokemon.PNG "another way to retrieve top 10 pokemon's speed average")



- We can also find the top 20 pokemon with highest attack values and then find the most common "Type 1" value.


```
pokemon.sort_values("Attack").tail(20)["Type 1"].mode()

pokemon.sort_values("Attack", ascending=False).head(20)["Type 1"].value_counts().head(1)
```


![find-the-most-common-value-using-mode](/pictures/python/indexing-and-sorting/find-the-most-common-value-using-mode.PNG "find the most common value using mode()")



![find-the-most-common-value-using-value_counts](/pictures/python/indexing-and-sorting/find-most-common-value-using-value-counts.PNG "find the most common value using value_counts()")