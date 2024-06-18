### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Changing Seaborn Themes

- How do we change the aesthetics or the appearance of Seaborn generated plots? And the first thing we are gonna cover are the **themes**.


- In Seaborn, we have 5 built-in themes:


```
# "white", "dark", "whitegrid", "darkgrid", "ticks"
```

- The simplest way to use these themes is to use **sns.set_style()** and pass in their names.


```
sns.set_style("white")
sns.scatterplot(data=tips, x="total_bill", y="tip")
```


![sns-set_style](/pictures/python/seaborn-aesthetics/sns-set_style.PNG "sns.set_style")


- The below are dark and whitegrid themes:


```
sns.set_style("dark")
sns.scatterplot(data=tips, x="total_bill", y="tip")

sns.set_style("whitegrid")
sns.scatterplot(data=tips, x="total_bill", y="tip")
```

![set_style-dark](/pictures/python/seaborn-aesthetics/set_style-dark.PNG "set_style dark")


![set_style-whitegrid](/pictures/python/seaborn-aesthetics/set_style-whitegrid.PNG "set_style whitegrid")


- If we just run **sns.axes_style()**, it will give us a dictionary with the current sytles that are applied:


```
sns.axes_style()
```

![sns_axes_style](/pictures/python/seaborn-aesthetics/sns_axes_style.PNG "sns.axes_style")


```
sns.set_style({"axes.facecolor": "bde0fe"})
sns.lineplot(data=tips, x="total_bill", y="tip")
```

![changing-axes-facecolor](/pictures/python/seaborn-aesthetics/changing-axes_facecolor.PNG "changing axes facecolor")



## Altering Spines with despine()


```
sns.set_style("white")
sns.histplot(data=tips, x="total_bill")
```

![spine-showing](/pictures/python/seaborn-aesthetics/spine-showing.PNG "spine showing")


- The default behavior of **despine()** method is to remove the top and right axes.


```
sns.set_style("white")
sns.histplot(data=tips, x="total_bill")
sns.despine()
```

![despine-method](/pictures/python/seaborn-aesthetics/despine-method.PNG "despine method")


```
sns.set_style("white")
sns.histplot(data=tips, x="total_bill")
sns.despine(bottom=True, top=False)
```

![how-to-control-the-despine-method](/pictures/python/seaborn-aesthetics/how-to-control-the-despine-method.PNG "how to control the despine method")



## Changing Color Palettes

- Seabron's existing color palettes are as below:


```
# deep, muted, bright, pastel, dark, colorblind
```


![default-palette](/pictures/python/seaborn-aesthetics/default-palette.PNG "default palette")


- If we set the color palette to **pastel**:


```
sns.set_palette("pastel")
sns.barplot(data=tips, x="day", y="tip")
```

![set-palette-pastel](/pictures/python/seaborn-aesthetics/color-palette-pastel.PNG "color palette pastel")

