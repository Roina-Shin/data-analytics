### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Seaborn Histograms

- Let's work with **histplot** in Seaborn.


```
sns.histplot(data=tips, x="tip")
```


![first-histplot](/pictures/python/seaborn3/first-histplot.PNG "first histplot")


```
sns.histplot(data=tips, x="tip", hue="time")
```

![histplot-hue](/pictures/python/seaborn3/histplot-hue.PNG "histplot hue")


- And **hue** argument is very important to remember across all these different methods.


- Now, the default behavior is to **set the opacity such that we can see through** each one of these. If we actually want them to stack **on top of one another**, there's a parameter called **multiple** to stack.


```
plt.figure(figsize=(6, 3))
sns.histplot(data=tips, x="tip", hue="smoker", multiple="stack")
```

![multiple-parameter-stack](/pictures/python/seaborn3/multiple-stack.PNG "stack for multiple parameter")


- If we use **dodge** for the multiple, this puts them **side by side** instead of stacking.


```
sns.histplot(data=tips, x="tip", hue="smoker", multiple="dodge")
```


![multiple-dodge](/pictures/python/seaborn3/multiple-dodge.PNG "multiple dodge")


```
penguins = sns.load_dataset("penguins")

sns.histplot(data=penguins, x="body_mass_g", hue="sex", multiple="stack")
```

![penguin-body-mass-distribution](/pictures/python/seaborn3/distribution-of-penguins-body-mass.PNG "distribution of penguins' body mass")


- We can control the number of bins by using **bins** parameter.


```
sns.histplot(data=penguins, x="body_mass_g", hue="sex", multiple="dodge", bins=10)
```

![bins-parameter](/pictures/python/seaborn3/bins-parameter.PNG "bins parameter")


- We can also control the width of bins by using **binwidth**.


```
sns.histplot(data=penguins, x="body_mass_g", hue="sex", multiple="stack",  binwidth=230)
```

![binwidth-parameter](/pictures/python/seaborn3/binwidth-parameter.PNG "binwidth parameter")


```
sns.histplot(data=penguins, x="body_mass_g", hue="species", multiple="stack")
```


![hue-species](/pictures/python/\/seaborn3/hue-species.PNG "hue species")


- The **element="step"** argument allows us to see the overall outline of the shape instead of having these **distinct bins**.


```
sns.histplot(data=penguins, x="body_mass_g", bins=20, hue="species", multiple="stack", element="step")
```


![element-step](/pictures/python/seaborn3/element-step.PNG "element step")


- We can also add a **KDE curve (Kernel Density Estimation)** and it's another way of visualizing the distribution instead of **relying on the separate, discrete bins**. It will overlay a smooth curve on top of the histogram.


```
sns.histplot(data=tips, x="tip", kde=True)
```


![histplot-kde-curve](/pictures/python/seaborn3/histplot-kde-curve.PNG "histplot kde curve")


## KDE Plot

```
sns.kdeplot(data=penguins, x="body_mass_g")
```


![kdeplot](/pictures/python/seaborn3/kdeplot.PNG "kdeplot")


```
sns.kdeplot(data=penguins, x="body_mass_g", hue="species")
```


![kdeplot-with-hue](/pictures/python/seaborn3/kdeplot-with-hue.PNG "kdeplot with hue")


- We can also provide **bw_adjust** argument to be more precise.


```
sns.kdeplot(data=penguins, x="body_mass_g", hue="species", bw_adjust=0.4)
```


![bw_adjust-parameter](/pictures/python/seaborn3/bw_adjust-parameter.PNG "bw_adjust parameter")


```
sns.kdeplot(data=penguins, x="body_mass_g", hue="species", bw_adjust=0.3, multiple="stack")
```

![bw_adjust-and-multiple-stack](/pictures/python/seaborn3/bw_adjust-and-multiple-stack.PNG "bw_adjust and multiple=stack")


## Bivariate Distribution Plots


- So far, we've seen **univariate plots** where they have only one variable or **x axis**. And those histograms and KDE plots are all **univariate plots**.


- You can actually create what are called **bivariate histograms** or **bivariate KDE plots** where we have an X and Y axis to see the distribution of 2 separate factors.


```
sns.histplot(data=penguins, x="body_mass_g", y="flipper_length_mm")
```


![bivariate-histogram](/pictures/python/seaborn3/bivariate-histogram.PNG "bivariate histogram")


- You can kind of read this as a heat map. So, anywhere we have a darker color means that we have **a higher density**, a greater distribution of those values.


- And remember, with a **KDE plot**, whether it's univariate or bivariate, we are looking at a smoothed curve without any discrete edges. And the circles in the KDE plot roughly corresponds to the darker areas in the histogram above.


```
sns.kdeplot(data=penguins, x="body_mass_g", y="flipper_length_mm")
```

![bivariate-kdeplot](/pictures/python/seaborn3/bivariate-kdeplot.PNG "bivariate kdeplot")


```
sns.kdeplot(data=penguins, x="bill_length_mm", y="flipper_length_mm", hue="species")
```

![adding-hue-to-bivariate-kdeplot](/pictures/python/seaborn3/adding-hue-to-bivariate-kdeplot.PNG "adding hue to the bivariate KDE plot")


## Rugplots

- **rugplot()** is another method that allows to see the distribution of data **by showing a tick mark for every value**.


```
sns.rugplot(data=tips, x="tip")
```

- We can control the height of  the tick marks by using the **height** parameter. And the height is a **proportion relative to the height of the entire plot**.


```
sns.rugplot(data=tips, x="tip", height=0.4)
```


![height-parameter-for-rugplot](/pictures/python/seaborn3/height-parameter-for-rugplot.PNG "height parameter for rugplot")


- The document says that the **rugplot** is intended to complement other plots by showing the location of individual observations in an unobtrusive way. Let's take a quick example:


```
sns.kdeplot(data=tips, x="total_bill")
sns.rugplot(data=tips, x="total_bill", height=0.2)
```

![kdeplot-with-rugplot](/pictures/python/seaborn3/kdeplot-with-rugplot.PNG "kde plot with rugplot")



```
sns.scatterplot(data=tips, x="total_bill", y="tip")
sns.rugplot(data=tips, x="total_bill", y="tip")
```

![scatterplot-with-rugplot](/pictures/python/seaborn3/scatterplot-and-rugplot.PNG "scatterplot with rugplot")


## The Displot() Method

- This is our first displot. We need to specify the kind and dataset.


```
sns.displot(kind="hist", data=penguins, x="body_mass_g")
```


![first-displot](/pictures/python/seaborn3/first-displot.PNG "first displot")


- But we can directly control the **facet** like we would with the **relplot**. We can set the hegith=3 and aspect=2 if we want it to be **twice as wide as it's tall**. So, displot() is a figure-level method.


```
sns.displot(kind="hist", data=penguins, x="body_mass_g", height=3, aspect=2)
```

![control-the-height-and-aspect-in-displot](/pictures/python/seaborn3/control-height-and-aspect-in-displot.PNG "control the height and aspect in the displot")


- What's really cool about the displot is that we can add **rug=True** (rugplot) as a parameter directly to the method.


```
sns.displot(data=tips, kind="kde", x="tip", col="time", rug=True)
```

![rug-true-parameter](/pictures/python/seaborn3/rug-true-parameter.PNG "rug true parameter")


```
sns.displot(kind="hist", data=tips, x="total_bill", y="tip", rug=True, col="time", height=4, aspect=1.2)
```


![bivariate-hist-using-displot](/pictures/python/seaborn3/bivariate-hist-using-displot.PNG "bivariate hist using displot")


