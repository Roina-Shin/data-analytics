### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Exercise


```
time_period = billboard[billboard["date"].between("2016-12-25", "2021-01-01")]


all_i_want = time_period[(time_period["song"] == "All I Want For Christmas Is You") & (time_period["artist"] == "Mariah Carey")]
rockin_around = time_period[(time_period["song"] == "Rockin' Around The Christmas Tree") & (time_period["artist"] == "Brenda Lee")]
jingle_bell = time_period[(time_period["song"] == "Jingle Bell Rock") & (time_period["artist"] == "Bobby Helms")]

all_i_want.set_index("date").sort_index()["rank"].plot(kind="line")
rockin_around.set_index("date").sort_index()["rank"].plot(kind="line")
jingle_bell.set_index("date").sort_index()["rank"].plot(kind="line")
plt.gca().invert_yaxis()
```


![set_index-and-sort_index-methods](/pictures/python/plotting-with-pandas-and-matplotlib4/set_index-and-sort_index-methods.PNG "set_index and sort_index methods")



![completed-plot](/pictures/python/plotting-with-pandas-and-matplotlib4/completed-plot.PNG "completed plot")


```
time_period = billboard[billboard["date"].between("2016-12-25", "2021-01-01")]


all_i_want = time_period[(time_period["song"] == "All I Want For Christmas Is You") & (time_period["artist"] == "Mariah Carey")]
rockin_around = time_period[(time_period["song"] == "Rockin' Around The Christmas Tree") & (time_period["artist"] == "Brenda Lee")]
jingle_bell = time_period[(time_period["song"] == "Jingle Bell Rock") & (time_period["artist"] == "Bobby Helms")]

all_i_want.set_index("date").sort_index()["rank"].plot(kind="line", label="All I want for Christmas is You")
rockin_around.set_index("date").sort_index()["rank"].plot(kind="line", label="Rockin' Around The Christmas Tree")
jingle_bell.set_index("date").sort_index()["rank"].plot(kind="line", label="Jingle Bell Rock")
plt.gca().invert_yaxis()
plt.title("Christmas Songs On The Hot 100")
plt.legend(loc="lower left")
plt.tight_layout()
```


![plt-gca-invert_yaxis-and-title-legend](/pictures/python/plotting-with-pandas-and-matplotlib4/gca_invert_yaxis_title_legend.PNG "plt gca() invert_yaxis title legend")


- Then you can use **plt.xticks()** method to change the x axis labels:


```
time_period = billboard[billboard["date"].between("2016-12-25", "2021-01-01")]


all_i_want = time_period[(time_period["song"] == "All I Want For Christmas Is You") & (time_period["artist"] == "Mariah Carey")]
rockin_around = time_period[(time_period["song"] == "Rockin' Around The Christmas Tree") & (time_period["artist"] == "Brenda Lee")]
jingle_bell = time_period[(time_period["song"] == "Jingle Bell Rock") & (time_period["artist"] == "Bobby Helms")]

all_i_want.set_index("date").sort_index()["rank"].plot(kind="line", label="All I want for Christmas is You")
rockin_around.set_index("date").sort_index()["rank"].plot(kind="line", label="Rockin' Around The Christmas Tree")
jingle_bell.set_index("date").sort_index()["rank"].plot(kind="line", label="Jingle Bell Rock")
plt.gca().invert_yaxis()
plt.xticks(["2016-12-25", "2017-12-25", "2018-12-25", "2019-12-25", "2020-12-25"],
          labels=["Xmas 2016", "Xmas 2017", "Xmas 2018", "Xmas 2019", "Xmas 2020"])
plt.title("Christmas Songs On The Hot 100")
plt.legend(loc="lower left")
plt.tight_layout()
```


![adding-xmas-labels](/pictures/python/plotting-with-pandas-and-matplotlib4/adding-xmas-labels.PNG "adding xmas labels")


![xmas-displayed-on-xaxis](/pictures/python/plotting-with-pandas-and-matplotlib4/adding-xmas-labels.PNG "xmas displayed on xaxis")



## Exercise - 2


```
john_lennon = billboard[billboard["artist"] == "John Lennon"].set_index("date")["song"].value_counts().head(8)
wings = billboard[billboard["artist"] == "Wings"].set_index("date")["song"].value_counts().head(8)
george_harrison = billboard[billboard["artist"] == "George Harrison"].set_index("date")["song"].value_counts().head(8)
ringo_starr = billboard[billboard["artist"] == "Ringo Starr"].set_index("date")["song"].value_counts().head(8)

fig, axs = plt.subplots(2,2, figsize=(14, 8))
plt.suptitle("Beatles Solo Songs (By Weeks On Chart)", fontsize=20)
john_lennon.plot(kind="barh", ax=axs[0][0], title="John Lennon")
wings.plot(kind="barh", ax=axs[0][1], title="Wings (Paul)")
george_harrison.plot(kind="barh", ax=axs[1][0], title="George Harrison")
ringo_starr.plot(kind="barh", ax=axs[1][1], title="Ringo Starr")
plt.tight_layout()
plt.legend()
```


![my-solution](/pictures/python/plotting-with-pandas-and-matplotlib4/my-solution.PNG "my solution")


![beatles-solo-songs](/pictures/python/plotting-with-pandas-and-matplotlib4/beatles-solo-songs.PNG "beatles solo songs")



## Saving a figure as a separate image file


- You can save your figure as a separate image file by using **matplotlib.pyplot.savefig()** method.


```
john_lennon = billboard[billboard["artist"] == "John Lennon"].set_index("date")["song"].value_counts().head(8)
wings = billboard[billboard["artist"] == "Wings"].set_index("date")["song"].value_counts().head(8)
george_harrison = billboard[billboard["artist"] == "George Harrison"].set_index("date")["song"].value_counts().head(8)
ringo_starr = billboard[billboard["artist"] == "Ringo Starr"].set_index("date")["song"].value_counts().head(8)

fig, axs = plt.subplots(2,2, figsize=(14, 8))
plt.suptitle("Beatles Solo Songs (By Week on Chart)", fontsize=20)
john_lennon.plot(kind="barh", ax=axs[0][0], title="John Lennon")
wings.plot(kind="barh", ax=axs[0][1], title="Wings (Paul)")
george_harrison.plot(kind="barh", ax=axs[1][0], title="George Harrison")
ringo_starr.plot(kind="barh", ax=axs[1][1], title="Ringo Starr")
plt.tight_layout()
plt.savefig("Beatles_Solo_Songs")
```


![plt-savefig](/pictures/python/plotting-with-pandas-and-matplotlib4/plt-savefig.PNG "plt savefig")


- Now, you will see that the image is saved in your folder.


![image-saved](/pictures/python/plotting-with-pandas-and-matplotlib4/image-saved.PNG "image saved")


