## Interviews

- Samantha interviews many candidates from different colleges using coding challenges and contests. Write a query to print the contest_id, hacker_id, name, and the sums of total_submissions, total_accepted_submissions, total_views, and total_unique_views for each contest sorted by contest_id. Exclude the contest from the result if all four sums are 0.


- Solution:


```
select c.contest_id, c.hacker_id, c.name,
    sum(total_submissions) as sum_total_submissions, 
    sum(total_accepted_submissions) as sum_total_accepted_submissions, 
    sum(total_views) as sum_total_views, 
    sum(total_unique_views) as sum_total_unique_views
from Contests c left join Colleges cg on c.contest_id = cg.contest_id
    left join Challenges ch on ch.college_id = cg.college_id
    left join (
    select challenge_id, sum(total_views) as total_views, sum(total_unique_views) as total_unique_views
    from View_Stats
    group by challenge_id) as view_stats on ch.challenge_id = view_stats.challenge_id
left join 
    (select challenge_id, sum(total_submissions) as total_submissions, sum(total_accepted_submissions) as total_accepted_submissions
    from Submission_Stats
    group by challenge_id) as submi_stats on ch.challenge_id = submi_stats.challenge_id
group by c.contest_id, c.hacker_id, c.name
having sum(total_submissions) + sum(total_accepted_submissions) + sum(total_views) + sum(total_unique_views) > 0
order by c.contest_id
```


![hard-question-interviews](/pictures/hackerrank-sql-coding/medium-level-challenge-two/hard-question-interviews.PNG "hard question - interviews")


## 15 days of learning SQL


- Julia conducted a 15 days of learning SQL contest. The start date of the contest was March 01, 2016 and the end date was March 15, 2016.

Write a query to print total number of unique hackers who made at least 1 submission each day (starting on the first day of the contest), and find the hacker_id and name of the hacker who made maximum number of submissions each day. If more than one such hacker has a maximum number of submissions, print the lowest hacker_id. The query should print this information for each day of the contest, sorted by the date.


- Solution:


```
select b1.submission_date, b1.counting, b2.hacker_id, h.name
from
(select submission_date, count(distinct hacker_id) as counting
from Submissions s
where s.submission_date - DATE('2016-03-01') = (
select count(distinct s2.submission_date)
from Submissions s2
where s2.submission_date < s.submission_date and
s2.hacker_id = s.hacker_id)
group by submission_date) b1
join
(select distinct submission_date, hacker_id
from Submissions s3
where hacker_id = (
select hacker_id
from Submissions s4
where s3.submission_date = s4.submission_date
group by hacker_id
order by count(submission_id) desc, hacker_id limit 1)) b2
on b1.submission_date = b2.submission_date
join
Hackers h on b2.hacker_id = h.hacker_id
order by 1
```


- It still takes me a lot of time to understand how subqueries work and this problem was exactly requiring that level of understanding. I referred to someone's query but it was worth my time reconstructing my own answer, while trying to make sense of how the subqueries are put together to make a whole jigsaw puzzle.