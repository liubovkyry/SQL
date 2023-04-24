


1. INNER JOIN diagram

Recall the INNER JOIN diagram from Chapter 1. The only records in the result were those where the id field had matching values in both tables.
![image](https://user-images.githubusercontent.com/118057504/234122191-1c23ef01-45e3-48d0-a78a-99bf58f356e6.png)


2. LEFT JOIN initial diagram

We'll now compare INNER JOIN with three types of outer joins, beginning with LEFT JOIN. Outer joins can obtain records from other tables, even if matches are not found for the field being joined on. A LEFT JOIN will return all records in the left_table, and those records in the right_table that match on the joining field provided. In the diagram shown, the values of 2 and 3 do not appear in the id field of right_table but will still be retained in the join. Records that are not of interest to a LEFT JOIN on the id field have been faded out.

3. LEFT JOIN diagram

We now look at the result of the LEFT JOIN on id. INNER JOIN returns only records corresponding to ids 1 and 4, whereas <i>LEFT JOIN keeps all records in left_table, as well as null values for right_val where is no match in right_table.</i> Note that ids 5 and 6 in right_table do not feature in LEFT JOIN in any way.
![image](https://user-images.githubusercontent.com/118057504/234122492-e5ffd9f3-bb4b-40c0-bf53-ad84dcfb40b7.png)

![image](https://user-images.githubusercontent.com/118057504/234122581-a9b70806-0f15-4f75-b8f4-0de68402a1a8.png)

4. LEFT JOIN syntax

Let's go back to our world leaders example. Say we want our query to include all countries with prime ministers, presidents if they happen to have them, and missing values if they don't. LEFT JOIN will give us the results we need! The syntax of LEFT JOIN is very similar to INNER JOIN. Only the word INNER is replaced with LEFT. Note that LEFT JOIN can also be written as LEFT OUTER JOIN in SQL. The first three records in the result are the same as they were with an INNER JOIN, but from the fourth record the result starts to look different. Since the United Kingdom does not have a president, a corresponding null value is returned in the president field.
![image](https://user-images.githubusercontent.com/118057504/234123328-c9c2cadc-6544-4848-be0d-d21cccb1eccd.png)


5. RIGHT JOIN

On to RIGHT JOIN! RIGHT JOIN is the second type of outer join, and is much less common than LEFT JOIN so we won't spend as much time on it here. Instead of matching entries in the id column of the left table to the id column of the right table, a RIGHT JOIN does the reverse. All records are retained from right_table, even when id doesn't find a corresponding match in left_table. Null values are returned for the left_value field in records that do not find a match.

6. RIGHT JOIN syntax

Generic syntax for a RIGHT JOIN is shown. Note that the order of left_table and right_table is the same as in LEFT JOIN. The only change is that we call RIGHT JOIN instead of LEFT JOIN. RIGHT JOIN can also be written as RIGHT OUTER JOIN in SQL.

7. RIGHT JOIN with presidents and prime ministers

Let's make this concrete with our world leaders example. We perform a right join of prime_ministers on the left and presidents on the right. The only change is from the LEFT JOIN keyword to RIGHT JOIN. The result contains null values where countries have presidents but no prime ministers.

8. LEFT JOIN or RIGHT JOIN?

Now that you're familiar with both LEFT and RIGHT JOIN, let's discuss why RIGHT JOIN is less commonly used. A key reason for this is that a RIGHT JOIN can always be re-written as a LEFT JOIN. Because we typically type from left to right, LEFT JOIN feels more intuitive to most users when constructing queries.

9. Let's practice!
