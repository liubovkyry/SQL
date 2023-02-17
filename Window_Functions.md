## It's OVER

Let's tackle another limitation you've likely encountered in SQL -- the fact that you have to group results when using aggregate functions. If you try to retrieve additional information without grouping by every single non-aggregate value, your query will return an error. Thus, you can't compare aggregate values to non-aggregate data.
![image](https://user-images.githubusercontent.com/118057504/219614928-07633814-8f09-4b28-8886-4e31a6d2dc8a.png)

You can work around this limitation using a window function. Window functions are a class of functions that perform calculations on a result set that has already been generated, also referred to as a "window". <b> You can use window functions to perform aggregate calculations without having to group your data, just as you did with a subquery in SELECT.</b> You can also use them to calculate information such as running totals, rankings, and moving averages.
![image](https://user-images.githubusercontent.com/118057504/219615253-5b22113e-df7c-48a0-8ada-54fae093dd0f.png)

![image](https://user-images.githubusercontent.com/118057504/219616021-84e69eae-49e0-40ce-8653-13d0f58b13cb.png)

So what's a window function? How do you use it? Let's start with a query from chapter 2, where we answered the question, "how many goals were scored in each match in 2011/2012, and how did that compare to the average?" This query selects two columns from match table, and then used a subquery in SELECT to pass the overall average along the data set without aggregating the results.

![image](https://user-images.githubusercontent.com/118057504/219616125-91d0d70b-ea45-4da2-a540-0f90e04fb29b.png)

The same results can be generated using the clause common to all window functions -- the OVER clause. Instead of writing a subquery, calculate the AVG of home_goal and away_goal, and follow it with the OVER clause. This clause tells SQL to "pass this aggregate value over this existing result set." The results are identical to the previous statement that used a subquery in SELECT, with a simpler syntax and faster processing time.

![image](https://user-images.githubusercontent.com/118057504/219616538-f7cb29ee-3255-404a-abf1-a94986d88cfe.png)

Another simple type of column you can generate with a window function is a RANK. A RANK simply creates a column numbering your data set from highest to lowest, or lowest to highest, based on a column that you specify.

To create the rank, you start with the RANK function, using parentheses, followed by the OVER clause. Inside the OVER clause, include the ORDER BY clause, and the column or columns you want to use to generate the rank. By default, the RANK function orders the results and ranking from smallest to largest values. In the case of our data set here, this isn't particularly informative.

![image](https://user-images.githubusercontent.com/118057504/219617173-873cdd3e-a995-4cc8-97f5-b96550be288b.png)



You can easily correct this by adding the DESC function to reverse the order of the rank, just as you would if you were using ORDER BY at the end of your query. You'll notice that the RANK function automatically ties identical values, such as the first 2 results, and then skips the next value in the rank.


![image](https://user-images.githubusercontent.com/118057504/219617646-8e75ba00-12ed-417b-8b38-0355a8f6a934.png)

CODE SAMPLES:

The OVER() clause allows you to pass an aggregate function down a data set, similar to subqueries in SELECT. The OVER() clause offers significant benefits over subqueries in select -- namely, your queries will run faster, and the OVER() clause has a wide range of additional functions.
```
SELECT 
	-- Select the id, country name, season, home, and away goals
	m.id, 
    c.name AS country, 
    m.season,
	m.home_goal,
	m.away_goal,
    -- Use a window to include the aggregate average in each row
	AVG(m.home_goal + m.away_goal)OVER() AS overall_avg
FROM match AS m
LEFT JOIN country AS c 
ON m.country_id = c.id;
```
Window functions allow you to create a RANK of information according to any variable you want to use to sort your data. When setting this up, you will need to specify what column/calculation you want to use to calculate your rank. This is done by including an ORDER BY clause inside the OVER() clause.

```
SELECT 
	-- Select the league name and average goals scored
	l.name AS league,
    AVG(m.home_goal + m.away_goal) AS avg_goals,
    -- Rank each league according to the average goals
    RANK() OVER(ORDER BY AVG(m.home_goal + m.away_goal)) AS league_rank
FROM league AS l
LEFT JOIN match AS m 
ON l.id = m.country_id
WHERE m.season = '2011/2012'
GROUP BY l.name
-- Order the query by the rank you created
ORDER BY league_rank;
```
In the last exercise, the rank generated in your query was organized from smallest to largest. By adding DESC to your window function, you can create a rank sorted from largest to smallest.

```
SELECT 
	-- Select the league name and average goals scored
	l.name AS league,
    AVG(m.home_goal + m.away_goal) AS avg_goals,
    -- Rank leagues in descending order by average goals
    RANK() OVER(ORDER BY AVG(m.home_goal + m.away_goal) DESC) AS league_rank
FROM league AS l
LEFT JOIN match AS m 
ON l.id = m.country_id
WHERE m.season = '2011/2012'
GROUP BY l.name
-- Order the query by the rank you created
ORDER BY league_rank;
```

## OVER with a PARTITION

One important statement you can add to your OVER clause is PARTITION BY. 
![image](https://user-images.githubusercontent.com/118057504/219622504-0cb9a118-844e-420e-93d7-d20fb028bedd.png)

![image](https://user-images.githubusercontent.com/118057504/219623501-ae6b8433-eb95-43ff-9264-c9271e934fd0.png)
![image](https://user-images.githubusercontent.com/118057504/219623962-a7145a83-d796-4b9f-bc4c-6f97dd88ec7c.png)

Let's expand on the previous question, and instead ask, "How many goals were scored in each match, and how did that compare to the season's average?" We can do this by adding a PARTITION BY clause to the OVER clause from the previous slide. Specifying, "PARTITION BY season" returns each season's average on each row, in accordance to the season that each record belongs to. As you can see, rows 1 and 2 are matches played in the 2011/2012 season, and the season_avg column contains the 2011/2012 season average. Rows 3 and 4 are part of the 2012/2013 season, and return the 2012/2013 season average.

![image](https://user-images.githubusercontent.com/118057504/219625451-9efba641-7570-4c57-a232-588e845c454b.png)


The result set returns the average goals scored broken out by season and country. In row 1, a match was played in Belgium in the 2011/2012 season, and had 1 goal scored throughout the match. This is compared to the 2.88, which is the average goals scored in Belgium in the 2011/2012 season.

CODE SAMPLES:

The PARTITION BY clause allows you to calculate separate "windows" based on columns you want to divide your results. For example, you can create a single column that calculates an overall average of goals scored for each season.
```
SELECT
	date,
	season,
	home_goal,
	away_goal,
	CASE WHEN hometeam_id = 8673 THEN 'home' 
		 ELSE 'away' END AS warsaw_location,
    -- Calculate the average goals scored partitioned by season
    AVG(home_goal) OVER(PARTITION BY season) AS season_homeavg,
    AVG(away_goal) OVER(PARTITION BY season) AS season_awayavg
FROM match
-- Filter the data set for Legia Warszawa matches only
WHERE 
	hometeam_id = 8673 
    OR awayteam_id = 8673
ORDER BY (home_goal + away_goal) DESC;
```
The PARTITION BY clause can be used to break out window averages by multiple data points (columns). You can even calculate the information you want to use to partition your data! For example, you can calculate average goals scored by season and by country, or by the calendar year (taken from the date column).
```
SELECT 
	date,
	season,
	home_goal,
	away_goal,
	CASE WHEN hometeam_id = 8673 THEN 'home' 
         ELSE 'away' END AS warsaw_location,
	-- Calculate average goals partitioned by season and month
    AVG(home_goal) OVER(PARTITION BY season, 
         	EXTRACT(month FROM date)) AS season_mo_home,
    AVG(away_goal) OVER(PARTITION BY season, 
         	EXTRACT(month FROM date)) AS season_mo_away
FROM match
WHERE 
	hometeam_id = 8673
    OR awayteam_id = 8673
ORDER BY (home_goal + away_goal) DESC;
```

## Sliding windows
In addition to calculating aggregate and rank information, window functions can also be used to calculate information that changes with each subsequent row in a data set.

These types of window functions are called sliding windows. Sliding windows are functions that perform calculations relative to the current row of a data set. You can use sliding windows to calculate a wide variety of information that aggregates one row at a time down your data set -- running totals, sums, counts, and averages in any order you need. A sliding window calculation can also be partitioned by one or more columns, just like a non-sliding window.

The general syntax looks like this -- you use the phrase ROWS BETWEEN to indicate that you plan on slicing information in your window function for each row in the data set, and then you specify the starting and finishing point of the calculation. For the start and finish in your ROWS BETWEEN statement, you can specify a number of keywords as shown here. PRECEDING and FOLLOWING are used to specify the number of rows before, or after, the current row that you want to include in a calculation. UNBOUNDED PRECEDING and UNBOUNDED FOLLOWING tell SQL that you want to include every row since the beginning, or the end, of the data set in your calculations. Finally, CURRENT ROW tells SQL that you want to stop your calculation at the current row.
![image](https://user-images.githubusercontent.com/118057504/219634201-f7f8b1cf-5191-4fdb-96f9-546f98baacd3.png)

The sliding window in this query includes several key pieces of information in its calculation. It first states that the goal is to calculate a sum of goals scored when Manchester City played as the home team during the 2011/2012 season. It then tells you that you want to turn this calculation into a running total, ordered by the date of the match from oldest to most recent and calculated from the beginning of the data set to the current row. 

![image](https://user-images.githubusercontent.com/118057504/219634518-a17cc7b9-3302-4bf1-984d-f66fffed0e3c.png)

Using the PRECEDING statement, you also have the ability to calculate sliding windows with a more limited frame. For example, the query you see here is similar to the previous one, with a slightly modified sliding window. The phrase UNBOUNDED PRECEDING is replaced here with the phrase 1 PRECEDING, which calculates the sum of Manchester City's goals in the current and previous match. As you see in the data set here, the two rows in red are used to calculate the sum on the second row, and the two rows in green are used to calculate the sum on the third row.
![image](https://user-images.githubusercontent.com/118057504/219635374-e2de533a-78c4-4f89-b3a9-5a4927603174.png)

CODE SAMPLES:

Sliding windows allow you to create running calculations between any two points in a window using functions such as PRECEDING, FOLLOWING, and CURRENT ROW. You can calculate running counts, sums, averages, and other aggregate functions between any two points you specify in the data set.

```
SELECT 
	date,
	home_goal,
	away_goal,
    -- Create a running total and running average of home goals
    SUM(home_goal) OVER(ORDER BY date 
         ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total,
    AVG(home_goal) OVER(ORDER BY date 
         ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_avg
FROM match
WHERE 
	hometeam_id = 9908 
	AND season = '2011/2012';
```
n this exercise, you will slightly modify the query from the previous exercise by sorting the data set in reverse order and calculating a backward running total from the CURRENT ROW to the end of the data set (earliest record).
```
SELECT 
	-- Select the date, home goal, and away goals
    date,
    home_goal,
    away_goal,
    -- Create a running total and running average of home goals
    SUM(home_goal) OVER(ORDER BY date DESC
         ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING) AS running_total,
    AVG(home_goal) OVER(ORDER BY date DESC
         ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING) AS running_avg
FROM match
WHERE 
	awayteam_id = 9908 
    AND season = '2011/2012';
    ```


