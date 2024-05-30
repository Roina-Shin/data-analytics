### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Multiple Plots on the Same Axes


```
ufos[ufos["state"] == "CA"]["year"].value_counts().sort_index().plot()
```


![value-counts-sort-index](/pictures/python/plotting-with-pandas-and-matplotlib3/value-counts-sort-index.PNG "value counts sort index")


```
ufos[ufos["state"] == "CA"]["year"].value_counts().sort_index().plot()
ufos[ufos["state"] == "TX"]["year"].value_counts().sort_index().plot()
```


![plotting-multiple-plots-on-the-same-axes](/pictures/python/plotting-with-pandas-and-matplotlib3/plotting-multiple-axes.PNG "plotting multiple axes")



```
ufos[ufos["state"] == "CA"]["year"].value_counts().sort_index().plot(label="CA")
ufos[ufos["state"] == "TX"]["year"].value_counts().sort_index().plot(label="TX")
plt.legend()
```

![add-labels](/pictures/python/plotting-with-pandas-and-matplotlib3/add-labels.PNG "add labels")


- This is more **finessed** than just doing a dot plot on the entire dataframe. We're filtering it out by state at a time and then plotting each state separately.


## Exercise


- My solution is to limit years in the line plot by using **plt.xticks** method.


```
years = np.arange(2000, 2019, 2)
lights = ufos[ufos["shape"] == "light"]["year"].value_counts().sort_index(ascending=False).head(20)
circles = ufos[ufos["shape"] == "circle"]["year"].value_counts().sort_index(ascending=False).head(20)
triangles = ufos[ufos["shape"] == "triangle"]["year"].value_counts().sort_index(ascending=False).head(20)
fireballs = ufos[ufos["shape"] == "fireball"]["year"].value_counts().sort_index(ascending=False).head(20)
formations = ufos[ufos["shape"] == "formation"]["year"].value_counts().sort_index(ascending=False).head(20)
plt.plot(lights, linewidth=5)
plt.plot(circles, linewidth=5)
plt.plot(triangles, linewidth=5)
plt.plot(fireballs, linewidth=5)
plt.plot(formations, linewidth=5)
plt.xticks(years)
plt.show()
```


![line-plot](/pictures/python/plotting-with-pandas-and-matplotlib3/xticks-method-line-plot.PNG "xticks method line plot")


- I added labels and legend on the upper left side along with the title at the top center.


```
years = np.arange(2000, 2019, 2)
lights = ufos[ufos["shape"] == "light"]["year"].value_counts().sort_index(ascending=False).head(20)
circles = ufos[ufos["shape"] == "circle"]["year"].value_counts().sort_index(ascending=False).head(20)
triangles = ufos[ufos["shape"] == "triangle"]["year"].value_counts().sort_index(ascending=False).head(20)
fireballs = ufos[ufos["shape"] == "fireball"]["year"].value_counts().sort_index(ascending=False).head(20)
formations = ufos[ufos["shape"] == "formation"]["year"].value_counts().sort_index(ascending=False).head(20)
plt.plot(lights, linewidth=5, label="light")
plt.plot(circles, linewidth=5, label="circle")
plt.plot(triangles, linewidth=5, label="triangle")
plt.plot(fireballs, linewidth=5, label="fireball")
plt.plot(formations, linewidth=5, label="formation")
plt.xticks(years)
plt.legend(loc="upper left")
plt.title("UFO Sightings by Shape")
plt.show()
```


![ufo-sightings-by-shape](/pictures/python/plotting-with-pandas-and-matplotlib3/ufo-signtings-by-shape.PNG "UFO Sightings by Shape")


![by-shape-completed](/pictures/python/plotting-with-pandas-and-matplotlib3/by-shape-completed.PNG "by shaping completed")



## Exercise - 2 


- To invert the Y axis of the axes you are currently working on:


```
plt.gca().invert_yaxis()
```


```
plt.figure(figsize=(9,5))
billboard[billboard["song"] == "Blinding Lights"].set_index("date")["rank"].plot()
plt.gca().invert_yaxis()
```


![get-current-axes-gca-invert_yaxis](/pictures/python/plotting-with-pandas-and-matplotlib3/gca-invert-yaxis.PNG "gca invert yaxis")


![blinding-lights-performance-completed](/pictures/python/plotting-with-pandas-and-matplotlib3/blinding-light-performance-completed.PNG "blinding lights performance completed")



## Pandas Automatic Subplots


- Pandas supports subplots right out of the box with **pandas.Dataframe.plot()** method.


```
df.plot(kind="hist", subplots=True)
```


![pandas-plot-subplots-true](/pictures/python/plotting-with-pandas-and-matplotlib3/pandas-plot-subplots-true.PNG "pandas plot subplots=True")


- If you think that it is a little bit **cramped**, you can call **plt.tight_layout()** to space things out a bit.


```
df.plot(kind="hist", subplots=True)
plt.tight_layout()
```

![plt-tight_layout](/pictures/python/plotting-with-pandas-and-matplotlib3/plt-tight_layout.PNG "plt tight layout")


- In order to get a more granular control over each plot on the axes, you can use **set_xlim()** or **set_title()** methods:


```
axs = df.plot(kind="hist", subplots=True, sharex=False, layout=(1,3), figsize=(10,3), bins=20)
plt.tight_layout()
axs[0][0].set_xlim(0, 30000)
axs[0][1].set_xlim(0, 20000)
axs[0][2].set_xlim(0, 20000)
axs[0][0].set_title("Base Pay")
axs[0][1].set_title("Overtime Pay")
axs[0][2].set_title("Other Pay")
```


![set_xlim-and-set_title](/pictures/python/plotting-with-pandas-and-matplotlib3/set-xlim-and-set-title.PNG "set_xlim and set_title")



## Manual Subplots with Pandas


- You can specify the layout you want by using **plt.subplots()** and stroing the value to the **figure(fig)** and **axes (axs)** objects. Then use the parameter **ax** to specify the particular axes you want to plot on.


```
fig, axs = plt.subplots(1,2)
ufos["month"].value_counts().sort_index().plot(kind="bar", ax=axs[0])
ufos["year"].value_counts().sort_index().plot(ax=axs[1])
```

![two-subplots-in-the-same-figure](/pictures/python/plotting-with-pandas-and-matplotlib3/two-subplots-in-the-same-figure.PNG "two subplots in the same figure")



```
fig, axs = plt.subplots(1,2, figsize=(10,3))
ufos["month"].value_counts().sort_index().plot(kind="bar", ax=axs[0])
ufos["year"].value_counts().sort_index().plot(ax=axs[1])
axs[0].set_title("UFO Sightings by Month")
axs[1].set_title("UFO Sightings by Year")
```


![axs-set-title](/pictures/python/plotting-with-pandas-and-matplotlib3/axs-set-titles.PNG "axs set title")



```
months = {1: "Jan", 2: "Feb", 3: "Mar", 4: "Apr", 5: "May", 6: "Jun", 7: "Jul", 8: "Aug", 9: "Sep", 10: "Oct", 11: "Nov", 12: "Dec"}
fig, axs = plt.subplots(2, 3, figsize=(9, 6))
plt.suptitle("UFO Sightings by Months (2014-2019)")
ufos[ufos["year"] == 2014]["month"].value_counts().sort_index().rename(months).plot(kind="bar", ax=axs[0][0], title="2014")
ufos[ufos["year"] == 2015]["month"].value_counts().sort_index().rename(months).plot(kind="bar", ax=axs[0][1], title="2015")
ufos[ufos["year"] == 2016]["month"].value_counts().sort_index().rename(months).plot(kind="bar", ax=axs[0][2], title="2016")
ufos[ufos["year"] == 2017]["month"].value_counts().sort_index().rename(months).plot(kind="bar", ax=axs[1][0], title="2017")
ufos[ufos["year"] == 2018]["month"].value_counts().sort_index().rename(months).plot(kind="bar", ax=axs[1][1], title="2018")
ufos[ufos["year"] == 2019]["month"].value_counts().sort_index().rename(months).plot(kind="bar", ax=axs[1][2], title="2019")
plt.tight_layout()
```


![rename-numerical-months-to-month-names](/pictures/python/plotting-with-pandas-and-matplotlib3/rename-numerical-months-to-month-name.PNG "rename numerical months to month names")


![plot-on-a-particular-axes](/pictures/python/plotting-with-pandas-and-matplotlib3/plot-on-a-particular-axes.PNG "plot on a particular axes")