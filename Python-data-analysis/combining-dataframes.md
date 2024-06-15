### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Combining Dataframes

- **pd.concat()** is a Pandas method to concatenate either series or dataframe.


```
import pandas as pd

s1 = pd.Series(["a", "b", "c"])
s2 = pd.Series(["d", "e", "f", "z"])

pd.concat([s1, s2])
```


![pd-series-concat](/pictures/python/concatenating-series/pd-series-concat.PNG "pd series concat")


- To create a new index for the new series, you can use **ignore_index=True** as a parameter.


```
pd.concat([s1, s2], ignore_index=True)
```


![ignore_index-True](/pictures/python/concatenating-series/ignore_index-true.PNG "ignore_index=True")


## Concatenating Series by Index

- Instead of stacking them, you can **concatenate horizontally**, putting them side by side to make a new dataframe.


```
c1 = pd.Series(["red", "orange", "yellow"])
c2 = pd.Series(["green", "blue", "purple"])

pd.concat([c1, c2])

pd.concat([c1, c2], axis=1)
```

![concat-series-axis-1](/pictures/python/concatenating-series/concat-series-axis-one.PNG "concat series axis one")


- When I concatenate two series with spefied index, it concatenates using that index.


```
fruits = pd.Series(data = ["apple", "banana", "cherry"], index = ["a", "b", "c"])
animals = pd.Series(data = ["badger", "cougar", "anaconda"], index = ["b", "c", "a"])

pd.concat([fruits, animals], axis=1)
```


![concatenate-by-using-index](/pictures/python/concatenating-series/concatenate-by-using-index.PNG "concatenate by using index")


- If we want to rename the columns, we can add **keys=[" "]** argument to provide a list of column names.


```
pd.concat([fruits, animals], axis=1, keys=["fruit", "animal"])
```


![keys-for-column-names](/pictures/python/concatenating-series/keys-for-column-names.PNG "keys for column names")



## Inner vs. Outer Joins


```
fruits = pd.Series(data = ["apple", "banana", "cherry", "durian"], index = ["a", "b", "c", "d"])
animals = pd.Series(data = ["badger", "cougar", "anaconda", "elk", "pika"], index = ["b", "c", "a", "p", "e"])

pd.concat([fruits, animals], axis=1)
```

- When we are concatenating two series that are not exactly matching, it fills in the empty spaces with NaN values.


![nan-values](/pictures/python/concatenating-series/nan-values.PNG "nan values")


- The **outer join** is gonna keep in everything while the **inner join** only shows the overlapping part.


```
pd.concat([fruits, animals], axis=1, join="inner")
```


![inner-join-in-concat](/pictures/python/concatenating-series/inner-join-in-concat.PNG "inner join in concat")



## Concatenating Dataframes with Columns


```
harvest_24 = pd.DataFrame([['potatoes', '900'], ['garlic', 1350], ['onions', 875]], columns=['crop', 'qty'])
harvest_25 = pd.DataFrame([['garlic', 1600], ['spinach', 560], ['turnips', 999], ['onions', 1000]], columns=['crop', 'qty'])


pd.concat([harvest_24, harvest_25], keys=[2024, 2025]) 
```


![concatenating-dataframes](/pictures/python/concatenating-series/concatenating-dataframes.PNG "concatenating dataframes")


```
livestock = pd.DataFrame([["pasture", 9], ["stable", 3], ["coop", 34]], columns=["location", "qty"], index=["alpaca", "horse", "chicken"])

weights = pd.DataFrame([[4, 10], [800, 2000], [1.2, 4], [110, 150]], columns=["min_weight", "max_weight"], index=["chicken", "horse", "duck", "alpaca"])
```

![joining-two-dataframes](/pictures/python/concatenating-series/joining-two-dataframes.PNG "joining two dataframes")


- If I use **axis=1** argument, it's going to join two dataframes on the common indices:


```
pd.concat([livestock, weights], axis=1)
```

![joining-on-common-indices](/pictures/python/concatenating-series/joining-on-common-indices.PNG "joining on common indices")


- If you only want the rows where everything matches up, you can use **join="inner"** argument:


```
pd.concat([livestock, weights], axis=1, join="inner")
```


![inner-join-axis-1](/pictures/python/concatenating-series/inner-join-axis-1.PNG "inner join axis=1")



## The DataFrame Merge() Method

- The idea with the Merge() method is that it is a SQL or Database style join. We are merging 2 dataframes based upon common columns.


```
teams = pd.DataFrame(
    [
        ["Suns", "Phoenix", 20, 4],
        ["Mavericks", "Dallas", 11, 12],
        ["Rockets", "Houston", 7, 16],
        ["Nuggets", "Denver", 11, 12] ],
    columns = ["Team", "City", "Wins", "Losses"])

cities = pd.DataFrame(
    [
        ["Houston", "Texas", 2310000],
        ["Phoenix", "Arizona", 1630000],
        ["San Diego", "California", 1410000],
        ["Dallas", "Texas", 1310000]
    ],
    columns = ["City", "State", "Population"])

teams.merge(cities)
```

![merge-method](/pictures/python/concatenating-series/merge-method.PNG "merge method")


- The **how** parameter for merge() method allows us to change the type of merge that is being performed. The default behavior is to perform an **inner join**.


- If we do a left join, we will get everything from the left, even if there's no corresponding match from the cities dataframe.


![how-parameter](/pictures/python/concatenating-series/how-parameter.PNG "how parameter")


- If we want to get every single value from the 2 dataframes, we can do **outer join** instead. It doesn't matter if there was a match or not.


```
teams.merge(cities, how="outer")
```


![outer-join-using-merge](/pictures/python/concatenating-series/outer-join-using-merge.PNG "outer join using merge")



## Merge() On and Suffixes Arguments

- The **on** argument allows us to specify the column or the columns that are common in 2 or more dataframes.


```
first = pd.DataFrame(
[["alex", "padilla", 92], 
 ["rayna", "wilson", 83],
 ["juan", "gomez", 78],
 ["angela", "smith", 66],
 ["stephen", "yu", 98]],
    columns = ["first", "last", "score"]
)
first

finals = pd.DataFrame(
    [
        ["alex", "padilla", 97, False],
        ["rayna", "wilson", 88, False],
        ["alex", "smith", 86, True],
        ["juan", "gomez", 71, True],
        ["stephen", "yu", 91, False],
        ["sakura", "steel", 100, True]
    ],
    columns = ["first", "last", "score", "extra_credit"]
)
finals
```


- And we do joins on 2 columns: first and last.


```
first.merge(finals, on=["first", "last"])
```


![on-parameter](/pictures/python/concatenating-series/on-parameter.PNG "on parameter")


- Also, Pandas automatically added a **suffix** x and y to "score" column that is in both dataframes. We can change that by using a parameter called **suffixes**.


```
first.merge(finals, on=["first", "last"], suffixes=("_first", "_finals"))
```


![suffixes-parameter](/pictures/python/concatenating-series/suffixes-parameter.PNG "suffixes parameter")


```
combo["avg"] = (combo["score_first"] + combo["score_finals"]) / 2 

combo.sort_values("avg", ascending=False)
```


![analysis](/pictures/python/concatenating-series/analysis.PNG "analysis")


- If we give some extra credit where extra credit is True, then we can run:


```
combo.loc[combo["extra_credit"] == True, "avg"] += 5
```


![where-extra-credit-is-true](/pictures/python/concatenating-series/where-extra-credit-is-true.PNG "where extra credit is true")