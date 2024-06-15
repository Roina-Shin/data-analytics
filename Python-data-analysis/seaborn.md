### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Seaborn

- **Seaborn** is actually built on top of Matplotlib. It's data visualization library with a couple of goals or credos.


- It helps do complicated operations with a simple line of code, which makes it really powerful. Also, Seaborn comes with Pandas so we don't need to install it. We are gonna use a public seaborn dataset by using **load_dataset** method.


```
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

sns.load_dataset("tips")
```

![load_dataset-method](/pictures/python/seaborn/load_dataset-method.PNG "load_datset() method")


- We will also fetch penguins dataset:


```
penguins = sns.load_dataset("penguins")
```

![sns-load_dataset](/pictures/python/seaborn/sns-load_dataset.PNG "sns.load_dataset")



## Seaborn Scatterplots


```
sns.scatterplot(data=tips, x="total_bill", y="tip")
```


![provide-dataset-and-x-and-y-axes](/pictures/python/seaborn/provide-data-and-x-y-axis.PNG "provide data, x and y axes")


```
sns.set_theme()

sns.scatterplot(data=tips, x="total_bill", y="tip")
```


![sns-set_theme](/pictures/python/seaborn/sns-set_theme.PNG "sns set_theme")


- **hue** is another parameter that the seaborn method expects to see.


```
sns.scatterplot(data=tips, x="total_bill", y="tip", hue="smoker")
```


![hue-parameter](/pictures/python/seaborn/hue-parameter.PNG "hue parameter")


- But we are not limited to 2 values with the **hue parameter**.


```
sns.scatterplot(data=tips, x="total_bill", y="tip", hue="day")
```


![hue-paramter-with-more-than-2-values](/pictures/python/seaborn/hue-parameter-with-more-than-2-values.PNG "hue parameter with more than 2 values")


- So, it's very easy to incorporate additional columns using hue. But that's not actually the only option.


- Yet, we are gonna add a new argument that we have yet to see. **Style**.


```
sns.scatterplot(data=tips, x="total_bill", y="tip", hue="sex", style="time")
```


![style-parameter](/pictures/python/seaborn/style-parameter.PNG "style parameter")


- Also, we can use the same column for hue and style. And this makes it even easier to differentiate things.


```
sns.scatterplot(data=tips, x="total_bill", y="tip", hue="sex", style="sex")
```


![using-the-same-column-for-hue-and-style](/pictures/python/seaborn/using-the-same-column-for-hue-and-style.PNG "using the same column for hue and style")


- If you do have a **cramped or crowded scatterplot**, it's another way of differentiating things by using different colors and markers.


- **size** is another parameter we use to show data points from a size perspective.


```
sns.scatterplot(data=tips, x="total_bill", y="tip", hue="size", size="size")
```


![size-parameter](/pictures/python/seaborn/size-parameter.PNG "size parameter")


## Seaborn Lineplots


```
sum_of_passengers = flights.groupby("year")["passengers"].aggregate("sum")
plt.plot(sum_of_passengers)
```


![using-aggregate-method-to-sum](/pictures/python/seaborn/using-aggregate-method-to-sum.PNG "using aggregate method to sum")


- But we can provide our own estimator to aggregate across the dataframe.


```
sns.lineplot(data=flights, x="year", y="passengers", estimator="sum")
```

![estimator-sum](/pictures/python/seaborn/estimator-sum.PNG "estimator sum")


```
sns.lineplot(data=flights, x="year", y="passengers", hue="month")
```

![lineplot-hue-parameter](/pictures/python/seaborn/lineplot-hue-parameter.PNG "lineplot hue parameter")


- This time, we will use datetime "hour" and make a line plot on top of that.


```
taxis = sns.load_dataset("taxis", parse_dates=["pickup", "dropoff"])
taxis.info()

taxis["pickup_hour"] = taxis["pickup"].dt.hour

sns.lineplot(data=taxis, x="pickup_hour", y="fare")
```

![datetime-hour](/pictures/python/seaborn/datetime-hour-and-lineplot.PNG "datetime hour and lineplot")


```
sns.lineplot(data=taxis, x="pickup_hour", y="fare", hue="payment")
```

![hue-payment](/pictures/python/seaborn/hue-payment.PNG "hue payment")


