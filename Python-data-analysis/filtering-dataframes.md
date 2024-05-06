## Filtering Dataframes

- When you want to see from a dataframe, a particular column which contains a certain value, you need to filter the dataframe using operators like '==', '>=', '<=', '>', '!=', etc.


```
df[df["sex"] == 'female']
```


![filtering-with-a-certain-alue](/pictures/python/filtering-dataframes/filtering-with-a-certain-value.PNG "filtering with a certain value")


- Now, we will deal with the age column of the titanic dataframe whose data type is not integer.


![age-is-not-int](/pictures/python/filtering-dataframes/age-is-not-int.PNG "age is not integer")


- In that case, we will wrap the object value in single quotes so that Python recognizes the value.


![find-object-type-with-single-quotes](/pictures/python/filtering-dataframes/find-the-age-with-quotes.PNG "find the age with quotes")


- We can then find the passengers with their pclass is either 2 or 3, then count the number of each class.


```
titanic[titanic["pclass"] != 1]["pclass"].value_counts()
```


![filter-first-value-counts](/pictures/python/filtering-dataframes/filter-first-value-counts.PNG "filter first and value counts")


- Let's use **between** method to filter our series.


```
houses[houses["bedrooms"].between(5, 7)]
```


![between-method](/pictures/python/filtering-dataframes/between-method.PNG "between method")


- We will particularly take the bedrooms column and see the breakdown of the numbers of the bedrooms.


![breakdown-of-the-number-of-bedrooms](/pictures/python/filtering-dataframes/breakdown-of-the-numbers-of-bedrooms.PNG "breakdown of the number of bedrooms")


- Next, we will use the method called **isin()**. We can use this when filtering a series to find values that are in a list of other values.


![country-filtering](/pictures/python/filtering-dataframes/country-filtering.PNG "country filtering")


- We will filter the netflix dataframe by its rating that is including ["TV-MA", "R", "PG-13"].


```
mature = netflix["rating"].isin(["TV-MA", "R", "PG-13"])

netflix[mature]
```


![store-series-into-a-variable](/pictures/python/filtering-dataframes/store-series-into-variable.PNG "store series into a variable")


- We can take two conditions, and when both of them are true, Python will return the result. In order for this to work, we use **&** operator.


```
women = titanic.sex == 'female'
died = titanic.survived == 0
titanic[women & died]
```

![filter-with-ampersand-operator](/pictures/python/filtering-dataframes/filter-with-and-operator.PNG "filter with ampersand operator")


- Instead of storing the conditions into a variable, you can directly put the conditions in square brackets when filtering a dataframe. One caveat is that you need to wrap up both the left and right side of the operands in the parenthesis. 


```
houses[(houses["waterfront"] == 1) & (houses["price"] < 500000)]
```


![satisfying-coditions-using-parens](/pictures/python/filtering-dataframes/satisfying-conditions-with-ampersand.PNG "satisfying conditions using parens and amperand operator")


- You can add more complex filter and conditions by using operators and sort_values() method.


```
houses[(houses["view"] == 4) & (houses["grade"].between(11, 13))].sort_values("price",ascending=True).head(5)
```


![complex-conditions](/pictures/python/filtering-dataframes/complex-conditions.PNG "complex conditions")


- With ampersand (&), both sides should be True to return True result. But with **pipe character (|)**, which means OR, only one side can be True and Python will still return them as True result.


```
houses[(houses["yr_built"] >= 2007) | (houses["yr_renovated"] > 2007)]
```

![with-or-operator-using-pipe](/pictures/python/filtering-dataframes/with-or-operator-using-pipe.PNG "with OR operator using pipe")


![newish-houses](/pictures/python/filtering-dataframes/newish-huses.PNG "newish houses")


- We can add three or four conditions or as many conditions as we want in:


```
netflix[(netflix["director"] == 'David Fincher') | (netflix["director"] == 'Martin Scorsese') & (netflix["release_year"] >= 2010)]
```


![three-conditions](/pictures/python/filtering-dataframes/three-conditions.PNG "three conditions")


- Next, we will use tilde character (~) to negate a condition. 


```
df = titanic.head()
women = df["sex"] == 'female'
df[~women]
```


![negate-codition-using-tilde](/pictures/python/filtering-dataframes/negate-a-condition.PNG "negate a codition using tilde")


```
newly_built = houses["yr_built"] >= 2014
newly_renoated = houses["yr_renovated"] >= 2014
new_houses = newly_built | newly_renovated
houses[~new_houses]
```


![how-to-negate-multiple-conditions](/pictures/python/filtering-dataframes/how-to-negate-multiple-conditions.PNG "how to negate multiple conditions")


- This is also possible to combine the condition within the same square bracket, but you need to be careful with the parenthesis and where to put tildes.


```
titanic[~(titanic["survived"] == 0) & ~(titanic["sex"] == 'male')]
```


![careful-with-parenthesis-andtilde](/pictures/python/filtering-dataframes/careful-with-parenthesis-and-tilde.PNG "careful with parenthesis and tildes")


- We use **isna()** method to find NaN values for a certail series.


![isna-method](/pictures/python/filtering-dataframes/isna-method.PNG "isna() method")


- This is how to use multiple **isna()** conditions:


```
netflix[netflix["director"].isna() & netflix["cast"].isna()]
```


![multiple-isna-conditions](/pictures/python/filtering-dataframes/multiple-isna-conditions.PNG "multiple isna() conditions")


- We use notna() to exclude all the null values from a specific series.


```
netflix[netflix["director"].notna() & netflix["cast"].notna()]
```


![notna-method](/pictures/python/filtering-dataframes/notna-method.PNG "notna() method")


- We use tilde (~) to invert the results.


```
netflix[~netflix["director"].notna() & ~netflix["cast"].notna()]
```

![tilde-to-invert-the-results](/pictures/python/filtering-dataframes/tilde-to-invert-the-result.PNG "tilde to invert the results")


- Also, create a bar chart out of the filtered data and their value counts.


```
titanic[titanic["sex"] == 'male']["survived"].value_counts().plot(kind = 'barh')
```


![pie-chart](/pictures/python/filtering-dataframes/pie-chart.PNG "pie chart")


