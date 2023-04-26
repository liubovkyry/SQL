
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






