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


- You can also provide tick intervals for x axis by using **xticks**.


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


- This is how you can control your x axis and y axis.


