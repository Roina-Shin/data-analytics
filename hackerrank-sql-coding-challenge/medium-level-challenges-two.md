## SQL Project Planning


- You are given a table, Projects, containing three columns: Task_ID, Start_Date and End_Date. It is guaranteed that the difference between the End_Date and the Start_Date is equal to 1 day for each row in the table.

If the End_Date of the tasks are consecutive, then they are part of the same project. Samantha is interested in finding the total number of different projects completed.

Write a query to output the start and end dates of projects listed by the number of days it took to complete the project in ascending order. If there is more than one project that have the same number of completion days, then order by the start date of the project.


```
set @num = 0;
select Start_Date, End_Date
from (
select min(Start_Date) as Start_Date, 
    max(End_Date) as End_Date,
    count(*) as num_count
from (
select Task_ID,
    Start_Date,
    End_Date,
    @num := @num + 1 as num,
    date_sub(End_Date, interval @num day) as date_group
from Projects
order by Start_Date) as base
group by date_group) as base_two
order by num_count, Start_Date
```


- This is the PostgreSQL version of solving the case. Instead of setting the @num variable, which PostgreSQL doesn't allow, you can use row_number() function and subtract that value from the date as below:


```
select start_date, end_date
from (
select min(start_date) as start_date,
	max(end_date) as end_date,
	count(*) as num_count
from (
select start_date, end_date,
		(end_date - rownum * interval '1 day')::date as date_group
from (
select task_id,
	start_date,
	end_date,
	row_number() over(order by start_date) as rownum
from public.sql_projects) as base
order by start_date) as date_group
group by date_group) as final_data
order by num_count, start_date
```