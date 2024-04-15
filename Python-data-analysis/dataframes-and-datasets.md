### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Datasets

- Textual datasets can be stored in many different formats including:

* CSV

* JSON

* SQL

* Many others!

- By far, the most commonly used is CSV.


## Dataframe

- Dataframe is two-dimensional, size-mutable, potentially hetegeneous tabular data structure with labeled axes.


## Read CSV files

- We will try to read a states.csv file in our Jupyter Notebook. I already downloaded the data folder that the instructor shared. Import pandas and run read_csv(file location) to read the file.


```
import pandas as pd
pd.read_csv("DataAnalysis/data/states.csv")
```


![read-csv-using-pandas](/pictures/python/dataframes/read-csv.PNG "read csv using pandas")


```
pd.read_csv("DataAnalysis/data/kc_house_data.csv")
```


![read-another-csv](/pictures/python/dataframes/read-another-csv.PNG "read another csv")


- We can save the file into a variable and use that variable to show that data.


```
houses = pd.read_csv("DataAnalysis/data/kc_house_data.csv")
houses
```


![csv-variable](/pictures/python/dataframes/csv-variable.PNG "csv variable")


- And the houses is a type of dataframe.


![type-dataframe](/pictures/python/dataframes/dataframe-type.PNG "dataframe type")



- If we run the below, we will see all the columns in the 'houses' dataframe:


```
houses.columns
```


![dataframe-columns](/pictures/python/dataframes/dataframe-columns.PNG "dataframe columns")


- If we run the below, we get the **number of rows** in the table:


```
len(houses)
```

![len-houses](/pictures/python/dataframes/len-houses.PNG "len houses")


- When we run **dataframe.shape**, this is gonna give us the number of rows and number of columns.


```
houses.shape
```


![dataframe-shape](/pictures/python/dataframes/dataframe-shape.PNG "dataframe shape")


- When we run **dataframe.size**, then it will give us the number of rows **times** the number of columns.


```
houses.size
```

- If we want the dataframe to be shown with at least 15 rows, you can run:


```
pd.options.display.min_rows = 15
houses
```


![display-min-rows](/pictures/python/dataframes/display-min-rows.PNG "display min rows")


- If we want to make a **new dataframe** that contains the first 5 rows of the houses dataframe, run this:


```
houses.head()
```


![head-dataframe](/pictures/python/dataframes/head-dataframe.PNG "dataframe head")


- When you save that new dataframe to a variable and run the type, it will give you "dataframe":


```
first_5 = houses.head()

type(first_5)
```

![saved-variable](/pictures/python/dataframes/saved-variable.PNG "saved variable")


- The first 5 rows is the dafault value, but when you put 20 in the head method, then it will give you the first 20 rows of the table.


![first-20](/pictures/python/dataframes/first_20.PNG "first 20")


- If we want to get the last 9 rows of the table, then run this:


```
houses.tail(9)
```

![dataframe-tail](/pictures/python/dataframes/dataframe-tail.PNG "dataframe tail")






