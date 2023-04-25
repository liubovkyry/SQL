
## 1. CROSS JOIN diagram

CROSS JOINs are slightly different than joins we have seen previously: they create all possible combinations of two tables. Let's explore the diagram for a CROSS JOIN. In this diagram we have two tables named table1 and table2, with one field each: id1 and id2, respectively. The result of the CROSS JOIN is all nine combinations of the id values of 1, 2, and 3 in table1 with the id values of A, B, and C for table2.
![image](https://user-images.githubusercontent.com/118057504/234239544-f14d5e1b-956b-42c3-9ae9-317ca7eb0eff.png)


## 2. CROSS JOIN syntax

Let's have a look at the syntax for CROSS JOIN. Note that the syntax is very minimal, and we <b>do not</b> specify ON or USING with CROSS JOIN.
![image](https://user-images.githubusercontent.com/118057504/234239721-f268ba3f-6f92-4c80-b07a-23db38117e68.png)

## 3. Pairing prime ministers with presidents

Let's apply the SQL syntax for a CROSS JOIN to the following problem that uses our database of global leaders. Suppose that all prime ministers in Asia from our database are scheduled for individual meetings with all presidents in South America from our database, and we are journalists who wish to follow all the meetings that will happen. We can create a query that returns all these combinations using a CROSS JOIN! We use a WHERE clause to focus only on prime ministers from Asia and presidents from South America. The results of the query give us all possible pairings of the four prime ministers from Asia in the prime_ministers table, and the two presidents from South America in the presidents table.
![image](https://user-images.githubusercontent.com/118057504/234240167-925f2020-54c4-4e00-8d80-b36324428092.png)

## 4. Let's practice!

CROSS JOIN can be incredibly helpful when asking questions that involve looking at all possible combinations or pairings between two sets of data.

Imagine you are a researcher interested in the languages spoken in two countries: Pakistan and India. You are interested in asking:

What are the languages presently spoken in the two countries?
Given the shared history between the two countries, what languages could potentially have been spoken in either country over the course of their history?
In this exercise, we will explore how INNER JOIN and CROSS JOIN can help us answer these two questions, respectively.

Complete the code to perform an INNER JOIN of countries AS c with languages AS l using the code field to obtain the languages currently spoken in the two countries.

```
SELECT c.name AS country, l.name AS language
-- Inner join countries as c with languages as l on code
FROM countries AS c
INNER JOIN languages AS l
USING(code)
WHERE c.code IN ('PAK','IND')
	AND l.code in ('PAK','IND');
 ```
 
 ![image](https://user-images.githubusercontent.com/118057504/234240943-516da263-e719-418b-ba31-f29917fa4b7a.png)

Change your INNER JOIN to a different kind of join to look at possible combinations of languages that could have been spoken in the two countries given their history.
Observe the differences in output for both joins.

```
SELECT c.name AS country, l.name AS language
FROM countries AS c        
-- Perform a cross join to languages (alias as l)
CROSS JOIN languages AS l
WHERE c.code in ('PAK','IND')
	AND l.code in ('PAK','IND');
  ```
  ![image](https://user-images.githubusercontent.com/118057504/234241292-b2a977d5-f4d5-47e3-bf8d-6c5e368af43e.png)

  
