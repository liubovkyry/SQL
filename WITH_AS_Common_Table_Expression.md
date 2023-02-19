
# Common Table Expressions

A common method for improving readability and accessibility of information in subqueries - the common table expression.

Common table expressions, or CTEs are a special type of subquery that is declared ahead of your main query, just like you see here. Instead of wrapping subqueries inside, say the FROM statement, you name it using the WITH statement, and then reference it by name later in the FROM statement as if it were any other table in your database.

<img src='https://user-images.githubusercontent.com/118057504/219969925-d226cfa6-2a56-45a0-9435-c1c2259b5f12.png' width=600 height=350>

Let's rewrite a query by using a CTE. The query you see below uses a subquery, s, in the FROM statement to generate a list of country id's and match IDs that meet a certain criteria - specifically, we only wanted matches with 10 or more goals scored in total. This subquery is then joined to the country table, and the number of matches in the subquery is counted in the main query. Here are the results of that query - a short list of countries with very few high-scoring matches.

<img src='https://user-images.githubusercontent.com/118057504/219969994-69295344-b559-4849-925c-b658f5606b05.png' width=600 height=400>

In order to rewrite this query using a common table expression to represent the subquery, simply take the subquery out of the FROM clause, place it <i>at the beginning of your query</i>, declare it using the syntax <code>WITH</code>, followed by a CTE name, and AS. So, here we're starting our CTE, s, by stating WITH s <code>AS</code>, and then placing the subquery inside parentheses. It's now a common table expression!

<img src='https://user-images.githubusercontent.com/118057504/219970187-431480fa-f44a-4cd6-a440-58f9ff629078.png' width=600 height=350>

Finally, complete the rest of the query the same way you would if the CTE were an existing table in the database. You select the country name from the country table, count the number of matches in the CTE "s", <code>JOIN</code> "s" to the country table, and then group the results by the country name's alias. 
The results - you guessed it - are identical to the previous query setup!

<img src='https://user-images.githubusercontent.com/118057504/219970276-bc9af9e9-a45b-410a-a846-390a987b0975.png' width=600 height=400>
