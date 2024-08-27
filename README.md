# Sql4

1 Problem 1 : The Number of Seniors and Juniors to Join the Company	(https://leetcode.com/problems/the-number-of-seniors-and-juniors-to-join-the-company/)
<br>
With CTE as 
(SELECT 
*,
SUM(Salary) Over(Partition By experience Order By salary) as rsum
FROM
Candidates
)

Select 'Senior' as experience, count(*) as accepted_candidates
From CTE
Where rsum<70000 AND experience ='Senior'
Union
Select 'Junior' as experience, count(*) as accepted_candidates
From CTE
Where rsum<(Select 70000-ifnull(Max(rsum),0) From CTE Where rsum<70000 AND experience='Senior' ) AND experience ='Junior'

<br>

2 Problem 2 : League Statistics		(https://leetcode.com/problems/league-statistics/ )
<br>
select
    team_name
    , count(*) as matches_played
    , sum(case when home > away then 3 when home = away then 1 else 0 end) as points
    , sum(home) as goal_for
    , sum(away) as goal_against
    , sum(home) - sum(away) as goal_diff
    
from 
    (select home_team_id, home_team_goals as home, away_team_goals as away from matches
     union all
     select away_team_id as home_team_id, away_team_goals as home, home_team_goals as away from matches
	 ) g
join teams t on g.home_team_id = t.team_id
group by 1
order by 3 desc, 6 desc, 1

<br>

3 Problem 3 : Sales Person		(https://leetcode.com/problems/sales-person/)
<br>
select name

from SalesPerson
where name not in (select s.name as name
from SalesPerson s 
    left join Orders o on s.sales_id=o.sales_id
    left join Company c on o.com_id=c.com_id
where c.name='RED')

4 Problem 4 : Friend Requests II	(https://leetcode.com/problems/friend-requests-ii-who-has-the-most-friends/ )
<br>
With base as(select requester_id id from RequestAccepted
union all
select accepter_id id from RequestAccepted)
select id, count(*) num  from base group by 1 order by 2 desc limit 1
