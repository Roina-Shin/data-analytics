### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Working with text


- First, I'm going to replace "?" with None values using replace() method.

```
titanic["cabin"] = titanic["cabin"].replace(["?"], [None]).astype('object')

titanic["home.dest"] = titanic["home.dest"].replace(["?"], [None]).astype('object')
```


![replace-with-none-values](/pictures/python/working-with-text/replace-with-none-values.PNG "replace with none values")


- We have many object data columns in this dataframe, but to access those columns as a **string data type**, we will need to use **str() method**.


```
titanic["name"].str[:5]

titanic["name"].str.lower()
```


![str-method](/pictures/python/working-with-text/str-method.PNG "str method")


![str-lower](/pictures/python/working-with-text/str-lower.PNG "str lower")


```
titanic["deck"] = titanic["cabin"].str[0]

titanic.groupby("deck").aggregate({"survived": "sum", "fare": "mean"})
```


![survived-by-deck](/pictures/python/working-with-text/ "survived by deck")



## strip() method


- **strip() method** lets you eliminate any trailing whitespace or unnecessary characters from your string data.


```
titanic["name"].str.strip()
```


![str-strip-method](/pictures/python/working-with-text/str-strip-method.PNG "str strip method")


- **str.rstrip()** eliminates by default any trailing whitespace at the end of the string and **str.lstrip()** removes whitespace at the beginning of the string.


```
titanic["name"].str.rstrip()

titanic["name"].str.lstrip()
```


![rstrip-and-lstrip](/pictures/python/working-with-text/rstrip-and-lstrip.PNG "rstrip and lstrip")



## Splitting text values with split()


- You can pass in a character you want to split on with your string data.


```
titanic["home.dest"].str.split("/")
```


![pass-in-character-you-want-to-split-on](/pictures/python/working-with-text/pass-in-character-for-split-method.PNG "pass in a character you want to split on")


- As a result, we get a series consisting of lists of split texts. But if prefer to actually get separate columns for home and destination, we can use a parameter called **expand=True**.


```
titanic["home.dest"].str.split("/", expand=True)
```


![expand-true-parameter](/pictures/python/working-with-text/expand-true-parameter.PNG "expand=True parameter")


- And you can save it to a variable:


```
titanic["home"] = titanic["home.dest"].str.split("/", expand=True)[0]
```

![save-to-a-variable](/pictures/python/working-with-text/ "save to a variable")


- If you want to **limit the number of times a split happens**, you can use **n=?** parameter. At most, I want you to split one time:


```
titanic["home.dest"].str.split("/", expand=True, n=1)
```

![n-parameter](/pictures/python/working-with-text/n-parameter.PNG "n parameter")



## Testing strings with contains()


- Let's assume that we want to find all the strings that contains "hour" in a column.


```
ufos["duration"].str.contains("hour")
```


![str-contains](/pictures/python/working-with-text/str-contains.PNG "str contains")


- But the problem is that the dataset it returns could contain NA values. Fortunately for us, Pandas allows us to pass in a parameter called **na=False**. So, whenever we encounter NA values, it will set them to False, thus returning back to us a perfect **boolean series**.


```
ufos["duration"].str.contains("hour", na=False)
```

![na-equals-false-parameter](/pictures/python/working-with-text/na-equals-false-parameter.PNG "na=False parameter")


- You can even use regular expressions such as "day|week|month" in conjunction with the contains() method. The pipe character (|) means OR.


```
ufos[ufos["duration"].str.contains("day|week|month", na=False)]
```


![dataset-that-contains-either-day-or-week-or-month](/pictures/python/working-with-text/using-regex-with-contains-method.PNG "using regex with contains() method")


