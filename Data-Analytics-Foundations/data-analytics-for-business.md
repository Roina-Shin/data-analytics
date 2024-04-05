
## Types of Data Analytics

- **Descriptive**: "What has happened?"


- **Predictive**: "What might happen?"


- **Prescriptive**: "What should we do?"


## Data Types

- **Quantitative**: numerical

  - Annual Sales

  - Employee Performance Review Ratings (on the scale of 1 to 10)

  - Monthly Profitability


- **Qualitative**: descriptive

  - Summaries of written comments on Customer Cards

  - Results from Interviews Conducted by Store Managers

  - A Paragraph taken from Employee Self Evaluation


## Case Study 1


- Open Excel and perform the following to analyze small employee dataset.


```
n = COUNT(B7:B21)
range = MAX(B7:B21)-MIN(B7:B21)
Mean =SUM (B7:B21)/COUNT(B7:B21)
Median = MEDIAN(B7:B21)
Mode = MODE(B7:B21)
```


![case-study-one](/pictures/data-analytics-foundations/data-analytics-for-business/case-study-one.PNG "case study 1")


- Central tendencies include Mean, Median, and Mode. Mean is the average value. Median is the middle number in a series. The Mode is the most commonly occuring number. 


- Mean and Median are more useful when used in conjunction with other statistics.


- Now, we will exercise by applying what we just learned. We will calculate summary stats for sales in Q4. This time, when we calculate the Mode, the dataset has actually more than one Mode values. Therefore, we need to use **MODE.MULT(val1, val2...)** and hit **ctrl + shift + enter** to get multiple Mode values.


![mode.mult](/pictures/data-analytics-foundations/data-analytics-for-business/mode-mult.PNG "MODE.MULT()")



## Predictive and Prescriptive Analytics


- **Standard Deviation** is a measure of the relative spread of the data. It measures **how close to the Mean** is the overall sample. Or, are there many observations that are far from the average? **Large standard deviation** means a great deal of data lies further from the Mean. **Small standard deviation** means that the data points are close together around the Mean.



## Case Study 2


- We will calculate the values for sales that are within 68% of this months sales.


```
Average = AVERAGE(C7:C21)
Standard Deviation = STDEV.P(C7:C21)
Upper = B26+B27 or AVERAGE(C7:C21) + STDEV.P(C7:C21)
Lower = B26-B27 or AVERAGE(C7:C21) - STDEV.P(C7:C21)
```


![standard-deviation](/pictures/data-analytics-foundations/data-analytics-for-business/standard-deviation.PNG "standard deviation")


- And to find the range of 68%, you need to calculate the **empirical rule**. Determine the mean m and standard deviation s of your data. Add and subtract the standard deviation to/from the mean: [m - s, m + s] is the interval that contains around 68% of data. The empirical rule (also called the "68-95-99.7 rule") is a guideline for how data is distributed in a normal distribution.


- The Lower end of the range and Upper end of the range are respectively (AVERAGE + STDEV.P) and (AVERAGE - STDEV.P) which are within the 68% range of the dataset.


- The simple summary statistics such as Mean, Median, Mode, and Standard Deviation help you identify patterns and explore further.







