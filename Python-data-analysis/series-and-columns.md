### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Working with Columns

- We have two syntaxes we can use to select columns from a dataframe. The dot syntax is shorter, but it is more limited in what it can do. For example, the square bracket option allows us to pass in multiple columns instead of one.


```
df.age

df['age']
```

- First, load the 3 dataframed from 3 different csv files. Then, use Titanic file to create a dataframe.


```
import pandas as pd

houses = pd.read_csv("DataAnalysis/data/kc_house_data.csv")
titanic = pd.read_csv("DataAnalysis/data/titanic.csv")
netflix = pd.read_csv("DataAnalysis/data/netflix_titles.csv", sep="|", index_col = 0)
```


![syntax-for-seeing-columns](/pictures/python/series-and-columns/syntax-for-seeing-columns.PNG "syntax for seeing columns")



## Series

- According to Pandas, a series is one-dimensional ndarray with axis labels. In other words, a series is an unnested list of values. You can think that a dataframe consists of a bunch of series.


![series-example](/pictures/python/series-and-columns/series-example.PNG "series example")


- When we want a description on a particular series from a dataframe, we can first pick the column and then use 'describe' with a pair of parenthesis.


![describe-specific-columns](/pictures/python/series-and-columns/describe-specific-column.PNG "describe specific columns")



- Now, we will use a different method in relation to series. That is unique(). It will give you a collection of unique values of that series. 


![series-unique-method](/pictures/python/series-and-columns/series-unique-method.PNG "series unique method")


- If we want to count the number of unique values, nunique() will just do that. And this will by default exclude NaN values.


![nunique-method](/pictures/python/series-and-columns/nunique-method.PNG "nunique method")


- But, what if we want to count the number of unique values including NaN values? First, to see which columns have potentiall have NaN values, we can first run the info() method on our data frame.


![how-to-find-columns-with-missing-values](/pictures/python/series-and-columns/how-to-find-columns-with-missing-values.PNG "how to find columns with missing values")



- Then run the code with **dropna = False**.


![nunique-dropna-false](/pictures/python/series-and-columns/nunique-dropna-false.PNG "nunique dropna = false")


- nlargest() method is useful to quickly figure out the largest values of a series.


![nlargest-method](/pictures/python/series-and-columns/nlargest-method.PNG "nlargest method")


- If we have lots of duplicate values inside a series, we can control whether to show them all or not by using **keep** parameter inside the parenthesis.


![keep-all-parameter-for-nlargest-method](/pictures/python/series-and-columns/keep-all-parameter-for-nlargest-method.PNG "keep = all parameter for nlargest method")


- Another way to use the nlargest() method is to put it right beside the dataframe and specify the number of rows and column you want to apply the method to.


![another-way-to-use-nlargest-method](/pictures/python/series-and-columns/another-way-to-use-nlargest-method.PNG "another way to use nlargest() method")


- We can pass in multiple columns into that nlargest() method instead of just one column.


![multiple-columns](/pictures/python/series-and-columns/multiple-columns.PNG "multiple columns")


- nsmallest() method does the opposite to the nlargest() method. As its name implies, it shows the smallest values in a specific series with or without a defined number of them.


![nsmallest-method](/pictures/python/series-and-columns/nsmallest-method.PNG "nsmallest-method")


- Previously, at the beginning of this chapter, we said that the square bracket method of series allows you to pass in multiple values into it. Now, we will experiment with that to see how they work.


- When we do the below thing, it returns a dataframe with two specfic columns we passed in rather than a series like before.


![multiple-columns-inside-square-bracket](/pictures/python/series-and-columns/multiple-columns-inside-square-bracket.PNG "multiple columns inside square bracket")


- In other words, when we plug in a list instead of just one column, then it will return a dataframe and not a series.


![dataframe-not-a-series](/pictures/python/series-and-columns/dataframe-not-a-series.PNG "dataframe not a series")


- Now, we will explore an interesting series method called **value_counts()**. It will give us the breakdown of the counts of every unique values of that specfic column.


![value_counts-method](/pictures/python/series-and-columns/value_counts-method.PNG "value counts method")


- If want only top ten count values from that value_counts(), we can add **head(10)** method at the end so that it shows only top ten counts.


![value-counts-head-method](/pictures/python/series-and-columns/value_counts-head-method.PNG "value counts head method")


- And this value_counts() method is by default showing us in descending order. But we can change that into ascending order by adding **ascending=True** parameter there.


![ascending-true-parameter](/pictures/python/series-and-columns/ascending-true-parameter.PNG "ascending true parameter")


- We can also count the number of value pairs like below:


![value-counts-another-use-case](/pictures/python/series-and-columns/value_counts-another-use-case.PNG "value counts another use case")


- Finally, we will see how plot() method works. plot() will take the values and plot those against the labels. The line plot is the default graph.


![plot-method](/pictures/python/series-and-columns/plot-method.PNG "plot method")


- And **kind** option allows us to choose which plot we want to go with. 


![plot-kind](/pictures/python/series-and-columns/plot-kind.PNG "plot kind")


- I could also have done a pie chart. 


![plot-kind-pie](/pictures/python/series-and-columns/plot-kind-pie.PNG "plot kind pie")


![plot-kind-pie-another-example](/pictures/python/series-and-columns/plot-kind-pie-another-example.PNG "plot kind pie another example")


- Also, we can save two series as a variable and use their values to plot, while using x, y in conjunction with two series so that x axis corresponds to bathrooms and y axis corresponds to bedrooms. 


![store-two-series-as-variable](/pictures/python/series-and-columns/store-two-columns-as-variable.PNG "store two values as a variable")


- We can now switch the plot kind to "scatter" so that we can see if there's any correlation between the number of bathrooms and bedrooms. As a result, we see the overall distribution of bathrooms and bedrooms as well as the most common number of bathrooms and bedrooms.


![scatter-plot](/pictures/python/series-and-columns/scatter-plot.PNG "scatter plot")


![bar-chart-plotting](/pictures/python/series-and-columns/bar-chart-plotting.PNG "bar chart plotting")


![plot-kind-barh](/pictures/python/series-and-columns/plot-kind-barh.PNG "plot kind barh")


- For histogram, you can just put "hist" as the plot kind parameter.


![histogram](/pictures/python/series-and-columns/histogram.PNG "histogram")


