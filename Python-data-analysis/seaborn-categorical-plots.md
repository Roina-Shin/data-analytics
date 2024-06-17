### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Seaborn Countplot

- **countplot()** method is very similar to histogram. It shows us **the distribution of occurrences of certain values in our data**.


- But it's made to work with **categorical data**, so non-numeric values.


- If I want to count the occurrences of penguin species, I can provide "species" for the **x axis**. Then it will go ahead and group them and count the number of times each species occurs.


```
sns.countplot(data=penguins, x="species")
```


![first-countplot](/pictures/python/seaborn-categorical-plots/first-countplot.PNG "first countplot")


```
titanic = pd.read_csv("DataAnalysis/data/titanic.csv")

sns.countplot(data=titanic, x="pclass", hue="sex")
```

![countplot-xaxis-hue](/pictures/python/seaborn-categorical-plots/countplot-xaxis-hue.PNG "countplot xaxis and hue")


- We could have done the same thing in Pandas, but it is much more complex process. So, countplot() is one of the most simple methods in terms of what it can do in Seaborn.


```
plt.figure(figsize=(5, 3))
titanic.groupby(["pclass", "sex"])["sex"].aggregate("count").plot(kind="bar", color=["blue", "orange"])
```

![pandas-plotting](/pictures/python/seaborn-categorical-plots/pandas-plotting.PNG "pandas plotting")



## Strip and Swarm Plots


```
taxis = sns.load_dataset("taxis")
sns.scatterplot(data=taxis, x="pickup_borough", y="distance")
```

![seaborn-scatterplot](/pictures/python/seaborn-categorical-plots/seaborn-scatterplot.PNG "seaborn scatterplot")


- **stripplot()** is a similar method to the scatterplot() but it **expects one axis to be categorial values**.


```
sns.set_theme()
sns.stripplot(data=taxis, x="pickup_borough", y="distance", hue="pickup_borough")
```

![first-stripplot](/pictures/python/seaborn-categorical-plots/first-stripplot.PNG "first stripplot")


- But we still have a problem where **if things are dense enough, it really just forms a big strip** that is solid colored, that we can't really tell how many trips in a certain city had a distance of 10 versus 15, for example. So that is where a **swarmplot()** method comes in.


- The problem with the **swarmplot()** is that it will plot every single point and not have any overlap. So, you first need to make sure that you have a **manageable dataset**.


```
taxis_nlargest = taxis.nlargest(600, "total")
sns.swarmplot(data=taxis_nlargest, x="pickup_borough", y="distance", hue="pickup_borough")
```

![first-swarmplot](/pictures/python/seaborn-categorical-plots/first-swarmplot.PNG "first swarmplot")


- The beauty of swarmplot() is that you can see how many data points are at a certain value of Y axis for each categorical value. We can enlarge the figure size by using **plt.figure()** method. Now, none of the data points are overlapping.


```
taxis_nlargest = taxis.nlargest(600, "total")
plt.figure(figsize=(12,5))
sns.swarmplot(data=taxis_nlargest, x="pickup_borough", y="distance", hue="pickup_borough")
```


![enlarged-figure](/pictures/python/seaborn-categorical-plots/enlarged-figure.PNG "enlarged figure")


- Compare that with the stripplot() data points that are not as obvious. So, swarmplot is very useful when you want to see every single data point uniquely. But, it's gonna be rough when you have a large dataset.


![stripplot-data-points](/pictures/python/seaborn-categorical-plots/stripplot-overlapping-data-points.PNG "stripplot overlapping data points")



## Boxplots

- Boxplots are a good way to quickly visualize the distribution, especially when we have multiple levels of categorical values.


```
sns.boxplot(data=taxis, x="pickup_borough", y="distance")
```

![first-boxplot](/pictures/python/seaborn-categorical-plots/first-boxplot.PNG "first boxplot")


- The **whis** parameter controls the **proportion of the interquartile range** to extend the plot whiskers.


```
sns.boxplot(data=taxis, x="pickup_borough", y="total", whis=1)
```


![whis-parameter](/pictures/python/seaborn-categorical-plots/whis-proportion-of-the-interquartile-range.PNG "whis - the proportion of the interquartile range")


```
sns.boxplot(data=titanic, x="pclass", y="age", hue="sex")
```

![titanic-boxplot](/pictures/python/seaborn-categorical-plots/titanic-boxplot.PNG "titanic boxplot")


- You can even control the size of outliers (fliers) by using **fliersize** parameter.


```
sns.boxplot(data=titanic, x="pclass", y="age", hue="sex", fliersize=3)
```

![fliersize](/pictures/python/seaborn-categorical-plots/fliersize.PNG "fliersize")


- We can overlay the swarmplot on top of the boxplot so that we can see the distribution of individual data points.


```
penguins = sns.load_dataset("penguins")
sns.boxplot(data=penguins, x="species", y="body_mass_g")
sns.swarmplot(data=penguins, x="species", y="body_mass_g", color="grey")
```

![overlay-swarmplot](/pictures/python/seaborn-categorical-plots/overlay-swarmplot.PNG "overlay swarmplot")

