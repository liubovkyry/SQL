
## 1. CROSS JOIN diagram

CROSS JOINs are slightly different than joins we have seen previously: they create all possible combinations of two tables. Let's explore the diagram for a CROSS JOIN. In this diagram we have two tables named table1 and table2, with one field each: id1 and id2, respectively. The result of the CROSS JOIN is all nine combinations of the id values of 1, 2, and 3 in table1 with the id values of A, B, and C for table2.

## 2. CROSS JOIN syntax

Let's have a look at the syntax for CROSS JOIN. Note that the syntax is very minimal, and we do not specify ON or USING with CROSS JOIN.

## 3. Pairing prime ministers with presidents

Let's apply the SQL syntax for a CROSS JOIN to the following problem that uses our database of global leaders. Suppose that all prime ministers in Asia from our database are scheduled for individual meetings with all presidents in South America from our database, and we are journalists who wish to follow all the meetings that will happen. We can create a query that returns all these combinations using a CROSS JOIN! We use a WHERE clause to focus only on prime ministers from Asia and presidents from South America. The results of the query give us all possible pairings of the four prime ministers from Asia in the prime_ministers table, and the two presidents from South America in the presidents table.

## 4. Let's practice!
