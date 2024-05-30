### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Aggregates and Groups

- For example, if we want to see the average closing price of each car maker from the carstocks dataset, we can see the mean price by separately coding for each car symbol:


```
carstocks[carstocks["Symbol"] == "RIVN"]["Close"].mean()

carstocks[carstocks["Symbol"] == "LCID"]["Close"].mean()

carstocks[carstocks["Symbol"] == "GM"]["Close"].mean()
```


![calculating-mean-by-carmaker](/pictures/python/groupby-and-aggregates/calculating-mean-by-carmaker.PNG "calculating mean by carmaker")


- But if we use **groupby()** function, we can very easily see the breakdown of the mean closing prices for each car maker.


```
carstocks.groupby("Symbol")["Close"].mean()
```

![groupby-function](/pictures/python/groupby-and-aggregates/groupby-function.PNG "groupby function")


## Exploring Groups

- Now, we will work with titanic dataset.


```
titanic = pd.read_csv("DataAnalysis/data/titanic.csv")
titanic["age"] = pd.to_numeric(titanic["age"], errors="coerce")

df = titanic[["pclass", "survived", "sex", "age"]]
```


- We can start by grouping by sex and you can use the keyword **by** as a parameter for the groupby function. Then, you can use **ngroups** method to see how many groups are there in your specified column.


```
df = titanic[["pclass", "survived", "sex", "age"]]

gbo = df.groupby(by="sex")

gbo

gbo.ngroups
```


![by-keyword-and-ngroups](/pictures/python/groupby-and-aggregates/by-keyword-and-ngroups.PNG "by keyword and ngroups")


```
gbo

gbo["age"].mean()

gbo["age"].max()
```


![split-and-apply](/pictures/python/groupby-and-aggregates/split-and-apply.PNG "split and apply")


- What we basically did here is that we first **split** the data into groups (by sex) and then **apply** a function such as mean() or max() onto the series. Then, it automatically **combines** the result back into a nice series for us. So,


```
split

apply

combine
```

![groupby-function-plot](/pictures/python/groupby-and-aggregates/groupby-apply-function-plot.PNG "groupby apply function plot")


- We will apply our knowledge to analyze different groups of titanic data. Let's first group by passenger class and then look at the average age of each passenger group.


```
titanic.groupby("pclass")["age"].mean()
```


![groupby-pclass](/pictures/python/groupby-and-aggregates/groupby-pclass.PNG "groupby pclass")


```
class_dict = {1: "1st class", 2: "2nd class", 3: "3rd class"}
class_age = titanic.groupby("pclass")["age"].mean().rename(class_dict)
class_age.plot(kind="barh")
```


![class-dict-rename-method](/pictures/python/groupby-and-aggregates/class-dict-rename-method.PNG "class dict rename method")


## Using the Aggregate Method


- The **aggregate** method allows us to run multiple functions on groupby objects.


```
titanic.groupby("sex")["age"].aggregate("min")
```


![aggregate-method](/pictures/python/groupby-and-aggregates/aggregate-method.PNG "aggregate method")


- But the beauty of using the **aggregate()** method is the ability to pass in a list of different functions.


```
titanic.groupby("sex")["age"].aggregate(["min", "max", "mean", "median"])
```


![aggregate-to-pass-in-a-list](/pictures/python/groupby-and-aggregates/aggregate-to-pass-in-a-list.PNG "aggregate to pass in a list")


- We can add more coloums to the aggregate function and do analysis on them like below:


```
titanic.groupby("sex").aggregate({"age": ["min", "max"], "sibsp": ["max", "min"], "parch": ["max", "min"]})
```


![aggregate-on-dict](/pictures/python/groupby-and-aggregates/aggregate-on-dict.PNG "aggregate on dict")



```
carstocks.groupby("Symbol").aggregate({"Open": ["max", "min", "mean"], "Close": ["max", "min", "mean"], "Volume": ["max", "min", "mean"]})
```


![aggregate-on-dict-by-symbol](/pictures/python/groupby-and-aggregates/aggregate-on-dict-by-symbol.PNG "aggregate on dict by symbol")



## Aggregate with Custom Functions


- If we try and find the lowest and highest age per passenger class, we can run the below:


```
titanic.groupby("pclass")["age"].aggregate(["min", "max"])
```


![aggregate-on-min-and-max-by-pclass](/pictures/python/groupby-and-aggregates/aggregate-on-min-and-max-by-pclass.PNG "aggregate on min and max by pclass")


- But what if we want to get the different between the max and the min of age by passenger class? We can create a custom function at the top and call that function within the aggregate method.


```
def range(x):
    return x.max() - x.min()

titanic.groupby("pclass")["age"].aggregate(range)
```

![define-new-function](/pictures/python/groupby-and-aggregates/define-new-function.PNG "define new function")


- So, **x** in the above function we defined is actually a series of ages by passenger class.


- Now, let's assume that we want to figure out how many **NULL** values within the age column of the titanic dataset:


```
titanic["age"].count()

titanic["age"].size - titanic["age"].count()
```


![size-minus-count-equals-null-num](/pictures/python/groupby-and-aggregates/size-minus-count-equals-null-num.PNG "size minus count equals the number of null values")


- Then we can turn this into a function and apply it to our dataset:


```
def count_nulls(s):
    return s.size - s.count()

titanic.groupby("sex")["age"].aggregate(["min", "max", count_nulls])
```


![count_nulls-function-and-how-to-aggregate-on-that-function](/pictures/python/groupby-and-aggregates/count_nulls-function-and-how-to-aggregate-on-the-function.PNG "count nulls function and how to aggregate on that function")


- To have a customized column names instead of nested ones under the original column name, you can provide a customized column name and a tuple that includes the original column name and the function you want to apply to that column:


```
carstocks.groupby("Symbol").aggregate({"Open": ["min", "max"], "Close": ["min", "max"]})


carstocks.groupby("Symbol").aggregate(open_min=("Open", "min"), open_max=("Open", "max"), close_min=("Close", "min"), close_max=("Close", "max"))
```


![aggregate-on-customized-column-names-and-functions](/pictures/python/groupby-and-aggregates/aggregate-on-customized-column-names.PNG "aggregate on customized column names and apply functions on them")



## Exercise


- Find the teams that had the most red cards:


```
laliga[laliga["Red Cards"] == 1]["Team"].value_counts().head(5)
```

![team-with-the-most-red-cards](/pictures/python/groupby-and-aggregates/team-with-the-most-red-cards.PNG "team with the most red cards")


- Find the average number of "Long passes" made by each Position (Goalkeeper, Forward, etc.)


```
laliga.groupby("Position").aggregate({"Long passes": "mean"})
```


![groupby-and-aggregate](/pictures/python/groupby-and-aggregates/groupby-and-aggreate.PNG "groupby and aggregate")


- Find the 10 Shirt numbers that scored the most goals:


```
laliga.groupby("Shirt number").aggregate({"Goals scored": "sum"}).sort_values(by="Goals scored", ascending=False).head(10)
```


![groupby-shirt_number-and-aggregate-on-goals_scored-by-sum](/pictures/python/groupby-and-aggregates/groupby-shirt_number-and-aggregate-sum.PNG "groupby shirt_number and aggregate on dictionary goals_scored and sum")



- Calculate the **on target percentage** by team and provide the subplots for the **most accurate teams** and the **least accurate teams**.


```
def accuracy(s1, s2):
    return s2 / s1

fig, axs = plt.subplots(2, 1)
first_axs = accuracy(df.total, df.on_target).sort_values(ascending=False).head(5).plot(kind="barh", ax=axs[0], title="Most Accurate Teams")
accuracy(df.total, df.on_target).sort_values().head(5).plot(kind="barh", ax=axs[1], title="Least Accurate Teams", sharex=first_axs)
plt.xticks([0.0, 0.1, 0.2, 0.3, 0.4, 0.5])
plt.xlabel("On Target Percentage")
```


![use-custom-function-and-do-subplots-on-it](/pictures/python/groupby-and-aggregates/use-custom-function-and-do-subplots-on-it.PNG "use custom function and do subplots on it")



![completed-solution](/pictures/python/groupby-and-aggregates/completed-solution.PNG "completed solution")


