
# Subquerying with semi joins and anti joins

## 1. Subquerying with semi joins and anti joins

In this lesson, we'll return to joins one last time, with an introduction to nested SQL queries.


## 2. Calling all joins

The six joins we've worked with so far are all "additive" in that <i>they add columns to the original "left" table.</i> The diagram shows an INNER JOIN on the id field, showing an additional column being added to the original left table.
![image](https://user-images.githubusercontent.com/118057504/234645499-b7501fe6-7cd4-4e89-9942-331b37d52213.png)


## 3. Additive joins

Here is some familiar INNER JOIN syntax for this join on the id field. Lets dive deeper into what it means for this join to be "additive".
![image](https://user-images.githubusercontent.com/118057504/234645944-a0837dab-08f9-4fa9-8e47-ef7f93bac9e9.png)


## 4. Additive joins

Fields with different names are added to the result set with their original names. Fields with the same name, such as date, are added to the result set too, but notice how we now have duplicate date columns with the same name. This can be changed, however, with aliasing.
![image](https://user-images.githubusercontent.com/118057504/234646187-d33e3142-dcf5-4239-98c6-ea2e6be05f15.png)


## 5. Semi join

In this lesson, we'll see ways of joining data that do not expressly use JOIN keywords and are not additive in the same way. Instead of using JOIN or set operators, we can leverage the WHERE clause to specify the records to include. We'll use the two tables, left_table and right_table, in the diagram shown. A semi join chooses records in the first table where a condition is met in the second table.


## 6. Semi join

More specifically, the semi join will return all values of left_table where values of col1 are in a column we specify, namely col2 in the right_table. Records not of interest to the semi join have been faded out.



## 7. Semi join

The final result of our semi join is records corresponding to ids 2 and 3.
![image](https://user-images.githubusercontent.com/118057504/234647733-81e731ec-2197-4cfb-8de5-b262caaa597d.png)


## 8. Kicking off our semi join

Let's make this concrete with an example. Suppose we are interested in determining the presidents of countries that gained independence before 1800. We select the fields country, continent, and president. How do we filter this data for independence year, which is a field in the states table? We'll use a semi join!
![image](https://user-images.githubusercontent.com/118057504/234646785-bb4c22ef-2ce6-4517-96a3-900f39587ea2.png)


## 9. Building on our semi join

Before we knew anything about joins, we knew we could leverage the WHERE clause to filter data. The query that returns countries with indep_year before 1800 is shown. The query returns only Spain and Portugal from our database.
![image](https://user-images.githubusercontent.com/118057504/234647040-2763a4e2-d9a5-49b8-b537-955f94adafe1.png)


## 10. Finish the semi join (an intro to subqueries)

We can use this list of countries as a filter by embedding it in a further WHERE clause. This is called <b> a subquery</b>! It chooses records in the first table where the country matches the list returned by our subquery. Since Spain does not have a president, only the Portuguese president is listed.

![image](https://user-images.githubusercontent.com/118057504/234647293-a5dba9dc-f59a-4f20-a7d4-acbc56bf53cd.png)

## 11. Anti join

Moving on to anti joins! The anti join chooses records in the first table where col1 does NOT find a match in col2.


## 12. Anti join

The final result of our anti join is records corresponding to ids 1 and 4. Again, no new columns are added.
![image](https://user-images.githubusercontent.com/118057504/234647537-2e4e833b-29bb-4327-a34f-65a08a3048bc.png)


## 13. An anti join with the presidents

How might we adapt our semi join to determine countries in the Americas founded after 1800? To change our semi join from before to after 1800, we add <code>NOT</code> before the <code>IN</code> statement. We call this an anti join! In addition, we add to our first WHERE clause to filter for continents in the Americas. The result shows presidents of countries in the Americas that gained independence after 1800. Only Chile and Uruguay are returned, not the USA, which gained independence before 1800. Note again that we are only using the few countries in our database for illustration; this is not a comprehensive list of countries in the world!
![image](https://user-images.githubusercontent.com/118057504/234648546-1378787e-5841-4ce9-b21a-ed57fd1c9dff.png)


##  Let's practice!

### Semi join

Let's say you are interested in identifying languages spoken in the Middle East. The languages table contains information about languages and countries, but it does not tell you what region the countries belong to. You can build up a semi join by filtering the countries table by a particular region, and then using this to further filter the languages table.

Select country code as a single field from the countries table, filtering for countries in the 'Middle East' region.
```
Select code
FROM countries
WHERE region = 'Middle East';
```
![image](https://user-images.githubusercontent.com/118057504/234660620-00372d61-1a15-4d6e-8254-22d1a4b29863.png)

Write a second query to SELECT the name of each unique language appearing in the languages table; do not use column aliases here.
Order the result set by name in ascending order.
```
SELECT DISTINCT(name)
FROM languages
ORDER BY name ASC;
```
![image](https://user-images.githubusercontent.com/118057504/234660831-33da8739-6fc8-44c1-9643-13e6b984ad7f.png)

Create a semi join out of the two queries you've written, which filters unique languages returned in the first query for only those languages spoken in the 'Middle East'.
```
SELECT DISTINCT name
FROM languages
WHERE code IN
    (SELECT code
    FROM countries
    WHERE region = 'Middle East')
ORDER BY name;
```
![image](https://user-images.githubusercontent.com/118057504/234661065-7d159a1b-92fa-4ef5-b761-731ef181e9ef.png)

### Diagnosing problems using anti join
 The anti join is a related and powerful joining tool. It can be particularly useful for identifying whether an incorrect number of records appears in a join.

Say you are interested in identifying currencies of Oceanian countries. You have written the following INNER JOIN, which returns 15 records. Now, you want to ensure that all Oceanian countries from the countries table are included in this result. You'll do this in the first step.
```
SELECT c1.code, name, basic_unit AS currency
FROM countries AS c1
INNER JOIN currencies AS c2
ON c1.code = c2.code
WHERE c1.continent = 'Oceania';
```
![image](https://user-images.githubusercontent.com/118057504/234662482-9dbc85c2-692e-451b-b1cf-56bf3b2d1233.png)
 Tuvalu has two currencies, and therefore shows up twice in the INNER JOIN? This is why the INNER JOIN returned 15 rather than 14 results.
 

If there are any Oceanian countries excluded in this INNER JOIN, you want to return the names of these countries. You'll write an anti join to this in the second step!
Begin by writing a query to return the code and name (in order, not aliased) for all countries in the continent of Oceania from the countries table.
Observe the number of records returned and compare this with the provided INNER JOIN, which returns 15 records.
```
SELECT code, name
FROM countries
WHERE continent = 'Oceania';
```
![image](https://user-images.githubusercontent.com/118057504/234662826-3fbb4268-d01a-47fd-a424-0e7ee4300a62.png)

Now, build on your query to complete your anti join, by adding an additional filter to return every country code that is not included in the currencies table.

```
SELECT code, name
FROM countries
WHERE continent = 'Oceania'
-- Filter for countries not included in the bracketed subquery
  AND code NOT IN
    (SELECT code
    FROM currencies);
```
![image](https://user-images.githubusercontent.com/118057504/234663345-58d077f8-d746-4924-8ae6-bfc58e6f639b.png)

# Subqueries inside WHERE and SELECT

##  1. Subqueries inside WHERE and SELECT

In this lesson, we'll dive deeper into embedding queries within queries. We'll revisit subqueries within a WHERE clause, and then go on to subqueries within a SELECT statement.

##  2. Syntax for subqueries inside WHERE

The semi joins and anti joins we have seen so far involve subqueries inside WHERE. The WHERE clause is the most common place for subqueries, because filtering data is one of the most common data manipulation tasks. Let's revisit this with some generic syntax and understand the nuances of this type of subquery. Have a look at the query shown. Recall that the WHERE IN clause enables us to provide a list of values to filter on.

##  3. Syntax for subqueries inside WHERE

As we have seen in the lesson on semi joins, arguments to the IN operator are not limited to lists typed out by us. We can include a SQL subquery as an argument for the IN operator, provided the result of the subquery is of the same data type as the field we are filtering on.

##  4. Syntax for subqueries inside WHERE

The query shown will only work if some_field is of the same data type as some_numeric_field, because the result of the subquery will be a numeric field. Subqueries inside WHERE can be from the same table or from a different table, and here, the subquery is from a different table.

##  5. Subqueries inside SELECT

The second most common type of subquery is inside a SELECT clause. Say we want to count the number of monarchs listed in the monarchs table for each continent in the states table. Again, let's build up our solution, step by step. The query shown selects each of the five continents in the states table, and the result is displayed.

##  6. Subqueries inside SELECT

In earlier courses we have seen the use of GROUP BY to COUNT data by a group. However, since our monarchs data lives in a different table than the states table, this would involve a careful join before the GROUP BY. Let's look at how to do this with a subquery instead. Our subquery requires two things. First, it needs to COUNT all monarchs. Second, it needs a WHERE statement matching the continent fields in the two tables. This subquery follows the selection of DISTINCT continents, and will therefore count all monarchs within them in the SELECT statement. A subquery inside a SELECT statement like this requires an alias, like monarch_count here. Magic!

##  Let's practice!


