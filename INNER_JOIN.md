
## The ins and outs of INNER JOIN
INNER JOIN, along with LEFT JOIN makes up one of the two most common joins.

The INNER JOIN shown looks for records in both tables with the same values in the key field, id.

<img src='https://user-images.githubusercontent.com/118057504/219973584-61cc15b5-f892-41ed-b977-cd7f0fb891b5.png' width=600 height=400>

Throughout this course, we'll work with a database of world leaders. Our database schema is displayed here. It contains the presidents, prime_ministers and monarchs tables, as well as the states table containing independence years, and the prime_minister_terms table, providing years the prime_ministers assumed office.

<img src='https://user-images.githubusercontent.com/118057504/219973641-19652f1a-c5de-40fb-92bf-551d4a2f698f.png' width=650 height=400>

<img src='https://user-images.githubusercontent.com/118057504/219973718-85b80ab9-59b0-49b2-99ec-7cd13fd02d12.png' width=650 height=400>

<img src='https://user-images.githubusercontent.com/118057504/219973768-956b1fc0-c2a8-4939-aa25-2570e95de2aa.png' width=650 height=400>
<img src='https://user-images.githubusercontent.com/118057504/219973784-474d4528-b31e-4433-8393-082ddbc0afa2.png' width=650 height=400>
<img src='https://user-images.githubusercontent.com/118057504/219973804-b302a816-216e-4503-b939-63399fd50c75.png' width=650 height=400>

### Joining with aliased tables
Recall from the video that instead of writing full table names in queries, you can use table aliasing as a shortcut. The alias can be used in other parts of your query, such as the SELECT statement!

You also learned that when you SELECT fields, a field can be ambiguous. For example, imagine two tables, apples and oranges, both containing a column called color. You need to use the syntax apples.color or oranges.color in your SELECT statement to point SQL to the correct table. Without this, you would get the following error:

 <code> column reference "color" is ambiguous </code> 
In this exercise, you'll practice joining with aliased tables. You'll use data from both the countries and economies tables to examine the inflation rate in 2010 and 2015.

When writing joins, many SQL users prefer to write the SELECT statement after writing the join code, in case the SELECT statement requires using table aliases.

 - Start with your inner join in line 5; join the tables countries AS c (left) with economies (right), aliasing economies AS e.
 - Next, use code as your joining field in line 7; do not use the USING command here.
 - Lastly, select the following columns in order in line 2: code from the countries table (aliased as country_code), name, year, and inflation_rate.

```
-- Select fields with aliases
SELECT c.code AS country_code, c.name, e.year, e.inflation_rate
FROM countries AS c
-- Join to economies (alias e)
INNER JOIN economies AS e
-- Match on code field using table aliases
ON c.code=e.code;        -- USING(code)
```
![image](https://user-images.githubusercontent.com/118057504/219973973-ba9eba76-6bf5-4683-94ad-5e9a00f9dc87.png)

## Multiple joins

A powerful feature of SQL is that multiple joins can be combined and run in a single query. Let's have a look at some syntax for multiple joins. We begin with the same INNER JOIN as before, and then chain another INNER JOIN to the result of our first INNER JOIN. Notice that we use left_table-dot-id in the last line of this example. If we want to perform the second join using the id field of right_table rather than left_table, we can replace left_table-dot-id with right_table-dot-id in the final line.

<img src='https://user-images.githubusercontent.com/118057504/219974252-d580dd07-b2ba-4b21-98ce-e635bf342d3a.png' width=650 height=400>

Let's say that we are interested in identifying countries of the world that have both a president and a prime minister, and want to know the year each prime minister came into office. Let's have a look at the prime_minister_terms table from our database of world leaders. It contains a prime_minister column with prime minister names as well as a pm_start column containing the start year of a prime minister's term.

<img src='https://user-images.githubusercontent.com/118057504/219974507-04b11dd2-b1de-4df3-8bb1-33aaf90a4a4f.png' width=650 height=400>
<img src='https://user-images.githubusercontent.com/118057504/219974647-39fc268a-f5d6-42d4-887e-338bfeaca4f1.png' width=650 height=400>
<img src='https://user-images.githubusercontent.com/118057504/219974670-5a9e1fb1-562c-4a16-a524-fb12cd4382ab.png' width=650 height=400>
<img src='https://user-images.githubusercontent.com/118057504/219974713-0c70e939-bd91-46ee-bf02-1d5ba54bfe1e.png' width=650 height=400>

One more thing to know about joins is that it isn't always the case that each value in the field being joined on corresponds to exactly one record in the joining field of the right table. In the example shown, if we join on one field as we have done before, the query will return multiple records from right_table that match with left_table on the id field.

<img src='https://user-images.githubusercontent.com/118057504/219974735-22e379ee-4ba5-4a6d-a00f-3bbc2d688965.png' width=650 height=400>

We can limit the records returned by supplying an additional field to join on by adding the AND keyword to our ON clause. In this example, we join on date, a frequently used second column when joining on multiple fields. The result set now contains records that match on both id AND date.

<img src='https://user-images.githubusercontent.com/118057504/219974913-72664b57-5edc-4505-8b2a-e0990b581b5e.png' width=650 height=400>

### Joining multiple tables
You've seen that the ability to combine multiple joins using a single query is a powerful feature of SQL.

Suppose you are interested in the relationship between fertility (populations) and unemployment (economies) rates. Your task in this exercise is to join tables to return the country name, year, fertility rate, and unemployment rate in a single result from the countries, populations and economies tables.

STEP 1
 - Perform an inner join of countries AS c (left) with populations AS p (right), on code.
 - Select name, year and fertility_rate.
```
-- Select relevant fields
SELECT name, year, fertility_rate
-- Inner join countries and populations, aliased, on code
FROM populations AS p
INNER JOIN countries AS c
ON c.code = p.country_code;
```
![image](https://user-images.githubusercontent.com/118057504/219975347-31be0543-edf0-4d54-8075-6ae8648e6859.png)

STEP 2
 - Chain another inner join to your query with the economies table AS e, using code.
 - Select name, and using table aliases, select year and unemployment_rate from economies.

```
-- Select fields
SELECT name, e.year, fertility_rate, e.unemployment_rate
FROM populations AS p
INNER JOIN countries AS c
ON p.country_code = c.code
-- Join to economies (as e)
INNER JOIN economies AS e
-- Match on country code
ON p.country_code = e.code;
```
![image](https://user-images.githubusercontent.com/118057504/219975524-ba3cfb15-3118-453c-bcc9-4b0ad7357f3e.png)

### Checking multi-table joins
Have a look at the results for Albania from the previous query below. You can see that the 2015 fertility_rate has been paired with 2010 unemployment_rate, and vice versa.
```
name	   year	fertility_rate	unemployment_rate
Albania	2015	1.663	         17.1
Albania	2010	1.663	         14
Albania	2015	1.793	         17.1
Albania	2010	1.793	         14
```
Instead of four records, the query should return two: one for each year. The last join was performed on <code>c.code = e.code</code>, without also joining on <code>year</code>. Your task in this exercise is to fix your query by explicitly stating that both the country <code>code</code> and <code>year</code> should match!

```
SELECT name, e.year, fertility_rate, unemployment_rate
FROM countries AS c
INNER JOIN populations AS p
ON c.code = p.country_code
INNER JOIN economies AS e
ON c.code = e.code
-- Add an additional joining condition such that you are also joining on year
AND p.year = e.year;
```
![image](https://user-images.githubusercontent.com/118057504/219975727-0d112cf5-1871-4291-85e8-ccf2a67c6cd6.png)
