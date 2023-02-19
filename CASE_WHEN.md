## Case statements are SQL's version of an "IF this THEN that" statement. 

![image](https://user-images.githubusercontent.com/118057504/219962332-0c1f8616-c6fa-480e-a5cc-f030e83bd9ef.png)

## CASE statements comparing column values

Barcelona is considered one of the strongest teams in Spain's soccer league.

In this exercise, you will be creating a list of matches in the 2011/2012 season where Barcelona was the home team. You will do this using a CASE statement that compares the values of two columns to create a new group -- wins, losses, and ties.

In 3 steps, you will build a query that identifies a match's winner, identifies the identity of the opponent, and finally filters for Barcelona as the home team. Completing a query in this order will allow you to watch your results take shape with each new piece of information.

The matches_spain table currently contains Barcelona's matches from the 2011/2012 season, and has two key columns, hometeam_id and awayteam_id, that can be joined with the teams_spain table. However, you can only join teams_spain to one column at a time.

![image](https://user-images.githubusercontent.com/118057504/219962995-3bc8869e-6ce5-4370-8951-9e3d970ea794.png)
![image](https://user-images.githubusercontent.com/118057504/219963012-ab8c6cc3-a641-4504-ac0d-261df4180d75.png)

1
 - Select the date of the match and create a CASE statement to identify matches as home wins, home losses, or ties.
```
SELECT 
	-- Select the date of the match
	date,
	-- Identify home wins, losses, or ties
	CASE when home_goal > away_goal then 'Home win!'
        when home_goal < away_goal then 'Home loss :(' 
        ELSE 'Tie' 
		END AS outcome
FROM matches_spain;
```
![image](https://user-images.githubusercontent.com/118057504/219963071-fb9437b9-d745-469b-8476-4caba4478472.png)

2
 - Left join the teams_spain table team_api_id column to the matches_spain table awayteam_id. This allows us to retrieve the away team's identity.
 - Select team_long_name from teams_spain as opponent and complete the CASE statement from Step 1.
```
SELECT 
	m.date,
	--Select the team long name column and call it 'opponent'
	t.team_long_name AS opponent, 
	-- Complete the CASE statement with an alias
	CASE WHEN m.home_goal > m.away_goal THEN 'Home win!'
        WHEN m.home_goal < m.away_goal THEN 'Home loss :('
        ELSE 'Tie' END AS outcome
FROM matches_spain AS m
-- Left join teams_spain onto matches_spain
LEFT JOIN teams_spain AS t
ON m.awayteam_id = t.team_api_id;
```
![image](https://user-images.githubusercontent.com/118057504/219963137-ee8eb181-1717-4537-95d8-7f518ffed260.png)

3
 - Complete the same CASE statement as the previous steps.
 - Filter for matches where the home team is FC Barcelona (id = 8634).
 ```
 SELECT 
	m.date,
	t.team_long_name AS opponent,
    -- Complete the CASE statement with an alias
	CASE WHEN m.home_goal > m.away_goal THEN 'Barcelona win!'
        WHEN m.home_goal < m.away_goal THEN 'Barcelona loss :(' 
        ELSE 'Tie' END AS outcome 
FROM matches_spain AS m
LEFT JOIN teams_spain AS t 
ON m.awayteam_id = t.team_api_id
-- Filter for Barcelona as the home team
WHERE m.hometeam_id = 8634; 
```
![image](https://user-images.githubusercontent.com/118057504/219963210-ad3bbe11-34b2-4b27-b81c-5ef6403e765f.png)

## In CASE things get more complex

If you want to test multiple logical conditions in a CASE statement, you can use AND inside your WHEN clause. 
For example, let's see if each match was played, and won, by the team Chelsea. Let's see the CASE statement in this query. Each WHEN clause contains two logical tests.

<img src='https://user-images.githubusercontent.com/118057504/219963447-4a6330c1-60b4-4f49-acef-6db83c28babd.png' width=600 height=500>

When testing logical conditions, it's important to carefully consider which rows of your data are part of your ELSE clause, and if they're categorized correctly.
Here, we specify this by using an OR statement in WHERE, which retrieves only results where the id 8455 is present in the hometeam_id or awayteam_id columns. 

## What's NULL?
It's also important to consider what your ELSE clause is doing. These two queries here are identical, except for the ELSE NULL statement specified in the second. They both return identical results - a table with quite a few null results. But what if you want to exclude them?

<img src='https://user-images.githubusercontent.com/118057504/219964114-cf51d817-59be-4818-ba9e-dac0aa49493f.png' width=600 height=500>


Let's say we're only interested in viewing the results of games where Chelsea won, and we don't care if they lose or tie. Simply removing the ELSE clause will still retrieve those results - and a lot of NULL values.

<img src='https://user-images.githubusercontent.com/118057504/219964167-9e2759d7-2d8a-499f-8f8d-207c8e5b3326.png' width=600 height=500>



To correct for this, you can treat the entire CASE statement as a column to filter by in your WHERE clause, just like any other column. In order to filter a query by a CASE statement, you include the entire CASE statement, except its alias, in WHERE. You then specify what you want to include, or exclude. For this query, I want to keep all rows where this CASE statement IS NOT NULL. My resulting table now only includes Chelsea's home and away wins -- and I don't need to filter by their team ID anymore!

<img src='https://user-images.githubusercontent.com/118057504/219964227-ce9eb397-7814-4130-9263-ebffb4b72012.png' width=600 height=500>


## CASE WHEN with aggregate functions

<img src='https://user-images.githubusercontent.com/118057504/219964314-414f8d7e-ac4f-4249-b2ae-0ce8278a732f.png' width=600 height=300>

<img src='https://user-images.githubusercontent.com/118057504/219964585-e5932da7-1e1a-4c69-807b-ef0cd23c923b.png' width=600 height=500>

<img src='https://user-images.githubusercontent.com/118057504/219964676-161e0e49-58c3-4b92-b94d-aa0b84f214ff.png' width=600 height=500>

<img src='https://user-images.githubusercontent.com/118057504/219964705-c0e72237-5ea8-4f3a-8ff4-457988f05114.png' width=600 height=500>

<img src='https://user-images.githubusercontent.com/118057504/219964723-e40510de-1a6d-4153-8013-1bce6dc721a2.png' width=600 height=500>

<img src='https://user-images.githubusercontent.com/118057504/219964763-f25f33d9-08db-4e4c-a78c-5ae43c351fdb.png' width=600 height=500>



