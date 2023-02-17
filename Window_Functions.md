## It's OVER

Let's tackle another limitation you've likely encountered in SQL -- the fact that you have to group results when using aggregate functions. If you try to retrieve additional information without grouping by every single non-aggregate value, your query will return an error. Thus, you can't compare aggregate values to non-aggregate data.
![image](https://user-images.githubusercontent.com/118057504/219614928-07633814-8f09-4b28-8886-4e31a6d2dc8a.png)

You can work around this limitation using a window function. Window functions are a class of functions that perform calculations on a result set that has already been generated, also referred to as a "window". You can use window functions to perform aggregate calculations without having to group your data, just as you did with a subquery in SELECT. You can also use them to calculate information such as running totals, rankings, and moving averages.
![image](https://user-images.githubusercontent.com/118057504/219615253-5b22113e-df7c-48a0-8ada-54fae093dd0f.png)

![image](https://user-images.githubusercontent.com/118057504/219616021-84e69eae-49e0-40ce-8653-13d0f58b13cb.png)

So what's a window function? How do you use it? Let's start with a query from chapter 2, where we answered the question, "how many goals were scored in each match in 2011/2012, and how did that compare to the average?" This query selects two columns from match table, and then used a subquery in SELECT to pass the overall average along the data set without aggregating the results.

![image](https://user-images.githubusercontent.com/118057504/219616125-91d0d70b-ea45-4da2-a540-0f90e04fb29b.png)

The same results can be generated using the clause common to all window functions -- the OVER clause. Instead of writing a subquery, calculate the AVG of home_goal and away_goal, and follow it with the OVER clause. This clause tells SQL to "pass this aggregate value over this existing result set." The results are identical to the previous statement that used a subquery in SELECT, with a simpler syntax and faster processing time.

![image](https://user-images.githubusercontent.com/118057504/219616538-f7cb29ee-3255-404a-abf1-a94986d88cfe.png)

Another simple type of column you can generate with a window function is a RANK. A RANK simply creates a column numbering your data set from highest to lowest, or lowest to highest, based on a column that you specify.

To create the rank, you start with the RANK function, using parentheses, followed by the OVER clause. Inside the OVER clause, include the ORDER BY clause, and the column or columns you want to use to generate the rank. By default, the RANK function orders the results and ranking from smallest to largest values. In the case of our data set here, this isn't particularly informative.
