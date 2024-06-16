### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Seaborn Relplot

- If we use **relplot** this is how we can generate multiple subplots with a more complex layout.


- The **relplot()** method is expecting us to pass in a kind, "scatter" or "line". There's only 2 types of relplot.


```
tips = sns.load_dataset("tips")

sns.set_theme()

sns.relplot(data=tips, x="total_bill", y="tip")
```


![using-relplot](/pictures/python/seaborn2/using-relplot.PNG "using relplot")


- If we do **columns by sex**, we end up with 2 plots broken up by sex.


```
sns.relplot(data=tips, x="total_bill", y="tip", kind="scatter", col="sex")
```


![col-parameter](/pictures/python/seaborn2/col-parameter.PNG "col parameter")


- relplot() is a figure-level method that it can generate an entire figure with multiple subplots.


```
sns.relplot(data=tips, x="total_bill", y="tip", hue="smoker", col="sex")
```

![hue-and-col-parameters](/pictures/python/seaborn2/hue-and-col-parameters.PNG "hue and col parameters fol relplot() method")


```
sns.relplot(data=tips, x="total_bill", y="tip", hue="smoker", col="sex", row="time")
```

![hue-col-row-parameters](/pictures/python/seaborn2/hue-col-row-parameters.PNG "hue col row parameters")


```
taxis = sns.load_dataset("taxis", parse_dates=["pickup", "dropoff"])
taxis["pickup_hour"] = taxis["pickup"].dt.hour

sns.relplot(data=taxis, x="pickup_hour", y="total", kind="line")
```

![relplot-line](/pictures/python/seaborn2/relplot-line.PNG "relplot line")


```
sns.relplot(data=taxis, x="pickup_hour", y="total", kind="line", col="pickup_borough")
```


![broken-down-by-pickup_borough](/pictures/python/seaborn2/col-broken-down-by-pickup_borough.PNG "col broken down by pickup_borough")


- So, that's an introduction to **relplot in Seaborn**, where you can create grids of plots across different columns and rows.



## Resizing Seaborn Plots

- This is how we control the figsize of the axis-level method like below:


```
flights = sns.load_dataset("flights")
plt.figure(figsize=(10, 4))
sns.lineplot(data=flights, x="year", y="passengers", estimator="sum")
```

![plt-figure-figsize](/pictures/python/seaborn2/plt-figure-figsize.PNG "plt.figure(figsize=())")


- But, when we are actually working with figure-level methods such as relplot, plt.figure() won't impact the outcome because, the relplot() will create its own figure.


- The **height** parameter is going to control the height of each individual subplot, which Seaborn calls **a facet**.


```
sns.relplot(data=tips,
            x="total_bill",
            y="tip",
            kind="scatter",
            hue="smoker",
            col="sex",
           height=3)
```


![control-the-height-of-each-subplot](/pictures/python/seaborn2/control-height-of-each-subplot.PNG "control the height of each individual subplot")


- But, what about the width? There's no parameter called width but we can provide **aspect** instead. The **aspect** is the ratio of the width to the height.


```
sns.relplot(data=tips,
            x="total_bill",
            y="tip",
            kind="scatter",
            hue="smoker",
            col="sex",
           height=3,
           aspect=2)
```


![aspect-is-the-ratio-of-the-width-to-the-height](/pictures/python/seaborn2/aspect-is-the-ratio-of-the-width-to-the-height.PNG "aspect is the ratio of the width to the height")


- So, remember **the height and aspect control the individual subplots' size, such as a facet in the grid**.


