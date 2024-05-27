### [Source of this study material : Python Data Analysis and Visualization Master Class by Colt Steele](https://www.udemy.com/course/python-data-analysis-visualization/)


## Working with Dates and Times

- In Pandas, to convert a sting into a date time, we use to_datetime() method.


```
pd.to_datetime('2024-05-26')
```


![use-to-datetime](/pictures/python/working-with-dates-and-times/use_to_datetime.PNG "use to_datetime")


- In the US, the date format is usually MM / DD / YY, which means the Month comes first followed by the Day, and the Year comes last.


![us-time-standard](/pictures/python/working-with-dates-and-times/us-time-standard.PNG "us time standard")


- To tell the Python that we want the day to come first, then we use this parameter **dayfirst=True**.


![dayfirst-true](/pictures/python/working-with-dates-and-times/dayfirst-true.PNG "dayfirst = True")


- You can tell that the year comes first as well by using the parameter **yearfirst=True**.


![yearfirst-and-dayfirst](/pictures/python/working-with-dates-and-times/yearfirst-and-dayfirst.PNG "yearfirst and dayfirst")


- Beside that, you can also use **format** parameter to tell Pandas to format your string to a particular form of date.


```
%m : Month as a zero-padded number. (01, 02, ... 12)

%M : Minute as a zero-padded number. (01, 02, ... 59)

%Y : Year without century as a zero padded number. (00, 01, ... 99)

%y : Year with century as decimal number. (0001, 0002, 2023, 2024)

%b : Month as abbreviated name. (Jan, Feb, ... Dec)

%B : Month as full name. (January, February, ... December)

%a : Weekday as abbreviated name. (Sun, Mon, ... Sat)

%A : Weekday as full name. (Sunday, Monday, ... Saturday)
```

- So, another way I can tell Pandas to parse a certain date specifically is to use a **format string**. 


```
pd.to_datetime('24-10-11', format='%y-%m-%d')

pd.to_datetime('10-12-08', format='%m-%d-%y')
```


![format-parameter](/pictures/python/working-with-dates-and-times/format-parameter.PNG "format parameter")


- It is also useful to convert a bundch of strings into a date format in a consistent and parsable way. Imagine we have a list of dates we had a meeting with someone. To make these strings parsable, we first need to convert these into a date time like below:


```
meetings = ["Dec 11 2020 Meeting", "Jun 12 2021 Meeting", "Nov 12 2022 Meeting", "Jul 12 2023 Meeting"] 

pd.to_datetime(meetings, format="%b %d %Y Meeting")
```


![list-of-dates](/pictures/python/working-with-dates-and-times/list-of-dates.PNG "list of dates")


- Another option is to use **parse_dates=["column_name"]** parameter when importing your file using Pandas.


![parse-dates-parameter](/pictures/python/working-with-dates-and-times/parse-dates-parameter.PNG "parse dates parameter")


- If we want to look at the data by year and count the number of each year:


```
ufo["date_time"].dt.year.value_counts()
```


![dt-accessor](/pictures/python/working-with-dates-and-times/dt-accessor.PNG "dt accessor")


![top-10-value-counts](/pictures/python/working-with-dates-and-times/top-10-value-counts.PNG "top 10 value counts")


![plotting](/pictures/python/working-with-dates-and-times/plotting.PNG "plotting")


- So, in order to access individual properties of a date type column, we use **.dt** and then the property such as month, year, or day.


- If our question is "Is there a particular month when a UFO is observed more?":


![when-ufo-is-observed-more](/pictures/python/working-with-dates-and-times/month-when-ufo-is-observed.PNG "when is ufos are observed most")


- Also, find out the time when the UFOs are observed more during the day:


```
ufo["date_time"].dt.hour.value_counts().plot(kind="bar")
```


![the-hour-the-ufos-are-seen](/pictures/python/working-with-dates-and-times/the-hour-ufos-are-seen.PNG "the hour the UFOs are seen")


- You can even find out the **Day of the Week** the UFOs are seen. **0 = Monday and 6 = Sunday**.


![dt-dayofweek](/pictures/python/working-with-dates-and-times/dt-dayofweek.PNG "dt dayofweek")


- We can also provide a certain year or date time with a greater than or less than operator so that we only get a dataset that corresponds to the condition.


![using-comparison-operator](/pictures/python/working-with-dates-and-times/comparison-operator.PNG "comparison operator")


- If you want to get a dataset that corresponds to a specific time period you provide, you can use:


```
ufo[ufo["date_time"].between("2018-01-01", "2019-12-23")]
```


![time-period](/pictures/python/working-with-dates-and-times/time-period.PNG "time period")


- So far, we learned how to use **.dt** to narrow down to more specific date time properties such as day, dayofweek, month, hour, etc.


- Next, we will do some math using two date time columns. We will subtract the date_time from the posted and take that result to add a new column to the dataframe.


```
ufo["time_before_reported"] = ufo["posted"] - ufo["date_time"]
```


![time-before-reported](/pictures/python/working-with-dates-and-times/time-before-reported.PNG "time before reported")


- Then we will sort values by "time_before_reported" in descending order.


```
ufo.sort_values("time_before_reported", ascending=False)
```


![time-before-reported-ascending-false](/pictures/python/working-with-dates-and-times/sort-values-ascending-false.PNG "time before reported ascending false")


- And we call this difference between two date times as **timedelta** as a data type in Pandas.


![time-delta](/pictures/python/working-with-dates-and-times/dt-timedelta.PNG "dt timedelta")


- We will first see the top 10 longest wait before reported. 


![top-ten-longest-wait](/pictures/python/working-with-dates-and-times/top-ten-longest-wait.PNG "top ten longest wait")


- And the data type **timedelta** can have **days** properties. We will use this to delve into how many days it took them to report their observations.


[dt-days-for-timedelta](/pictures/python/working-with-dates-and-times/dt-days-for-timedelta.PNG "dt days for timedelta")



### Excercise

- Find the date when there were the most number of house sales.


```
houses["date"].value_counts()
houses["date"].mode()[0]

houses[houses["date"] == houses["date"].mode()[0]]
```

![mode-value-and-plug-into-dataframe](/pictures/python/working-with-dates-and-times/mode-value-and-plug-into-dataframe.PNG "mode value and plug that into the dataframe")



- Then we will see the distribution of years in the dataframe using a pie chart.


```
houses["date"].dt.year.value_counts().plot(kind="pie")
```


![plot-kind-pie](/pictures/python/working-with-dates-and-times/plot-kind-pie.PNG "plot kind pie")


- Next, find all the homes sold in 2014, sorted from earliest to latest.


```
houses[houses["date"].dt.year == 2014].sort_values("date", ascending=True)
```


![tweenty-fourteen](/pictures/python/working-with-dates-and-times/twenty-fourteen.PNG "twenty fourteen")


- Now, count up the number of sales that took place in each month of the year.


```
one_year = houses[houses["date"].between("2014-05-01", "2015-05-01")].sort_values("date")
one_year["date"].dt.month.value_counts()
```


![dt-month-value-counts](/pictures/python/working-with-dates-and-times/dt-month-value-counts.PNG "dt month value counts")


- I'll also plot the result by using a bar chart and also use **sort_index()** method.


```
one_year["date"].dt.month.value_counts().sort_index().plot(kind="bar")
```

![value-counts-sort-index-plot](/pictures/python/working-with-dates-and-times/value-counts-and-sort-index-plot.PNG "value counts + sort index + plot")


- **isocalendar()** function can be used to return **week number**. 


```
houses["date"].dt.isocalendar().week.value_counts().sort_index().plot()
```


![iso-calendar-week-value-counts](/pictures/python/working-with-dates-and-times/isocalendar-week-value-counts.PNG "isocalendar week value_counts")


- Create a bar plot showing the total number of sales that took place in December, January, and February.


```
houses[houses["date"].dt.month.isin([12, 1, 2])]
```

```
houses[houses["date"].dt.month.isin([12, 1, 2])]["date"].dt.month.value_counts().plot(kind="bar")
```


![monsth-isin](/pictures/python/working-with-dates-and-times/month-isin.PNG "month isin()")


- We can also use **.dt** to see the quater of the year as well.


```
one_year = houses[houses["date"].between("2014-05-01", "2015-05-01")]
one_year

waterfront = one_year[one_year.waterfront == 1]
waterfront["date"].dt.quarter
```

![dt-quarter](/pictures/python/working-with-dates-and-times/dt-quarter.PNG "dt quarter")


- Describe the result using a bar chart to show the number of waterfront home sales per quarter.


```
waterfront["date"].dt.quarter.value_counts().plot(kind="bar")
```


![quarter-value-counts](/pictures/python/working-with-dates-and-times/quarter-value-counts.PNG "quarter value counts")


