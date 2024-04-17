### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Basic Data Computations

- dataframe.min() will get you the minimum values of each column of the entire dataframe.


![dataframe-min](/pictures/python/basic-dataframe-computations/dataframe-min.PNG "dataframe min")


- dataframe.max() gives you the maximum values of each column.


![dataframe-max](/pictures/python/basic-dataframe-computations/dataframe-max.PNG "dataframe max")


- This list of values that Pandas gives you is called **Series**. It's Pandas' data structure.


```
type(houses.max())
```


![pandas-series](/pictures/python/basic-dataframe-computations/pandas-series.PNG "pandas series")



- dataframe.sum() will sum up the values of all the rows and give you the total sum of each column.


![dataframe-sum](/pictures/python/basic-dataframe-computations/dataframe-sum.PNG "dataframe sum")


- And Pandas concatenated texts in some field where the data type is not **numeric**. You can specify to compute only numeric values by:


```
houses.sum(numeric_only=True)
```


![numeric-only-true](/pictures/python/basic-dataframe-computations/numric-only-true.PNG "numeric_only = True")


- We can calculate mean, median, and mode of our dataframes.


![dataframe-mean](/pictures/python/basic-dataframe-computations/dataframe-mean.PNG "dataframe mean")


![dataframe-mode](/pictures/python/basic-dataframe-computations/dataframe-mode.PNG "dataframe-mode")



- dataframe.describe() gives us a descriptive statistics of the dataframe. 25%, 50%, and 75% in the descriptive statistics are percentile values. 50% percentile value is median.


![dataframe-describe](/pictures/python/basic-dataframe-computations/dataframe-describe.PNG "dataframe dssecribe")


- When we use dataframe describe method, it give us back the descriptive statistics in a dataframe form.


![descriptive-statistics-in-dataframe-form](/pictures/python/basic-dataframe-computations/descriptive-statistics-in-dataframe.PNG "descriptive statistics in a dataframe form")


- Some dataframes have more **objects** than numeric values. Can we still get descriptions on them?


![object-info](/pictures/python/basic-dataframe-computations/object-description.PNG "object description")


```
titanic.describe(include=["object"])
```


![object-include](/pictures/python/basic-dataframe-computations/object-include.PNG "include = [object]")


- If we want to see descriptive statistics on other values, you can provide a list of data types you want:


```
titanic.describe(include = ["object", "int64"])
```


![object-and-int](/pictures/python/basic-dataframe-computations/object-and-int.PNG "object and int")


- Also, inspect the Netflix dataframe to look for interesting info.


![describe-netflix](/pictures/python/basic-dataframe-computations/describe-netflix.PNG "describe netflix")


