### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Multi Indexing

- When the labels are hierarchical, we call that **multi indexing**. It means we can **group by multiple columns**.


```
titanic.groupby("sex").aggregate({"age": "mean"})

titanic.groupby(["pclass", "sex"]).aggregate({"age": "mean"})
```

![hierarchical-indexing](/pictures/python/hierarchical-indexing/hierarchical-indexing.PNG "hierarchical indexing")


- If we store this into a variable called **df** and run **df.index**, we will see it's **MultiIndex**.


```
df = titanic.groupby(["pclass", "sex"]).aggregate({"age": "mean"})

df.index
```

![multiindex](/pictures/python/hierarchical-indexing/multiindex.PNG "multiindex")


- It's represented as a list of tuples.


## Creating a Multiindex With set_index

- We will start by reading in a file called "state_pops". To conduct a more meaningful analysis, we will set its index to the state instead of the default numbered list.


```
pops = pd.read_csv("DataAnalysis/data/state_pops.csv")
```


![read-state-pops](/pictures/python/hierarchical-indexing/read-state-pops.PNG "read state pops")


```
pops.set_index("state")

pops
```

![set_index-to-state](/pictures/python/hierarchical-indexing/set_index-to-state.PNG "set index to state")


- But what we are gonna do is to use a combination of multiple columns as an index. And the order matters. Whatever order I provide, that will be one the multi index will be set up in. 


```
pops.set_index(["state", "year"])
```


![set_index-multiindex](/pictures/python/hierarchical-indexing/set_index-multiindex.PNG "set index multiindex")



## Sorting a Multiindex

- Now, let's sort our dataframe by index or indices. As we have already made the dataframe to have **multiindex** in place, we don't need to specify it's multiindex. If you just run **sort_index()**, it will do the work of sorting all indices in ascending order.


```
pops.sort_index()
```


![sort-index-will-do-the-work](/pictures/python/hierarchical-indexing/sort-index-will-do-the-work.PNG "sort index will do the work")


- But in case I want to sort the first index in an alphabetical order and then sort the second index in a descending order, I can do so by specifying **ascending** parameter for each level and also providing the list of the levels.


```
pops.sort_index(ascending=[True, False], level=[0, 1])
```


## Using loc[] with a Multiindex

- If someone asks me to find the Nevada's population in 2001, how do I do that?


```
pops.loc[("NV", 2001)]
```

![using-loc-to-find-specific-information](/pictures/python/hierarchical-indexing/using-loc-to-find-specific-info.PNG "using loc to find specific information")


- This version also works to find a particular row:


```
pops.loc["MT", 2011]
```

![using-loc-and-indices-pair](/pictures/python/hierarchical-indexing/using-loc-and-pair.PNG "using loc and a pair of indices")


- But passing in a tuple is a safer way to tell Python to find a particular row using multiindex.


- What if I want to get the population of Alaska from 1990 through 2000?


```
pops.sort_index().loc[("AK", 1990): ("AK", 2000)]
```

![sort_index-then-loc-with-slice](/pictures/python/hierarchical-indexing/sort_index-then-loc-with-slick.PNG "loc with slicing")



## Cross Sections with XS method

- If I put **:** first as a loc parameter, it means to skip the first index as is. Then I put the **2013** to only look at the dataset in 2013, and put **:** again to see all the dataset that corresponds to the condition.


```
pops.loc[:, 2013, :]
```


![using-colon-to-get-a-portion-of-dataset](/pictures/python/hierarchical-indexing/using-colon-to-get-a-portion-of-dataset.PNG "using colon to get a portion of dataset")


- We can also use the **xs method** to get a cross section of the dataframe and achieve the same thing as above.


```
pops.xs(2013, level="year")
```

![xs-method](/pictures/python/hierarchical-indexing/xs-method.PNG "xs method")


## get_level_values()

- If I want for some reason a list of the particular level in a hierarchical index, I can do so by running **dataframe.index.get_level_values()** method.


```
pops.index.get_level_values(0)

pops.index.get_level_values(1)
```


![get-level-values](/pictures/python/hierarchical-indexing/get_level-values.PNG "get level values")


- Let's say we want to have the index years that are even. How do we do that?


```
pops[pops.index.get_level_values(1) % 2 == 0]
```


![using-get_level_values-to-get-even-numbers](/pictures/python/hierarchical-indexing/use-get_level_values-to-get-even-numbers.PNG "using get_level_values() to get even numbered list")


- Let's look a portion of dataframe at the level **state** that ends with "A":


```
pops[pops.index.get_level_values(0).str[1]  == "A"]
```


![using-get_level_values()-method-with-str-function](/pictures/python/hierarchical-indexing/get_level_values-str-function.PNG "using get_level_values() method with str function")