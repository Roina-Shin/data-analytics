### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Plotting with Pandas + Matplotlib

- Now, we can use built-in Pandas plot methods in conjunction with Matplotlib's plot methods to make more customized plots.


- First, import **Pandas** and **Matplotlib.pyplot** and read Titanic dataset. And convert the age column from object into numeric by using **to_numeric** method and the parameter **error="coerce"**.


```
import pandas as pd
import matplotlib.pyplot as plt

titanic = pd.read_csv("DataAnalysis/data/titanic.csv")
titanic.info()

titanic["age"] = pd.to_numeric(titanic["age"], errors="coerce")
```

![pd-to_numeric-errors-coerce](/pictures/python/plotting-with-pandas-and-matplotlib/pd-to_numeric-error-coerce.PNG "pd to_numeric errors coerce")



## Changing Pandas Plot Styles


- You can use plt after creating a plot using Pandas as Pandas is calling Matplotlib behind the scenes for plotting. So if you use **plt.** after plotting in Pandas, the matplotlib references the current plot you are working on as it always would in any other Matplotlib methods.


```
ufos["month"] = ufos["date_time"].dt.month

ufos.month.value_counts().sort_index().plot(kind="bar")
plt.title("UFO Sightings by Month")
```


![ufo-sightings-by-month](/pictures/python/plotting-with-pandas-and-matplotlib/ufo-sightings-by-month.PNG "ufo sightings by month")


```
plt.figure(figsize=(7,4))
sightings = ufos["month"].value_counts().sort_index().plot(kind="bar")
sightings
plt.title("UFO Sightings By Month")
plt.xlabel("Month")
plt.ylabel("Num of Sightings")
```

![xlabel-and-ylabel](/pictures/python/plotting-with-pandas-and-matplotlib/xlabel-and-ylabel.PNG "xlabel and ylabel")


- Now, we will see how to change your index names into other names by using **rename** method.


```
ufos["month"] = ufos["date_time"].dt.month

sightings = ufos["month"].value_counts()

sightings.rename({1: "Jan", 2: "Feb", 3: "Mar", 4: "Apr", 5: "May", 6: "Jun", 7: "Jul", 8: "Aug", 9: "Sep", 10: "Oct", 11: "Nov", 12: "Dec"})
```


![rename-method-dictionary](/pictures/python/plotting-with-pandas-and-matplotlib/rename-method-dictionary.PNG "rename method dictionary")


- You need to pass in a **dictionary** that contains key-value pairs to turn the numbers into Month names.


- And don't forget to add **inplace=True** to make the change effective on your dataset.


```
sightings.rename({1: "Jan", 2: "Feb", 3: "Mar", 4: "Apr", 5: "May", 6: "Jun", 7: "Jul", 8: "Aug", 9: "Sep", 10: "Oct", 11: "Nov", 12: "Dec"}, inplace=True)
```


- To wrap up, you first need to create a dictionary defining numbers into months names. And apply that dictionary to your series and then do plotting on it.


```
ufos["month"] = ufos["date_time"].dt.month

sightings_month = ufos["month"].value_counts().sort_index()

dict_months = {1: "Jan", 2: "Feb", 3: "Mar", 4: "Apr", 5: "May", 6: "Jun", 7: "Jul", 8: "Aug", 9: "Sep", 10: "Oct", 11: "Nov", 12: "Dec"}

sightings_month.rename(dict_months).plot(kind="bar")
plt.title("UFO Sightings By Month")
plt.xlabel("Month")
plt.ylabel("Num Sightings")
```


![dict-month-and-rename-and-plotting](/pictures/python/plotting-with-pandas-and-matplotlib/dict-month-and-plotting.PNG "dict month + rename + plotting")


![ufo-sightings-by-month-completed](/pictures/python/plotting-with-pandas-and-matplotlib/ufo-sightings-by-month-completed.PNG "ufo sightings by month completed")



## Closer Look at Pandas Bar Plots


- First, read "salaries** file with Pandas. And convert some pay data into numeric data by using **to_numeric** method and the **erros="coerce"** parameter. 


- And we are specifically looking at the Employee name and their base + overtime + other pays, so:


```
df = salaries[["EmployeeName", "BasePay", "OvertimePay", "OtherPay"]]
```


![salaries-dataset](/pictures/python/plotting-with-pandas-and-matplotlib/salaries-dataset.PNG "salaries dataset")


- Also, set the Employee Name as the index column using **set_index** method.


```
df.set_index("EmployeeName", inplace=True)
```


```
plt.figure(figsize=(6,3))
df.head(10).plot(kind="bar")
```


![creating-a-bar-plot](/pictures/python/plotting-with-pandas-and-matplotlib/creating-a-bar-plot.PNG "creating a bar plot")


- To make it a **stacked bar chart**, you can add **stacked=True** as a parameter to the plot method.


```
df.head(10).plot(kind="bar", stacked=True)
```


![stacked-bar-plot](/pictures/python/plotting-with-pandas-and-matplotlib/stacked-bar-plot.PNG "stacked bar plot")


```
df["BasePay"].sort_values(ascending=False).head(5).plot(kind="bar")
plt.title("Highest Paid SF Employees")
```


![bar-plot-for-highest-base-pay](/pictures/python/plotting-with-pandas-and-matplotlib/bar-plot-for-highest-base-paid.PNG "bar plot for highest base pays")



```
df.sort_values("OvertimePay", ascending=False).head(5).plot(kind="barh", stacked=True)
```


![stacked-bar-horizontal](/pictures/python/plotting-with-pandas-and-matplotlib/stacked-bar-horizontal.PNG "stacked bar horizontal")



## Exercise


```
billboard = pd.read_csv("DataAnalysis/data/billboard_charts.csv", parse_dates=["date"])

plt.style.use("ggplot")

plt.figure(figsize=(8,6))
billboard[billboard["rank"] == 1]["artist"].value_counts().head(10).plot(kind="barh")
plt.title("Top 10 Number 1 Artists")
plt.xlabel("Weeks at #1")
```


![horizontal-bar-top-artists](/pictures/python/plotting-with-pandas-and-matplotlib/horizontal-bar-top-artists.PNG "horizontal bar for top artists")


- To add a black edge to the bars by using **edgecolor** and **linewidth** parameters:


```
plt.figure(figsize=(8,6))
billboard[billboard["rank"] == 1]["artist"].value_counts().head(10).plot(kind="barh", edgecolor="black", linewidth=3)
plt.title("Top 10 Number 1 Artists")
plt.xlabel("Weeks at #1")
```


![edgecolor-and-linewidth-parameters](/pictures/python/plotting-with-pandas-and-matplotlib/edgecolor-parameter-and-linewidth.PNG "edgecolor and linewidth parameters")



