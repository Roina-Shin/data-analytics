### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Creating Scatter Plots

- In a scatter plot, you are plotting one measure against another measure.


```
heights = [137,140,142,145,147,150,152,155,157,160]
f_weights = [28.5,30.8,32.6,34.9,36.4,39,40.8,43.1,44.9,47.2]
m_weights = [34.9,38.1,33.5,35.8,46.7, 42.8,43.1,45.8,50.8,58.9]
```

```
plt.scatter(heights, f_weights)
```


![scatter-plot](/pictures/python/matplotlib2/scatter-plot.PNG "scatter plot")


- If I do another scatter plot:


```
plt.scatter(heights, f_weights)
plt.scatter(heights, m_weights)
```


![another-scatter-plot](/pictures/python/matplotlib2/another-scatter-plot.PNG "another scatter plot")


- You can add legend to the scatter by adding label to each axes.


```
plt.scatter(heights, f_weights, label="Female")
plt.scatter(heights, m_weights, label="Male")
plt.legend()
```

![add-legend-to-scatter](/pictures/python/matplotlib2/add-legend-to-scatter.PNG "add legend to scatter")


- Then, add xlabel and ylabel to the scatter plot.


```
plt.scatter(heights, f_weights, label="Female")
plt.scatter(heights, m_weights, label="Male")
plt.legend()
plt.xlabel("height")
plt.ylabel("weight")
```

![xlabel-and-ylabel](/pictures/python/matplotlib2/xlabel-and-ylabel.PNG "xlabel and ylabel")



## Creating Pie Charts

- For a pie chart, the simplest usage is just to pass in a list of numbers to figure out **the size of each wedge**.


```
labels = ["Turkey", "Potatoes", "Pumpkin Pie", "Stuffing"]
prices = [25.99,3.24,9.50, 6.99]
```

- You can add parameter **labels** to add labels to a pie chart.


![parameter-labels-pie](/pictures/python/matplotlib2/pie-parameter-labels.PNG "pie parameter labels")


- Next, let's add **autopct**, to label the wedges with their numeric value.


```
plt.figure(figsize=(4,4))
plt.pie(prices, labels=labels, autopct="%1.1f%%")
```


![autopct-format-string](/pictures/python/matplotlib2/autopct-format-string.PNG "autopct-format-string")


- To elimiate the explanation after your code, you can use **plt.show()** to explicitly tell Matplotlib to only show the output.


```
plt.figure(figsize=(4,4))
plt.pie(prices, labels=labels, autopct="%1.1f%%")
plt.show()
```


![plt-show](/pictures/python/matplotlib2/plt-show.PNG "plt show")


- You can also have a slice exploded out of a pie so that we can focus on that one piece away from the rest of the pie chart.


- So if I want to **explode a pumpkin pie** for example, I can add the **explode** parameter to my pie plot. And for this, the order matters as the offset values are decided as the order of the elements of the list.


```
labels = ["Turkey", "Potatoes", "Pumpkin Pie", "Stuffing"]
prices = [25.99,3.24,9.50, 6.99]

plt.pie(prices, labels=labels, autopct="%1.1f%%", explode=(0, 0, 1, 0))
plt.show()
```


![big-explode](/pictures/python/matplotlib2/big-explode.PNG "big explode")


- We've exploded that particular piece off and it gets the focus of our eyes when we are looking at this pie chart.


```
plt.pie(prices, labels=labels, autopct="%1.1f%%", explode=(0,0,0.1,0))
plt.show()
```


![explode-one-slice](/pictures/python/matplotlib2/explode-one-slice.PNG "explode one slice off")



## Exercise


```
labels = [
    "Building Construction", 
    "Deferred Maintenance",
    "General Contractor Fees",
    "Owner Costs",
    "Site Construction"
]
costs = [
    47945975,
    3753584,
    7340612,
    18247137,
    5766685
]


plt.pie(costs, labels=labels, autopct="%1.f%%", explode=(0,0.1,0,0,0.1))
plt.title("School District Breakdown")
plt.show()
```

![exercise-pie](/pictures/python/matplotlib2/exercise-pie.PNG "exercise pie")



## Working with Subplots

- A subplot is how we can create layouts of multiple plots within a single figure. So, we can have multiple axes within a figure.


- So, how do we create a separate axes? There are multiple ways of accomplishing it. One way is to use a method called **subplot**.


- Now, a subplot is going to expect us to pass in a number of rows, a number of columns, and an index for a particular plot that we are working on at the moment.


```
plt.figure(figsize=(10,4))
plt.subplot(1,3,1)
plt.plot(nums)
```

- To use subplot: **subplot(the number of rows, the number of columns, the number of index of the current plot)**


- So it's one row, and 3 columns, the first plot we are working on: **plt.subplot(1, 3, 1)**. So, we have now a plot taking up one third of the figure.


![plt-subplot](/pictures/python/matplotlib2/plt-subplot.PNG "plt subplot")


```
plt.figure(figsize=(10,4))
plt.subplot(1,3,1)
plt.plot(nums)
plt.subplot(1,3,2)
plt.plot(nums**2)
plt.subplot(1,3,3)
plt.plot(nums**3)
```

![creating-three-plots-within-a-single-figure](/pictures/python/matplotlib2/three-plots-within-a-single-figure.PNG "three plots within a single figure")


- You can even give each plot a title by:


```
plt.figure(figsize=(7,3))
plt.subplot(1,3,1)
plt.plot(nums)
plt.title("nums1")
plt.subplot(1,3,2)
plt.plot(nums**2)
plt.title("nums2")
plt.subplot(1,3,3)
plt.plot(nums**3)
plt.title("nums3")
```

![give-title-to-each-plot](/pictures/python/matplotlib2/give-title-to-each-plot.PNG "give title to each plot")


- And to give title to the entire figure, we use **plt.suptitle** method.


```
plt.figure(figsize=(7,3))
plt.suptitle("My First Subplot")
plt.subplot(1,3,1)
plt.plot(nums)
plt.title("nums1")
plt.subplot(1,3,2)
plt.plot(nums**2)
plt.title("nums2")
plt.subplot(1,3,3)
plt.plot(nums**3)
plt.title("nums3")
```


![suptitle-jammed](/pictures/python/matplotlib2/suptitle-jammed.PNG "suptitle jammed")


- To make sure things are not overlapping, we can use **plt.tight_layout** method.


```
plt.figure(figsize=(7,3))
plt.suptitle("My First Subplot")
plt.subplot(1,3,1)
plt.plot(nums)
plt.title("nums1")
plt.subplot(1,3,2)
plt.plot(nums**2)
plt.title("nums2")
plt.subplot(1,3,3)
plt.plot(nums**3)
plt.title("nums3")
plt.tight_layout()
```


![plt-tight_layout](/pictures/python/matplotlib2/plt-tight-layout.PNG "plt tight_layout")


```
plt.figure(figsize=(7,7))
plt.subplot(2,2,1)
plt.plot(nums)

plt.subplot(2,2,2)
plt.plot(nums**2)

plt.subplot(2,2,3)
plt.plot(nums**3)

plt.subplot(2,2,4)
plt.plot(nums**4)
```


![two-rows-two-columns-layout](/pictures/python/matplotlib2/two-rows-two-columns-layout.PNG "two rows two columns layout")



## Exercise


```
first_class = titanic[titanic["pclass"] == 1]["age"]
second_class = titanic[titanic["pclass"] == 2]["age"]
third_class = titanic[titanic["pclass"] == 3]["age"]

plt.figure(figsize=(10,4))
plt.subplot(1,3,1)
plt.hist(first_class, label="1st class", color="red")
plt.title("1st class")


plt.subplot(1,3,2)
plt.hist(second_class, label="2nd class", color="yellow")
plt.title("2nd class")

plt.subplot(1,3,3)
plt.hist(third_class, label="3rd class", color="green")
plt.title("3rd class")
```


![titanic-subplot](/pictures/python/matplotlib2/titanic-subplot.PNG "titanic subplot")


![subplot-viz](/pictures/python/matplotlib2/subplot-viz.PNG "subplot viz")


- But what can be bothering you is that the axes in the same figure have different Y axis range, one being 0-50 and the other being 0-140.


- To make them share the same Y axis, we can provide a Y axis by **storing the first axes** to a variable. Let's call this variable **ax** and you can share this axes in other subplots as well like below:


```
first_class = titanic[titanic["pclass"] == 1]["age"]
second_class = titanic[titanic["pclass"] == 2]["age"]
third_class = titanic[titanic["pclass"] == 3]["age"]

plt.figure(figsize=(10,4))
ax = plt.subplot(1,3,1)
plt.hist(first_class, label="1st class", color="red")
plt.title("1st class")


plt.subplot(1,3,2, sharey=ax)
plt.hist(second_class, label="2nd class", color="yellow")
plt.title("2nd class")

plt.subplot(1,3,3, sharey=ax)
plt.hist(third_class, label="3rd class", color="green")
plt.title("3rd class")
```

![sharey-axis](/pictures/python/matplotlib2/sharey-axis.PNG "sharey axis")


![how-sharing-axis-works](/pictures/python/matplotlib2/how-sharing-axis-works.PNG "how sharing axis works")



## Exercise - 2


- Import kc_houses dataset and use its date column to get their dayofweek counts. And plot those values using a bar chart.


```
sales_by_day = houses["date"].dt.dayofweek.value_counts()
plt.bar(sales_by_day.index.values, sales_by_day)
```

![dayofweek-value-counts](/pictures/python/matplotlib2/dayofwee-value_counts.PNG "dayofweek value counts")


- To change the numerical label to the day names, you can use **plt.xticks** and use two lists as the parameters.


```
sales_by_day = houses["date"].dt.dayofweek.value_counts()
plt.bar(sales_by_day.index.values, sales_by_day)
plt.xticks([0, 1, 2, 3, 4, 5, 6], ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"])
plt.show()
```


![plt-xticks-to-change-labels](/pictures/python/matplotlib2/plt-xticks-to-change-labels.PNG "plt xticks to change labels")


```
plt.figure(figsize=(10,4))

plt.subplot(1,2,1)
sales_by_weekday = houses["date"].dt.dayofweek.value_counts()
plt.bar(sales_by_weekday.index.values, sales_by_weekday)
plt.xticks([0, 1, 2, 3, 4, 5, 6], ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"])
plt.title("Sales by Week Day")

plt.subplot(1,2,2)
sales_by_month = houses["date"].dt.month.value_counts().sort_index()
plt.plot(sales_by_month.index.values, sales_by_month)
plt.xticks([2, 4, 6, 8, 10, 12])
plt.title("Sales by Month")
```


![combined-altogether](/pictures/python/matplotlib2/combined-altogether.PNG "combined altogether")


![wrapping-up-matplotlib](/pictures/python/matplotlib2/wrapping-up-matplotlib.PNG "wrapping up matplotlib")
