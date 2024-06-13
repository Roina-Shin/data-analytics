### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Apply and Map and ApplyMap

- We will look at useful Pandas methods: apply, map, and applymap.


![import-titanic-and-change-datatype](/pictures/python/apply-and-applymap/import-titanic-and-datatype.PNG "import titanic and change datatype")


- If I take the age column and use **apply() method** to pass in some function, it will apply that function to every value in that column. Let's say we want to change the age column in years into days:


```
def years_to_days(yrs):
    return yrs * 365
    
titanic["age"].apply(years_to_days)
```


![years_to_days-function](/pictures/python/apply-and-applymap/years_to_days-function.PNG "years_to_days function")


- We will create a custom function again and apply it to the age column.


```
def get_age_group(age):
    if age < 2:
        return "infant"
    elif age < 12:
        return "child"
    elif age < 19:
        return "teen"
    elif age < 60:
        return "adult"
    else: return "senior"


age_group = titanic["age"].apply(get_age_group).value_counts()
age_group_names = age_group.index.values
plt.pie(age_group, labels=age_group_names, autopct="%1.1f%%")
plt.title("Titanic Passenger Age Group", fontsize=15)
```


![apply-custom-function](/pictures/python/apply-and-applymap/apply-custom-function.PNG "apply custom function")


![review-pie-plot](/pictures/python/apply-and-applymap/review-pie-plot.PNG "review pie plot")


- Also, we don't have to write a standalone, separate function. You can pass in **lambda** if you prefer.


```
titanic["fare"].apply(lambda x: f"{x * 24}")
```

- **args** is a parameter apply() method. And if we created a custom function that takes a single or multiple parameters, we can use args parameter with a **tuple** so that we can specify values that go into it.


```
def currency_converter(num, multiplier):
    return f"${num * multiplier}"

currency_converter(2, 1000)


titanic["fare"].apply(currency_converter, args=(2,))
```


![args-parameter-for-apply](/pictures/python/apply-and-applymap/args-parameter-for-apply.PNG "args parameter for apply")



## Apply a function on a Dataframe


- If you run **apply()** method on a dataframe, the function runs on each column of that dataframe.


```
df = titanic[["pclass", "survived", "age"]]
df

def max_min_diff(s):
    return max(s) - min(s)

df.apply(max_min_diff)
```


![max_min_diff](/pictures/python/apply-and-applymap/max_min_diff.PNG "max_min_diff")


- If you add the parameter **axis=1** then you can apply a function **laterally**.


```
def get_fam_size(s):
    return s.sibsp + s.parch


titanic.apply(get_fam_size, axis=1)
```


![parameter-axis-equals-one](/pictures/python/apply-and-applymap/axis-equals-one-parameter.PNG "axis=1 parameter")



```
def get_fam_size(s):
    fam_size = s.sibsp + s.parch
    if fam_size == 0:
         return "solo"
    elif fam_size < 5:
         return "medium"
    else:
        return "large"

titanic.apply(get_fam_size, axis=1)
```


![custom-function-and-axis-parameter](/pictures/python/apply-and-applymap/custom-function-and-axis.PNG "custom function and axis paramter")


```
def get_fam_size(s):
    fam_size = s.sibsp + s.parch
    if fam_size == 0:
         return "solo"
    elif fam_size < 5:
         return "medium"
    else:
        return "large"

titanic["fam_size"] = titanic.apply(get_fam_size, axis=1)

titanic["fam_size"].value_counts()

titanic.groupby(["fam_size", "sex"]).survived.mean()
```


![groupby-function](/pictures/python/apply-and-applymap/groupby-function.PNG "groupby function")


- We can use **map()** to map one set of values to another based upon a dictionary.


```
titanic["pclass"].map({1: "1st", 2: "2nd", 3: "3rd"})
```

![map-method](/pictures/python/apply-and-applymap/map-method.PNG "map method")


- We can also use **lambda** with map():


```
titanic["age"].map(lambda x: x < 19)
```


![lambda-function](/pictures/python/apply-and-applymap/lambda-function.PNG "lambda function")


- **applymap()** method will apply a function across the entire dataframe.


```
titanic[["name", "sex", "age_group"]].applymap(str.upper)
```

![applymap-method](/pictures/python/apply-and-applymap/applymap-method.PNG "applymap method")

