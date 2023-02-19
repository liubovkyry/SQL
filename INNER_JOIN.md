
## The ins and outs of INNER JOIN
INNER JOIN, along with LEFT JOIN makes up one of the two most common joins.

The INNER JOIN shown looks for records in both tables with the same values in the key field, id.

![image](https://user-images.githubusercontent.com/118057504/219973584-61cc15b5-f892-41ed-b977-cd7f0fb891b5.png)

Throughout this course, we'll work with a database of world leaders. Our database schema is displayed here. It contains the presidents, prime_ministers and monarchs tables, as well as the states table containing independence years, and the prime_minister_terms table, providing years the prime_ministers assumed office.

![image](https://user-images.githubusercontent.com/118057504/219973641-19652f1a-c5de-40fb-92bf-551d4a2f698f.png)

![image](https://user-images.githubusercontent.com/118057504/219973718-85b80ab9-59b0-49b2-99ec-7cd13fd02d12.png)

![image](https://user-images.githubusercontent.com/118057504/219973768-956b1fc0-c2a8-4939-aa25-2570e95de2aa.png)
![image](https://user-images.githubusercontent.com/118057504/219973784-474d4528-b31e-4433-8393-082ddbc0afa2.png)
![image](https://user-images.githubusercontent.com/118057504/219973804-b302a816-216e-4503-b939-63399fd50c75.png)

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

