## Pythagorean Theorem / Euclidean Distance / Manhattan Distance

- First, let's discuss about Euclidean and Manhattan distances.

- The pythagorean theorem is the rule that says for a triangle, the square of one side plus the square of the other side is equal to the square of the hypotenuse. 


- In other words:


![pythagorean-theorem](/pictures/hackerrank-sql-coding/euclidean-and-manhattan-distance/pythagorean-theorem.PNG "pythagorean theorem")


- The practical applications of pythagorean theorem include **triangulating GPS coordinates**. 


- Now, let's consider that we have two location points like below:


![euclidean-distance](/pictures/hackerrank-sql-coding/euclidean-and-manhattan-distance/euclidean-distance.PNG "euclidean distance")


- Euclidean distance is basically calculated based on the Pythagorean Theorem. 


![euclidean-distance-using-pythagorean-theorem](/pictures/hackerrank-sql-coding/euclidean-and-manhattan-distance/euclidean-distance-using-pythagorean-theorem.PNG "pythagorean theoreem")


![euclidean-distance-formula](/pictures/hackerrank-sql-coding/euclidean-and-manhattan-distance/euclidean-distance-formula.PNG "euclidean distance formula")


- The AC squared is equal to the squared value of (x2 - x1) and the squared value of (y2 - y1). This is what Euclidean distance means.


- Then, what is Manhattan Distance?


- The basic way of calculating Manhattan Distance is by combining |(x2 - x1)| + |(y2 - y1)|. 


![manhattan-distance](/pictures/hackerrank-sql-coding/euclidean-and-manhattan-distance/manhattan-distance.PNG "manhattan distance")


- Why is it called Manhattan Distance? Because, in a developed city like New York, the city is created block-wise. 


![why-manhattan-distance](/pictures/hackerrank-sql-coding/euclidean-and-manhattan-distance/why-manhattan-distance.PNG "why manhattan distance")


- Consinder you have go from point A to point B. You cannot simply go there by following the diagonal line. Because the roads are not built that way. Instead, you have to follow the road which is shaped like a Manhattan Distance.


- Then, when can we use Euclidean Distance? When you fly from one place to another place, the route is shaped like the hypotenuse of a triangle. And that is one case where the Euclidean Distance can be used. 


## Weather Observation Station 1


- Consider p1 (a,b) and p2(c,d) to be two points on a 2D plane.

 happens to equal the minimum value in Northern Latitude (LAT_N in STATION).
 happens to equal the minimum value in Western Longitude (LONG_W in STATION).
 happens to equal the maximum value in Northern Latitude (LAT_N in STATION).
 happens to equal the maximum value in Western Longitude (LONG_W in STATION).
Query the Manhattan Distance between points  and  and round it to a scale of  decimal places.

Input Format

The STATION table is described as follows:


![weather-observation-station-one](/pictures/hackerrank-sql-coding/euclidean-and-manhattan-distance/weather-observation-station-one.PNG "weather observation station one")


- Solution:

```
select round(abs((c - a)) +  abs((d - b)),4)
from (
select min(LAT_N) as a,
    min(LONG_W) as b,
    max(LAT_N) as c,
    max(LONG_W) as d
from STATION) as points
```
