## Placements

- You are given three tables: Students, Friends and Packages. Students contains two columns: ID and Name. Friends contains two columns: ID and Friend_ID (ID of the ONLY best friend). Packages contains two columns: ID and Salary (offered salary in $ thousands per month).


- Write a query to output the names of those students whose best friends got offered a higher salary than them. Names must be ordered by the salary amount offered to the best friends. It is guaranteed that no two students got same salary offer.


- Solution:


```
select one.Name
from
(select s.ID, s.Name,
    f.Friend_ID, p.Salary
from Students s join Friends f on s.ID = f.ID
join Packages p on p.ID = s.ID) one
join
(select s.ID, s.Name,
 f.Friend_ID, p.Salary
from Students s join Friends f on s.ID = f.ID
join Packages p on p.ID = f.Friend_ID
) two on one.ID = two.ID
where one.Salary < two.Salary
order by two.Salary
```


![joins-problem](/pictures/hackerrank-sql-coding/medium-level-challenge-two/joins-problem.PNG "joins problem")



## Symmetric Pairs

- You are given a table, Functions, containing two columns: X and Y.


- Two pairs (X1, Y1) and (X2, Y2) are said to be symmetric pairs if X1 = Y2 and X2 = Y1.

Write a query to output all such symmetric pairs in ascending order by the value of X. List the rows such that X1 ≤ Y1.


```
select X, Y from
((select f.X, f.Y
from Functions f, Functions f2
where f.X = f2.Y and f.Y = f2.X and
(f.X, f.Y) not in 
(select X, Y
from Functions
group by X, Y
having X = Y and count(*) = 1)
and (f.X, f.Y) not in
(select X, Y
 from Functions
 group by X, Y
 having X = Y and count(*) > 1
)
and f.X <= f.Y
order by f.X)
union
(select X, Y
 from Functions
 group by X, Y
 having X = Y and count(*) > 1)) as base
 order by X
```


![symmetric-pairs](/pictures/hackerrank-sql-coding/medium-level-challenge-two/symmetric-pairs.PNG "symmetric pairs")



## Print Prime Numbers

- Write a query to print all prime numbers less than or equal to 1000. Print your result on a single line, and use the ampersand (&) character as your separator (instead of a space).

For example, the output for all prime numbers <= 10> would be:


```
2&3&5&7
```


- Solution:


```
with recursive numbers as (
select 2 as num
union all
select num + 1 from numbers
where num <= 1000)

select group_concat(n.num SEPARATOR '&')
from numbers n
where n.num not in (
select n.num
from numbers n2
where mod(n.num, n2.num) = 0 and
    SQRT(n.num) >= n2.num)
```


![print-prime-numbers](/pictures/hackerrank-sql-coding/medium-level-challenge-two/print-prime-numbers.PNG "print prime numbers")


- Here's the PostgreSQL version of my solution:


```
with recursive numbers as (
	select 2 as num
	union all
	select num + 1 from numbers
	where num < 1000
)

select string_agg(num::text, '&')
from numbers n
where num not in
(
	select n.num
	from numbers n2
	where
	mod(n.num, n2.num) = 0
	and sqrt(n.num) >= n2.num
)
```


![print-prime-numbers-postgresql](/pictures/hackerrank-sql-coding/medium-level-challenge-two/print-prime-numbers-postgresql.PNG "print prime numbers postgresql")