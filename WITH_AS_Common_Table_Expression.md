
# Common Table Expressions

A common method for improving readability and accessibility of information in subqueries - the common table expression.

Common table expressions, or CTEs are a special type of subquery that is declared ahead of your main query, just like you see here. Instead of wrapping subqueries inside, say the FROM statement, you name it using the WITH statement, and then reference it by name later in the FROM statement as if it were any other table in your database.

<img src='https://user-images.githubusercontent.com/118057504/219969925-d226cfa6-2a56-45a0-9435-c1c2259b5f12.png' width=600 height=350>

Let's rewrite a query by using a CTE. The query you see below uses a subquery, s, in the FROM statement to generate a list of country id's and match IDs that meet a certain criteria - specifically, we only wanted matches with 10 or more goals scored in total. This subquery is then joined to the country table, and the number of matches in the subquery is counted in the main query. Here are the results of that query - a short list of countries with very few high-scoring matches.

<img src='https://user-images.githubusercontent.com/118057504/219969994-69295344-b559-4849-925c-b658f5606b05.png' width=600 height=400>

In order to rewrite this query using a common table expression to represent the subquery, simply take the subquery out of the FROM clause, place it <i>at the beginning of your query</i>, declare it using the syntax <code>WITH</code>, followed by a CTE name, and AS. So, here we're starting our CTE, s, by stating WITH s <code>AS</code>, and then placing the subquery inside parentheses. It's now a common table expression!

<img src='https://user-images.githubusercontent.com/118057504/219970187-431480fa-f44a-4cd6-a440-58f9ff629078.png' width=600 height=300>

Finally, complete the rest of the query the same way you would if the CTE were an existing table in the database. You select the country name from the country table, count the number of matches in the CTE "s", <code>JOIN</code> "s" to the country table, and then group the results by the country name's alias. 
The results - you guessed it - are identical to the previous query setup!

<img src='https://user-images.githubusercontent.com/118057504/219970276-bc9af9e9-a45b-410a-a846-390a987b0975.png' width=600 height=400>

If you have multiple subqueries that you want to turn into a common table expression, you can simply list them one after another, with a comma in between each CTE, and NO comma after the last one. You can then retrieve the information you need into the main query - just make sure you properly join this second CTE as well!

<img src='https://user-images.githubusercontent.com/118057504/219970365-c43db5af-fa3d-4e19-96ff-19f1830c12fc.png' width=600 height=400>

## Why use CTEs?

So why are we learning yet another method of producing the same result in a SQL query? Common table expressions have numerous benefits over a subquery written inside your main query.
 - First, the CTE is run only once, and then stored in memory, so it often leads to an improvement in the amount of time it takes to run your query. 
 - Second, CTEs are an excellent tool for organizing long and complex CTEs. You can declare as many CTEs as you need, one after another.
 - You can also reference information in CTEs declared earlier. For example, if you have 3 CTEs in a query, your third CTE can retrieve information from the first and second CTE. 
 - Finally, a CTE can reference itself in a special kind of table called a recursive CTE. 


 ![image](https://user-images.githubusercontent.com/118057504/219970683-8c93f88a-f92c-43e2-98b0-1011c574abb9.png)
 ![image](https://user-images.githubusercontent.com/118057504/219970696-b3277492-0e4f-4508-bc0c-6a1a4c227716.png)


### Clean up with CTEs

Previuously we generated a list of countries and the number of matches in each country with more than 10 total goals. The query in that exercise utilized a subquery in the FROM statement in order to filter the matches before counting them in the main query. Below is the query you created:
```
SELECT
  c.name AS country,
  COUNT(sub.id) AS matches
FROM country AS c
INNER JOIN (
  SELECT country_id, id 
  FROM match
  WHERE (home_goal + away_goal) >= 10) AS sub
ON c.id = sub.country_id
GROUP BY country;
```
You can list one (or more) subqueries as common table expressions (CTEs) by <i>declaring</i> them ahead of your main query, which is an excellent tool for organizing information and placing it in a logical order.

In this exercise, let's rewrite a similar query using a CTE.

 - Complete the syntax to declare your CTE.
 - Select the country_id and match id from the match table in your CTE.
 - Left join the CTE to the league table using country_id.
 
```
-- Set up your CTE
WITH match_list AS (
    SELECT 
  		country_id, 
  		match.id
    FROM match
    WHERE (home_goal + away_goal) >= 10)
-- Select league and count of matches from the CTE
SELECT
    l.name AS league,
    COUNT(match_list.id) AS matches
FROM league AS l
-- Join the CTE to the league table
LEFT JOIN match_list ON l.id = match_list.country_id
GROUP BY l.name;
```

![image](https://user-images.githubusercontent.com/118057504/219970734-d79f28f8-350f-40be-ad79-80911b701c89.png)

### CTEs with nested subqueries
If you find yourself listing multiple subqueries in the FROM clause with nested statement, your query will likely become long, complex, and difficult to read.

Since many queries are written with the intention of being saved and re-run in the future, proper organization is key to a seamless workflow. Arranging subqueries as CTEs will save you time, space, and confusion in the long run!

 - Declare a CTE that calculates the total goals from matches in August of the 2013/2014 season.
 - Left join the CTE onto the league table using country_id from the match_list CTE.
 - Filter the list on the inner subquery to only select matches in August of the 2013/2014 season.

```
-- Set up your CTE
WITH match_list AS (
    SELECT 
  		country_id,
  	   (home_goal + away_goal) AS goals
    FROM match
  	-- Create a list of match IDs to filter data in the CTE
    WHERE id IN (
       SELECT id
       FROM match
       WHERE season = '2013/2014' AND EXTRACT(MONTH FROM date) = 8))
-- Select the league name and average of goals in the CTE
SELECT 
	l.name,
    avg(match_list.goals)
FROM league AS l
-- Join the CTE onto the league table
LEFT JOIN match_list ON l.id = match_list.country_id
GROUP BY l.name;
```
![image](https://user-images.githubusercontent.com/118057504/219970933-f2f80c34-7ffc-4d31-a2df-33463d8055fd.png)

### 

