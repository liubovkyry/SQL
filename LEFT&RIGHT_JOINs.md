


## 1. INNER JOIN diagram

Recall the INNER JOIN diagram from Chapter 1. The only records in the result were those where the id field had matching values in both tables.
![image](https://user-images.githubusercontent.com/118057504/234122191-1c23ef01-45e3-48d0-a78a-99bf58f356e6.png)


## 2. LEFT JOIN initial diagram

We'll now compare INNER JOIN with three types of outer joins, beginning with LEFT JOIN. Outer joins can obtain records from other tables, even if matches are not found for the field being joined on. A LEFT JOIN will return all records in the left_table, and those records in the right_table that match on the joining field provided. In the diagram shown, the values of 2 and 3 do not appear in the id field of right_table but will still be retained in the join. Records that are not of interest to a LEFT JOIN on the id field have been faded out.

## 3. LEFT JOIN diagram

We now look at the result of the LEFT JOIN on id. INNER JOIN returns only records corresponding to ids 1 and 4, whereas <i>LEFT JOIN keeps all records in left_table, as well as null values for right_val where is no match in right_table.</i> Note that ids 5 and 6 in right_table do not feature in LEFT JOIN in any way.
![image](https://user-images.githubusercontent.com/118057504/234122492-e5ffd9f3-bb4b-40c0-bf53-ad84dcfb40b7.png)

![image](https://user-images.githubusercontent.com/118057504/234122581-a9b70806-0f15-4f75-b8f4-0de68402a1a8.png)

## 4. LEFT JOIN syntax

Let's go back to our world leaders example. Say we want our query to include all countries with prime ministers, presidents if they happen to have them, and missing values if they don't. LEFT JOIN will give us the results we need! The syntax of LEFT JOIN is very similar to INNER JOIN. Only the word INNER is replaced with LEFT. Note that LEFT JOIN can also be written as LEFT OUTER JOIN in SQL. The first three records in the result are the same as they were with an INNER JOIN, but from the fourth record the result starts to look different. Since the United Kingdom does not have a president, a corresponding null value is returned in the president field.
![image](https://user-images.githubusercontent.com/118057504/234123328-c9c2cadc-6544-4848-be0d-d21cccb1eccd.png)


## 5. RIGHT JOIN

On to RIGHT JOIN! RIGHT JOIN is the second type of outer join, and is much less common than LEFT JOIN so we won't spend as much time on it here. Instead of matching entries in the id column of the left table to the id column of the right table, a RIGHT JOIN does the reverse. All records are retained from right_table, even when id doesn't find a corresponding match in left_table. Null values are returned for the left_value field in records that do not find a match.
![image](https://user-images.githubusercontent.com/118057504/234123484-cd333c06-9534-4142-a66f-786c582c5aa8.png)


## 6. RIGHT JOIN syntax

Generic syntax for a RIGHT JOIN is shown. <i><b>Note</b> that the order of left_table and right_table is the same as in LEFT JOIN.</i> The only change is that we call RIGHT JOIN instead of LEFT JOIN. RIGHT JOIN can also be written as RIGHT OUTER JOIN in SQL.
![image](https://user-images.githubusercontent.com/118057504/234123774-619f1935-cbe0-4f4f-a301-40dae92f4234.png)


## 7. RIGHT JOIN with presidents and prime ministers

Let's make this concrete with our world leaders example. We perform a right join of prime_ministers on the left and presidents on the right. The only change is from the LEFT JOIN keyword to RIGHT JOIN. 
The result contains null values where countries have presidents but no prime ministers.
![image](https://user-images.githubusercontent.com/118057504/234123866-aad5c935-7783-42a4-8bb1-8f5e622e406e.png)


## 8. LEFT JOIN or RIGHT JOIN?

Now that you're familiar with both LEFT and RIGHT JOIN, let's discuss why RIGHT JOIN is less commonly used. A key reason for this is that a <i>RIGHT JOIN can always be re-written as a LEFT JOIN.</i> Because we typically type from left to right, LEFT JOIN feels more intuitive to most users when constructing queries.

## 9. Let's practice!

In this exercise, you'll explore the differences between INNER JOIN and LEFT JOIN. This will help you decide which type of join to use.

Perform an inner join with cities AS c1 on the left and countries as c2 on the right.
Use code as the field to merge your tables on


```
SELECT 
    c1.name AS city,
    code,
    c2.name AS country,
    region,
    city_proper_pop
FROM cities AS c1
-- Perform an inner join with cities as c1 and countries as c2 on country code
INNER JOIN countries AS c2
ON c1.country_code = c2.code
ORDER BY code DESC;
```
![image](https://user-images.githubusercontent.com/118057504/234125241-15c24d26-e2f2-4687-aa80-9962aec73c4d.png)

Change the code to perform a LEFT JOIN instead of an INNER JOIN.
After executing this query, have a look at how many records the query result contains.

```
SELECT 
	c1.name AS city, 
    code, 
    c2.name AS country,
    region, 
    city_proper_pop
FROM cities AS c1
-- Join right table (with alias)
LEFT JOIN countries AS c2
ON c1.country_code = c2.code
ORDER BY code DESC;
```

![image](https://user-images.githubusercontent.com/118057504/234125416-1a2a92c2-6930-4988-838a-68e0fbc8bf79.png)

### Building on your LEFT JOIN

Being able to build more than one SQL function into your query will enable you to write compact, supercharged queries.

You will use AVG() in combination with a LEFT JOIN to determine the average gross domestic product (GDP) per capita by region in 2010.

Complete the LEFT JOIN with the countries table on the left and the economies table on the right on the code field.
Filter the records from the year 2010.

```
SELECT name, region, gdp_percapita
FROM countries AS c
LEFT JOIN economies AS e
-- Match on code fields
USING(code)
-- Filter for the year 2010
WHERE year = 2010;
```
![image](https://user-images.githubusercontent.com/118057504/234125958-b0eb9c6e-72d3-4410-8580-991f716ddc4a.png)

To calculate per capita GDP per region, begin by grouping by region.
After your GROUP BY, choose region in your SELECT statement, followed by average GDP per capita using the AVG() function, with AS avg_gdp as your alias.

```
-- Select region, and average gdp_percapita as avg_gdp
SELECT region, AVG(gdp_percapita) AS avg_gdp
FROM countries AS c
LEFT JOIN economies AS e
USING(code)
WHERE year = 2010
-- Group by region
GROUP BY region;
```
![image](https://user-images.githubusercontent.com/118057504/234126263-86adb52b-336f-40b1-af8d-aa3ab78776a8.png)

Order the result set by the average GDP per capita from highest to lowest.
Return only the first 10 records in your result.

```
SELECT region, AVG(gdp_percapita) AS avg_gdp
FROM countries AS c
LEFT JOIN economies AS e
USING(code)
WHERE year = 2010
GROUP BY region
-- Order by descending avg_gdp
ORDER BY avg_gdp DESC
-- Return only first 10 records
LIMIT 10;
```
![image](https://user-images.githubusercontent.com/118057504/234126458-282034be-2130-4eaa-94cc-c657a37df983.png)

### Is this RIGHT?

Write a new query using RIGHT JOIN that produces an identical result to the LEFT JOIN provided.
```
SELECT countries.name AS country, languages.name AS language, percent
FROM languages
LEFT JOIN countries
USING(code)
ORDER BY language;
```
![image](https://user-images.githubusercontent.com/118057504/234126927-9c5595bd-662b-4d6c-98ac-01d7f4e051eb.png)

```
SELECT countries.name AS country, languages.name AS language, percent
FROM languages
RIGHT JOIN countries
USING(code)
ORDER BY language;
```

![image](https://user-images.githubusercontent.com/118057504/234127029-3490778c-1c1a-495d-aed7-6ff9f7247955.png)

