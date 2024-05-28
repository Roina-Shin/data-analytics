### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Intro to Matplotlib

- We are gonna take our first babystep with Matplotlib, and the first thing we need to do is to import Matplotlib.


```
import matplotlib.pyplot as plt
```

![import-matplotlib-pyplot](/pictures/python/matplotlib/import-matplotlib-pyplot.PNG "import matplotlib pyplot")


```
salaries = [55000, 65000, 72000, 90000, 115000, 150000]
ages = [20, 25, 30, 32, 40, 45]
plt.plot(salaries, ages)
```

![salaries-and-ages](/pictures/python/matplotlib/salaries-and-ages.PNG "salaries and ages")


- The default behavior of matplotlib is as shown above. It will take the first array as an X axis and the second array as a Y axis.



## Anatomy of Plots

- To do a math with a list, you can import **numpy** as np. **numpy.arange(start, stop, step)** is gonna make us an evenly spaced list of values.


```
import numpy as np
nums = np.arange(1, 20, 2)
nums
```


![import-numpy](/pictures/python/matplotlib/import-numpy.PNG "import numpy")


- But more importantly, you can do with this list a math like:


```
import numpy as np
nums = np.arange(1, 20, 2)
nums

nums * 2

plt.plot(nums, nums)
```


![do-math-with-numpy](/pictures/python/matplotlib/do-math-with-numpy.PNG "do math with numpy")


![nums-cube](/pictures/python/matplotlib/nums-cube.PNG "nums cube")


- We've got some basic Matplotlib terminology to cover. We have a **figure** which is basically a canvas, a highest level thing, in Matplotlib. 


![canvas-in-matplotlib](/pictures/python/matplotlib/canvas-in-matplotlib.PNG "canvas in matplotlib")


- An **axes** is an individual chart component of a figure, which has a label, data, border, etc. They are all grouped together in a figure. **matplotlib.pyplot.figure()** will create a new figure in Matplotlib. And anything that comes after will be added to that new figure.


```
plt.plot(nums, nums)
plt.figure()
plt.plot(nums, nums**2)
plt.figure()
plt.plot(nums, nums**3)
```


![axes-in-matplotlib](/pictures/python/matplotlib/axes-in-matplotlib.PNG "axes in matplotlib")


- And this is how we control the size and dimensions of the figure itself:


```
plt.figure(figsize=(20, 6), dpi=100)
plt.plot(nums, nums)
plt.plot(nums, nums**2)
plt.plot(nums, nums**3)
```


![figsize-and-dpi](/pictures/python/matplotlib/figsize-and-dpi.PNG "figsize and dpi")


- Next, we are gonna learn how to use some pre-built style sheets that come with Matplotlib. There are more than 26 of them currently available. To see the names of those style sheets:


```
plt.style.available
```


![plt-style-available](/pictures/python/matplotlib/plt-style-available.PNG "plt style available")


- To use one of them, you can just pass in the name of the style sheet as a parameter to the plt.style.use().


![style-sheet-fivethirtyeight](/pictures/python/matplotlib/style-sheet-fivethirtyeight.PNG "style sheet fivethirtyeight")


- We can also use other style sheet to have a different colored background, different colored lines, etc.


```
plt.style.use("seaborn-v0_8")
plt.figure(dpi=80)
plt.plot(nums, nums)
plt.plot(nums, nums**2)
plt.plot(nums, nums**3)
```


![style-sheet-seaborn](/pictures/python/matplotlib/style-sheet-seaborn.PNG "style sheet seaborn")


- We use parameters such as **color** and **linewidth** to change the style of lines in the graph like below:


```
plt.figure(dpi=80)
plt.plot(nums, nums, color = "#2b2d42", linewidth = 4)
plt.plot(nums, nums**2, color = "#d90429", linewidth = 4)
plt.plot(nums, nums**3, color = "#00a6fb", linewidth =4)
```


![color-and-linewidth](/pictures/python/matplotlib/color-and-linewidth.PNG "color and linewidth")


- We can also change the style of lines using the parameter called **linestyle**:


```
plt.figure(dpi=80)
plt.plot(nums, nums, color = "#2b2d42", linewidth = 4, linestyle="dashed")
plt.plot(nums, nums**2, color = "#d90429", linewidth = 4, linestyle="dotted")
plt.plot(nums, nums**3, color = "#00a6fb", linewidth =4, linestyle="-.")
```


![plt-plot-parameter-linestyle](/pictures/python/matplotlib/parameter-linestyle.PNG "parameter linestyle")


- Also add marker and markersize to differentiate data points in the line.


```
plt.figure(dpi=80)
plt.plot(nums, nums, color = "#2b2d42", linewidth = 2, marker = "*", markersize=10)
plt.plot(nums, nums**2, color = "#d90429", linewidth = 2, marker = "*", markersize=10)
plt.plot(nums, nums**3, color = "#00a6fb", linewidth =2, marker = "*", markersize=10)
```


![marker-and-markersize](/pictures/python/matplotlib/marker-and-markersize.PNG "marker and markersize")


- To add a title to the figure, you can use this:


```
plt.figure(dpi=80)
plt.plot(nums, nums, color = "#2b2d42", linewidth = 2, marker = "*", markersize=10)
plt.plot(nums, nums**2, color = "#d90429", linewidth = 2, marker = "*", markersize=10)
plt.plot(nums, nums**3, color = "#00a6fb", linewidth =2, marker = "*", markersize=10)
plt.title("Random Num List")
```

![plt-title](/pictures/python/matplotlib/plt-title.PNG "plt title")


- You can also provide xlabel and ylabel along with their spacing between the acutal label and the axis.


```
plt.figure(dpi=80)
plt.plot(nums, nums, color = "#2b2d42", linewidth = 2, marker = "*", markersize=10)
plt.plot(nums, nums**2, color = "#d90429", linewidth = 2, marker = "*", markersize=10)
plt.plot(nums, nums**3, color = "#00a6fb", linewidth =2, marker = "*", markersize=10)
plt.title("Random Num List", fontsize=20)
plt.xlabel("num1", labelpad=10)
plt.ylabel("num2", labelpad=10)
```


![xlabel-ylabel-labelpad](/pictures/python/matplotlib/xlabel-ylabel-labelpad.PNG "xlabel-ylabel-labelpad")


- You can also provide a tick range for x axis by using **xticks**.


```
plt.figure(figsize=(5,5))
salaries = [55000, 65000, 72000, 90000, 115000, 150000]
ages = [20, 25, 30, 32, 40, 45]
plt.plot(ages, salaries)
plt.xticks([10, 20, 30, 40, 50])
```


![salaries-and-ages-xticks](/pictures/python/matplotlib/salaries-and-ages-xticks.PNG "salaries and ages xticks")


![xticks-range](/pictures/python/matplotlib/xticks-range.PNG "xticks range")


- I can provide **yticks** along with the **labels** as well.


```
plt.figure(figsize=(5,5))
salaries = [55000, 65000, 72000, 90000, 115000, 150000]
ages = [20, 25, 30, 32, 40, 45]
plt.plot(ages, salaries)
plt.xticks([10, 20, 30, 40, 50])
plt.yticks([60000, 140000], labels=["60k", "140k"])
```


![yticks-labels](/pictures/python/matplotlib/yticks-labels.PNG "yticks-labels")


- This is how you can control and customize your x axis and y axis.


- Now, let's turn our attention to the legend. We will provide a legend to each line of our figure.


```
plt.figure(figsize=(5,5))
plt.plot(nums, label="x")
plt.plot(nums**2, label="x squared")
plt.plot(nums**3, label="x cubed")
plt.legend()
```

![plt-legend-label](/pictures/python/matplotlib/plt-legend-label.PNG "plt legend label")


- And this is how you beautify the legend using various parameters:


```
plt.plot(nums, label="x")
plt.plot(nums**2, label="x squared")
plt.plot(nums**3, label="x cubed")
plt.legend(loc="center left", shadow=True, frameon=True, facecolor="white")
```


![matplotlib-legend-beautify](/pictures/python/matplotlib/matplotlib-legend-beautify.PNG "matplotlib legend beautify")



## Exercise

```
plt.style.use("fivethirtyeight")
avg_sp500_close = [
    1427.22,
    1192.57,
    993.93,
    965.23,
    1130.65,
    1207.23,
    1310.46,
    1477.18,
    1220.04,
    948.05,
    1139.97,
    1267.64,
    1379.61,
    1643.80,
    1931.38,
    2061.07,
    2094.65,
    2449.08,
    2746.21,
    2913.36,
    3217.86,
    4246.11
]

avg_nasdaq_close = [
    3783.67,
    2029.61,
    1539.73,
    1647.17,
    1986.53,
    2099.32,
    2263.41,
    2578.47,
    2161.68,
    1845.39,
    2349.89,
    2677.44,
    2965.74,
    3541.29,
    4375.10,
    4945.55,
    4987.79,
    6235.30,
    7425.96,
    7940.36,
    10201.51,
    14294.87
]

years = [2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017,2018,2019,2020,2021]
```


```
plt.figure(figsize=(3,3))
plt.plot(years, avg_sp500_close, color="orange", linewidth=5, label="S&P 500")
plt.plot(years, avg_nasdaq_close, color="blue", linewidth=5, label="Nasdaq")
plt.xticks([2000, 2005, 2010, 2015, 2020])
plt.yticks([2000, 4000, 6000, 8000, 10000, 12000, 14000])
plt.legend()
```


![exercise-1](/pictures/python/matplotlib/exercise-1.PNG "exercise 1")


- Also, add the title along with x-label and y-label.


```
plt.figure(figsize=(3,3))
plt.plot(years, avg_sp500_close, color="orange", linewidth=5, label="S&P 500")
plt.plot(years, avg_nasdaq_close, color="blue", linewidth=5, label="Nasdaq")
plt.xticks([2000, 2005, 2010, 2015, 2020])
plt.yticks([2000, 4000, 6000, 8000, 10000, 12000, 14000])
plt.legend()
plt.title("AVG Anuual Close")
```


![add-title](/pictures/python/matplotlib/add-title.PNG "add title")


```
plt.figure(figsize=(3,3))
plt.plot(years, avg_sp500_close, color="orange", linewidth=5, label="S&P 500")
plt.plot(years, avg_nasdaq_close, color="blue", linewidth=5, label="Nasdaq")
plt.xticks([2000, 2005, 2010, 2015, 2020])
plt.yticks([2000, 4000, 6000, 8000, 10000, 12000, 14000])
plt.legend()
plt.title("AVG Anuual Close")
plt.xlabel("Year")
plt.ylabel("Price")
```


![xlabel-and-ylabel-exercise](/pictures/python/matplotlib/xlabel-and-ylabel-exercise.PNG "xlabel and ylabel exercise")




