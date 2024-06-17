### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Boxenplots

- **Boxenplot** is very similar to boxplot but it shows us a bunch of smaller boxes and a fewer outliers.


```
sns.boxenplot(data=taxis, x="pickup_borough", y="total")
```


![first-boxenplot](/pictures/python/seaborn-categorical-plots2/first-boxenplot.PNG "first boxenplot")


- It works well to show you a larger dataset that may have more outliers when you plot them as a regular boxplot.



## Violinplots

- Violinplot plays a similar role as the boxplot and KDE (Kernel Density Estimation) plot. And it helps us visualize the distribution of data across, usually, categorical columns.


```
sns.violinplot(data=titanic, x="pclass", y="age")
```

![first-violinplot](/pictures/python/seaborn-categorical-plots2/first-violinplot.PNG "first violinplot")


```
sns.violinplot(data=titanic, x="pclass", y="age", hue="sex")
```


![violinplot-hue-sex](/pictures/python/seaborn-categorical-plots2/violinplot-hue.PNG "violinplot hue")


```
sns.violinplot(data=titanic, x="pclass", y="age", hue="sex", split=True)
```

![split-true-for-violinplot](/pictures/python/seaborn-categorical-plots2/split-true-for-violinplot.PNG "split=True for violinplot")


- Split parameter makes it easier to compare the distribution like a split violin.


## Barplot

- **barplot()** shows us the mean by default. Just like **lineplot**, barplot method has an **estimator argument** and it defaults to mean. So, it is actually performing a mean function on all of the distances in each city.


```
sns.barplot(data=taxis, x="pickup_borough", y="distance")
```

![first-barplot](/pictures/python/seaborn-categorical-plots2/first-barplot.PNG "first barplot")


- But if we change the estimator to "sum":


```
sns.barplot(data=taxis, x="pickup_borough", y="distance", estimator="sum")
```

![barplot-estimator-sum](/pictures/python/seaborn-categorical-plots2/changed-estimator-for-barplot.PNG "changed estimator")


- To get the exact same result in Pandas:


```
taxis.groupby("pickup_borough")["distance"].aggregate("sum").sort_values(ascending=False).plot(kind="bar", color=["blue", "orange", "green", "red"])
```


![pandas-groupby-aggregate](/pictures/python/seaborn-categorical-plots2/pandas-groupby-aggregate.PNG "pandas groupby aggregate")


```
sns.barplot(data=taxis, x="pickup_borough", y="total", hue="color")
```

![barplot-hue](/pictures/python/seaborn-categorical-plots2/barplot-hue.PNG "barplot hue")


```
sns.violinplot(data=taxis, x="pickup_borough", y="distance", hue="color", split=True)
```

![split-violin](/pictures/python/seaborn-categorical-plots2/split-violin.PNG "split violin")


- It's a little cramped so I'll the figsize.


```
plt.figure(figsize=(10,4))
sns.violinplot(data=taxis, x="pickup_borough", y="distance", hue="color", split=True)
```

![changed-figsize](/pictures/python/seaborn-categorical-plots2/changed-figsize.PNG "changed figsize")


- We use **orient="h"** paramter to change the bar into horizontal.


```
sns.barplot(data=titanic, x="survived", y="pclass", estimator="sum", orient="h")
```


![orient-h](/pictures/python/seaborn-categorical-plots2/orient-h.PNG "orient h")



## The Big Boy Catplot Method


- **Catplot()** works in a very similar way to a **relplot()**.


```
sns.catplot(data=titanic, x="sex", y="survived", kind="bar", col="pclass")
```


![first-catplot](/pictures/python/seaborn-categorical-plots2/first-catplot.PNG "first catplot")


```
sns.catplot(data=taxis, kind="strip", x="pickup_borough", y="distance", col="color")
```

![catplot-with-various-parameters](/pictures/python/seaborn-categorical-plots2/catplot-with-various-parameters.PNG "catplot with various parameters")


```
sns.catplot(data=taxis, x="pickup_borough", y="distance", kind="violin", hue="color", split=True)
```


![catplot-with-violin](/pictures/python/seaborn-categorical-plots2/catplot-with-violin.PNG "catplot with violin")


```
sns.catplot(data=taxis, x="pickup_borough", y="distance", kind="violin", hue="color", split=True, height=4, aspect=2)
```

![height-and-aspect-for-catplot](/pictures/python/seaborn-categorical-plots2/height-and-aspect.PNG "height and aspect for catplot")


