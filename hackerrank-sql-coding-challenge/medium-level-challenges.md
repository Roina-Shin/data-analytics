## Top Competitors

- Julia just finished conducting a coding contest, and she needs your help assembling the leaderboard! Write a query to print the respective hacker_id and name of hackers who achieved full scores for more than one challenge. Order your output in descending order by the total number of challenges in which the hacker earned a full score. If more than one hacker received full scores in same number of challenges, then sort them by ascending hacker_id.


- Solution:

```
select hacker_id, name
from
(select hacker_id, name,
    count(challenge_id) as num_of_challenges
    from
(
select s.submission_id, s.hacker_id, s.score, c.challenge_id, c.difficulty_level, d.score as full_score,
    h.name
from Submissions s join Challenges c on s.challenge_id = c.challenge_id
join Difficulty d on d.difficulty_level = c.difficulty_level
join Hackers h on h.hacker_id = s.hacker_id
where d.score = s.score) as base_table
group by hacker_id, name
having count(challenge_id) > 1) best_hackers
order by num_of_challenges desc, hacker_id
```


## Ollivander's Inventory


- Harry Potter and his friends are at Ollivander's with Ron, finally replacing Charlie's old broken wand.

Hermione decides the best way to choose is by determining the minimum number of gold galleons needed to buy each non-evil wand of high power and age. Write a query to print the id, age, coins_needed, and power of the wands that Ron's interested in, sorted in order of descending power. If more than one wand has same power, sort the result in order of descending age.


- Solution: 

```
select w.id, wp.age, w.coins_needed, w.power
from Wands w join Wands_Property wp on w.code = wp.code
where 
w.coins_needed = (
select min(w2.coins_needed)
from Wands w2 join Wands_Property wp2 on w2.code = wp2.code
where w.power = w2.power and wp.age = wp2.age 
) and is_evil = 0
order by w.power desc, wp.age desc
```


## Challenges


- Julia asked her students to create some coding challenges. Write a query to print the hacker_id, name, and the total number of challenges created by each student. Sort your results by the total number of challenges in descending order. If more than one student created the same number of challenges, then sort the result by hacker_id. If more than one student created the same number of challenges and the count is less than the maximum number of challenges created, then exclude those students from the result.


- Solution:

```
select hacker_id, name, challenges_created 
from (
select c.hacker_id, h.name, 
    count(challenge_id) as challenges_created
from Challenges c join Hackers h on c.hacker_id = h.hacker_id
group by c.hacker_id, h.name
having challenges_created = (
select max(num) from (
select count(challenge_id) as num
from Challenges
group by hacker_id) as derived)
or
challenges_created in (
select num from (
select count(challenge_id) as num
from Challenges
group by hacker_id) as derived_two
group by num
having count(num) = 1
)) as derived_trhee
order by 3 desc, 1
```