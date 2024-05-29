### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Pandas Histogram

- Histogram is going to help us visualize the distribution of data. And you can control the number of bins by using **bins** parameter, to control how many chunks the data is broken into.


```
df["BasePay"].plot(kind="hist", bins=20)
```


![bins-parameter](/pictures/python/plotting-with-pandas-and-matplotlib2/bins-parameter.PNG "bins paramter")



## Box Plots

- Box Plot is another way of visualizing the distribution of values in a column.


- The box extends from the Q1 to Q3 quartile values of the data, with a line at the median(Q2). The whiskers extend from the edges of the box to show the range of the data. Outlier points are those past the end of the whiskers.


![box-plot-explained](/pictures/python/plotting-with-pandas-and-matplotlib2/box-plot-explained.PNG "box plot explained")

source: https://www.simplypsychology.org/boxplots.html


- To use box plot in pandas, we either use **.plot(kind="box")** or **.boxplot()** method.


![box-plot-for-bedrooms](/pictures/python/plotting-with-pandas-and-matplotlib2/box-plot-for-bedrooms.PNG "box plot for bedrooms")


- If we don't want to see outlier values, we use **showfliers=False** parameter.


```
houses["bedrooms"].plot(kind="box", showfliers=False)
```

![showfliers-false](/pictures/python/plotting-with-pandas-and-matplotlib2/showfliers-false.PNG "showfliers false")


## Line Plots


```
ufos["year"] = ufos["date_time"].dt.year

ufos.year.value_counts().sort_index().plot(kind="line")
```


![line-plot](/pictures/python/plotting-with-pandas-and-matplotlib2/line-plot.PNG "line plot")



- If you set a **linewidth** to 30, you get a very chunky like below:


```
ufos["year"].value_counts().sort_index().plot(kind="line", linewidth=30)
```


![chunky-line-plot](/pictures/python/plotting-with-pandas-and-matplotlib2/chunky-line-plot.PNG "chunky line plot")



## Exercise


```
postman_artists = billboard[billboard["song"] == "Please Mr. Postman"]["artist"].value_counts()
postman_artist_names = postman_artists.index.values
plt.pie(postman_artists, labels=postman_artist_names, autopct="%1.0f%%")
plt.title("Please Mr. Postman Versions")
plt.ylabel("artist")
```


![pie-plot-exercise](/pictures/python/plotting-with-pandas-and-matplotlib2/pie-plot-exercise.PNG "pie plot exercise")


- I'll also explode one piece for Gentle Persuasion so that it stands out from the rest of the pie.


```
postman_artists = billboard[billboard["song"] == "Please Mr. Postman"]["artist"].value_counts()
postman_artist_names = postman_artists.index.values
plt.pie(postman_artists, labels=postman_artist_names, autopct="%1.0f%%", explode=(0,0,0.1))
plt.title("Please Mr. Postman Versions")
plt.ylabel("artist")
```

![explode-one-wedge](/pictures/python/plotting-with-pandas-and-matplotlib2/explode-one-wedge.PNG "explode one wedge")



## Scatter Plot


```
houses.plot.scatter(x="bedrooms", y="bathrooms")
```

![compare-bathrooms-and-bedrooms](/pictures/python/plotting-with-pandas-and-matplotlib2/plot-scatter-x-and-y.PNG "plot scatter x and y")


- Now, we can see a rough distribution of bedrooms and bathrooms and how they compare and correlate to one another.


- The most important arguments are going to be passing in the X and Y columns and it is a **dataframe** method.


```
houses.plot.scatter(x="bedrooms", y="bathrooms")
```


