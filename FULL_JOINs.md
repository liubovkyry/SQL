
## 1. FULL JOINs
 
 FULL JOIN is the last of the three types of outer join. We'll compare FULL JOIN to the two other types of outer join we have learned (LEFT JOIN and RIGHT JOIN), as well as to INNER JOIN.

## 2. FULL JOIN initial diagram

A FULL JOIN combines a LEFT JOIN and a RIGHT JOIN. As you can see in this diagram, no values are faded out as they were in earlier diagrams. This is because the FULL JOIN will return all ids, irrespective of whether they have a match in the other table being joined.

![image](https://user-images.githubusercontent.com/118057504/234127679-c973273b-167e-41fb-8efd-9b44cd80984d.png)

## 3. FULL JOIN diagram

Let's have a look at the result after FULL JOIN. We see that it has retained all ids, returning missing values in the form of nulls for fields corresponding to records where id did not find a match. All six ids we have been working with are included in the result after the FULL JOIN. Note that this time, nulls can appear in either left_value or right_value fields.

## 4. FULL JOIN syntax

 In order to produce a FULL JOIN, the general format aligns closely with the SQL syntax we've been using for INNER JOIN, LEFT JOIN and RIGHT JOIN. We adapt our join to have the word 'FULL' before 'JOIN'. Note that the keyword FULL OUTER JOIN can also be used to return the same result.
 ![image](https://user-images.githubusercontent.com/118057504/234128102-cafb0b5a-1d4f-4f81-97c1-2607468e79a0.png)


## 5. FULL JOIN example using leaders database

Let's make our sample syntax concrete with our example from the leaders database. Suppose we were interested in all the countries in our database, and wanted to check whether they had a president, a prime minister, or both. We'll walk through the code line by line to do this using a FULL JOIN. The SELECT statement starts us off by including the country, as well as the prime_minister and president fields.

## 6. FULL JOIN example using leaders database

Next, we specify prime_ministers as our left table and alias this as p1.<i><b> Note </b>that order of the tables matters here,</i> and if we switched the order, the records would be ordered differently depending on how prime ministers and presidents are ordered in the tables.

## 7. FULL JOIN example using leaders database

We then add the FULL JOIN query and add presidents as the right table, using alias p2.

## 8. FULL JOIN example using leaders database

Lastly, the join is performed using country as the field to join on in both tables. We LIMIT to the first 10 records.
![image](https://user-images.githubusercontent.com/118057504/234128442-6e37d397-23ba-48e2-bdcc-9e39fcd5d6b4.png)


## 9. FULL JOIN example using leaders database

Here is a look at our result. Note that there are null values in both the prime_minister and president fields. We chose a FULL JOIN because we were interested in all countries, whether they had a prime minister, a president, or both.
![image](https://user-images.githubusercontent.com/118057504/234128514-718c0b3b-8075-4ccd-9b40-d952607d3838.png)

## 10. Let's practice!

Comparing joins
In this exercise, you'll examine how results can differ when performing a full join compared to a left join and inner join by joining the countries and currencies tables. You'll be focusing on the North American region and records where the name of the country is missing.

You'll begin with a full join with countries on the left and currencies on the right. Recall the workings of a full join with the diagram below!
![image](https://user-images.githubusercontent.com/118057504/234226734-320aee01-7024-4cf8-96e7-3a66606384b3.png)

Perform a <b>full join</b> with countries (left) and currencies (right).
Filter for the North America region or NULL country names.
<i>Hint</i>
The WHERE clause can have multiple conditions, separated by AND or OR.
field IS NULL can be used in a WHERE clause to filter for NULL values in a field.

```
SELECT name AS country, code, region, basic_unit
FROM countries
-- Join to currencies
FULL JOIN currencies 
USING (code)
-- Where region is North America or name is null
WHERE region = 'North America' OR name IS NULL
ORDER BY region;
```
![image](https://user-images.githubusercontent.com/118057504/234235429-e478ef28-6f3c-43b0-8b09-6cd2211bfbe3.png)

Repeat the same query as before, turning your full join into a <b>left join</b> with the currencies table.
Have a look at what has changed in the output by comparing it to the full join result.

```
SELECT name AS country, code, region, basic_unit
FROM countries
-- Join to currencies
LEFT JOIN currencies
USING (code)
WHERE region = 'North America' 
	OR name IS NULL
ORDER BY region;
```
![image](https://user-images.githubusercontent.com/118057504/234235800-00939207-1808-447e-a2eb-c9027483e497.png)

Repeat the same query again, this time performing an <b>inner join</b> of countries with currencies.
Have a look at what has changed in the output by comparing it to the full join and left join results!

```
SELECT name AS country, code, region, basic_unit
FROM countries
-- Join to currencies
INNER JOIN currencies
USING (code)
WHERE region = 'North America' 
	OR name IS NULL
ORDER BY region;
```
![image](https://user-images.githubusercontent.com/118057504/234236340-b2ce558f-00ed-45da-9218-428fd9d1eaeb.png)

### Chaining FULL JOINs
As you have seen in the previous chapter on INNER JOIN, it is possible to chain joins in SQL, such as when looking to connect data from more than two tables.

Suppose you are doing some research on Melanesia and Micronesia, and are interested in pulling information about languages and currencies into the data we see for these regions in the countries table. Since languages and currencies exist in separate tables, this will require two consecutive full joins involving the countries, languages and currencies tables.

Complete the FULL JOIN with countries as c1 on the left and languages as l on the right, using code to perform this join.
Next, chain this join with another FULL JOIN, placing currencies on the right, joining on code again.

```
SELECT 
	c1.name AS country, 
    region, 
    l.name AS language,
	basic_unit, 
    frac_unit
FROM countries as c1 
-- Full join with languages (alias as l)
FULL JOIN languages AS l
USING(code)
-- Full join with currencies (alias as c2)
FULL JOIN currencies AS c2
USING(code)
WHERE region LIKE 'M%esia';
```

![image](https://user-images.githubusercontent.com/118057504/234238768-2bc48050-cb38-4d86-a07f-3fa8989a4866.png)



