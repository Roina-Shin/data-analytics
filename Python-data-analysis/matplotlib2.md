### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Creating Bar Plots

- We are gon


```
plants = ["spinach", "turnip", "rhubarb", "broccoli", "kale"]
died = [10, 25, 5, 30, 21]
germinated = [974, 88, 56, 69, 59]
```

```
plt.figure(figsize=(6,3))
plt.bar(plants, germinated)
```


![plt-bar](/pictures/python/matplotlib2/plt-bar.PNG "plt bar")


- We can change the style of the bar by using title, ylable, color, etc.


```
plt.figure(figsize=(6,3))
plt.bar(plants, germinated, width=0.5, color="pink")
plt.title("Germination Rate")
plt.ylabel("Number of seedlings")
```


![title-ylabel-color](/pictures/python/matplotlib2/title-ylabel-color.PNG "title ylabel color")


```
plt.figure(figsize=(6,3))
plt.bar(plants, germinated, width=0.5, label="germinated")
plt.bar(plants, died, width=0.5, label="died")
plt.title("Germination Rate")
plt.ylabel("Number of seedlings")
plt.legend(shadow=True, frameon=True, facecolor="white")
```


![germination-rate](/pictures/python/matplotlib2/germination-rate.PNG "germination rate")



## Creating Histograms

- A histogram is used to visualize the distribution of values in a dataset. And in Matplotlib, we get a method called **.hist**, which we can use to make a histogram.


- The simplest usage is just to pass in a list or a numpy array or a series, and it will automatically display them and do all the hard work for us.


```
nums = np.random.randn(100)
plt.hist(nums)
```


![np-random-randn](/pictures/python/matplotlib2/np-random-randn.PNG "np random randn")


- Another important parameter for a hist is the number of **bins** that we want to have displayed on a histogram. **How many chunks or ranges do we want to divide the data into?** The default value is 10.


![bins-in-hist](/pictures/python/matplotlib2/bins-in-hist.PNG "bins in hist")


- Now to be more realistic, we will use titanic dataset to apply a histogram to it.


```
titanic["age"] = pd.to_numeric(titanic["age"], errors="coerce")

plt.hist(titanic["age"])
```


![pd-to-numeric-plt-hist](/pictures/python/matplotlib2/to-numeric-plt-hist.PNG "pd to_numeric plt hist")


- Then we will the distribution of the ages of all passenges by their class.


```
first_class = titanic[titanic["pclass"] == 1]["age"]
second_class = titanic[titanic["pclass"] == 2]["age"]
third_class = titanic[titanic["pclass"] == 3]["age"]
plt.hist(first_class, label="1st class", alpha=0.5)
plt.hist(second_class, label="2nd class", alpha=0.5)
plt.hist(third_class, label="3rd class", alpha=0.5)
plt.legend()
```


![age-distribution-by-class](/pictures/python/matplotlib2/age-distribution-by-class.PNG "age distribution by class")


- To create a **stacked bar chart**, you can add **bottom=(bottom part of axes)** to the top part of axes as a parameter.


```
plt.bar(resorts, snow_last_24, label="last 24 hours")
plt.bar(resorts, snow_next_24, label="next 24 hours", bottom=snow_last_24)
plt.legend()
```


![stacked-bar-bottom-parameter](/pictures/python/matplotlib2/stacked-bar-bottom.PNG "stacked bar bottom parameter")


![snow-storm-report](/pictures/python/matplotlib2/snow-storm-report.PNG "snow storm report")




