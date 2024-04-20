## Coding Test Prep -1

```
CREATE TABLE emp (id,name,salary,manager)
AS
  VALUES
    ( 1  , 'james' ,  10000 , null ),
    ( 2  , 'alex'  ,   5000 , 1 ),
    ( 3  , 'Alice' ,   4500 , 1 ),
    ( 4  , 'Jone'  ,   3000 , 3 ),
    ( 5  , 'Omar'  ,   2200 , 2 );
```

Anyone who is a manager of your manager is also your indirect manager so Omar has Alex and James as indirect managers. I need a query that will get me the first indirect manager who has twice salary or more than the employee.

So result should be:


```
ID | Manager
1  | null
2  | 1
3  | 1
4  | 1
5  | 2
```


Solution:

```
with main_recur AS (
  SELECT
    ID,
    boss_id,
    salary,
    'False' as found
  FROM
    employees
  UNION
  ALL
  SELECT
    C.ID,
    case
      when T.salary >= (C.salary * 2) then T.ID
      else T.boss_id
    end as boss_id,
    C.salary,
    case
      when T.salary >= (C.salary * 2) then 'True'
      else 'False'
    end as found
  FROM
    main_recur C
    INNER JOIN employees T ON C.boss_id = T.ID
  WHERE
    C.found = 'False'
)
select
  distinct ID,
  boss_id
from
  main_recur
where
  (boss_id is null)
  or (found = 'True')
order by
  ID
```


## Coding Test Prep -2


How to get each bus, return the number of passengers boarding it, each bus should provide the unique id and number of unique passengers_on_board.


Table 1:

Buses


```
id  ORIGIN  DESTINATION time
10  Warsaw  Berlin  10:55
20  Berlin  Paris   6:20
21  Berlin  Paris   14:00
22  Berlin  Paris   21:40
30  Paris   Madrid  13:30
```


Table 2:

Passenger


```
id  ORIGIN  DESTINATION time
1   Paris   Madrid  13:30
2   Paris   Madrid  13:31
10  Warsaw  Paris   10:00
11  Warsaw  Berlin  22:31
40  Berlin  Paris   6:15
41  Berlin  Paris   6:50
42  Berlin  Paris   7:12
43  Berlin  Paris   12:03
44  Berlin  Paris   20:00
```


Desirable outcome


```
id  count
10  0
20  1
21  3
22  1
30  1
```

Solution


```
select   b.id
        ,count(p.id)
from     (
          select *
                ,lag(time) over(partition by origin, destination order by time) as pre_bus_time
          from   Buses
         ) b left join Passenger p on p.origin  = b.origin  and p.destination = b.destination and p.time <= b.time and p.time >= coalesce(b.pre_bus_time, '00:00:00')
group by b.id
order by b.id
```


## Coding Test Prep -3


How to get max scoring player from each group using SQL?

I have tables players and matches and I want to find players with max points, note that player with lower id is winner in each group if scores matches.


```
create table players (
      player_id integer not null unique,
      group_id integer not null
  );

  create table matches (
      match_id integer not null unique,
      first_player integer not null,
      second_player integer not null,
      first_score integer not null,
      second_score integer not null
  );



insert into players values(20, 2);
insert into players values(30, 1);
insert into players values(40, 3);
insert into players values(45, 1);
insert into players values(50, 2);
insert into players values(65, 1);
insert into matches values(1, 30, 45, 10, 12);
insert into matches values(2, 20, 50, 5, 5);
insert into matches values(13, 65, 45, 10, 10);
insert into matches values(5, 30, 65, 3, 15);
insert into matches values(42, 45, 65, 8, 4);
```


Now I want result as

Note that first and second player can be same from group.

Result:


```
group_id | winner_id
  ----------+-----------
   1        | 45
   2        | 20
   3        | 40
```


Solution:

Use row_number():


```
select group_id, player_id
from (
    select
        p.*,
        row_number() over(
            partition by p.group_id 
            order by case 
                when m.first_player = p.player_id then m.first_score 
                else m.second_score 
            end desc,
            player_id
        ) rn
    from players p
    inner join matches m
        on m.first_player = p.player_id or m.second_player = p.player_id
) x
where rn = 1
```


```
with actual_data as 
(select row_number()over(partition by group_id 
order by group_id asc, 
coalesce(table2.sum_score, 0) desc) as rn, 
group_id, player_id, coalesce(table2.sum_score, 0) as score 
from  players as table1
left join 
(select player, sum(score) as sum_score from ( 
select match_id, 
case 
when first_score > second_score then first_player
when first_score = second_score and first_player < second_player 
then first_player 
else second_player end as player, 
case 
when first_score > second_score then first_score 
when first_score = second_score and first_player < second_player 
then first_score 
else second_score end as score 
from matches) as foo 
group by player 
order by sum_score desc) as table2 
on table1.player_id = table2.player 
order by group_id asc,  coalesce(table2.sum_score, 0) desc 
) 
select group_id , player_id from actual_data where rn = 1
```


## Coding Test Prep -4  


Question: For an online platform assessments were conducted for 3 topics and scores were provided.


Table: Assessments

Input:


```
|Id|experience|sql|algo|bug_fixing|
|1 |5         |100|null|100       |
|2 |1         |100|100 |null      |
|3 |3         |100|null|50        |
|4 |5         |100|null|50        |
|5 |5         |100|100 |100       |
```


We need to print experience, count of students having a perfect score(null is considered a perfect score) as max and count of students with that year of experience as counts. Results to be printed in descending order of years of experience.

Output:


```
|experience|max|counts|
|5         |2  |3     |
|3         |0  |1     |
|1         |1  |1     |
```


Solution


```
select 
    experience,
    count(*) filter (where 
        coalesce(sql, 100)+ 
        coalesce(algo, 100)+ 
        coalesce(bug_fixing, 100) = 300
    ) as max,
    count(*)
from assessments
group by experience
order by experience desc
```


## Coding Test Prep -5


I need to summarize each group of tests. The result should look like this:


```
name_of_the_group  |  all_test_cases  |  passed_test_cases  |  total_value
numerical stability          4                    4                 80
memory usage                 3                    2                 20
corner cases                 0                    0                  0
performance                  2                    0                  0
```


columns:

name_of_the_group (all groups need to be displayed even if there are no corresponding tests)
all_test_cases (number of tests in the group)
passed_test_cases (number of test cases with the status OK)
total_value (total value of passed tests in this group)



Solution:


Below SQL query provide a solution to all test cases

When there is a name entry in table test_groups but missing in test_cases.
Order by test_value desc first and then by name lexicographically increasing order.


```
SELECT CASE
           WHEN g.name = c.group_name THEN c.group_name
           ELSE g.name
       END AS name,
       count(c.group_name) AS all_test_cases,
       count(CASE
                 WHEN c.status = 'OK' THEN 1
             END) AS all_test_cases,
       sum(CASE
               WHEN c.status = 'OK' THEN g.test_value
               ELSE 0
           END) AS total_value
FROM test_groups g
LEFT JOIN test_cases c ON g.name = c.group_name
GROUP BY CASE
             WHEN g.name = c.group_name THEN c.group_name
             ELSE g.name
         END
ORDER BY total_value DESC,
         CASE
             WHEN g.name = c.group_name THEN c.group_name
             ELSE g.name
         END
```

simple conditional aggregation:


```
select
  t.name,
  count(*) as total,
  sum(case when c.status = 'OK' then 1 else 0 end) as passed,
  sum(case when c.status = 'OK' then test_value end) as score
from
test_groups t
left join test_cases c
 on t.name = c.group_name
 group by t.name
```