
## 1. Self joins

We'll now dive into a special kind of join, where a table is joined with itself. These types of joins are called self joins.

## 2. Self joins

Joining a table to itself may seem like an unusual thing to do. Why would we want to do that? Self joins are used to compare values from part of a table to other values from within the same table. Recall the prime_ministers table from earlier. Suppose all prime ministers are convening in summits on their own continents.<i> We want to create a new table showing all countries in the same continent as pairs.</i>
![image](https://user-images.githubusercontent.com/118057504/234260472-ce3282ac-23a2-4bc3-b494-7ccbe560ccea.png)


## 3. Prime minister, meet prime minister

<b>Self joins don't have dedicated syntax</b> as other joins we have seen do. We can't just write SELF JOIN in SQL code, for example. In addition, aliasing is required for a self join.
Let's look at a chunk of INNER JOIN code using the prime_ministers table. The country column is selected twice, and so is the continent column. The prime_ministers table is on both the left and the right of the JOIN, making this both a self join and an INNER JOIN! The vital step here is setting the joining fields which we use to match the table to itself. For each country, we will find multiple matched countries in the right table, since we are joining on continent. Each of these matched countries will be returned as pairs. Since this query will return several records, we use LIMIT to return only the first 10 records.
![image](https://user-images.githubusercontent.com/118057504/234261002-cf4c984e-8d94-47d1-bea2-4dbe5fe0b688.png)

![image](https://user-images.githubusercontent.com/118057504/234261333-d50a1192-b018-4195-94de-dc04128bd026.png)

## 4. Prime minister, meet prime minister

The results are a pairing of each country with every other country in the same continent. However, note that our join also paired countries with themselves, since they too are in the same continent as themselves. We don't want to include these, since a leader from Portugal does not need to meet with themselves, for example. Let's fix this.
![image](https://user-images.githubusercontent.com/118057504/234261588-1d4f90e0-12c8-4b31-8178-aea454ed5bfd.png)


## 5. Prime minister, meet prime minister

Recall the use of the AND clause to ensure multiple conditions are met in the ON clause. In our second condition, we use the not equal to operator to exclude records where the p1-dot-country and p2-dot-country fields are identical.

## 6. The self joined prime ministers table

Here's a look at our final table, showing combinations of countries in our database that are in the same continent, but excluding records where the two country fields are the same.

![image](https://user-images.githubusercontent.com/118057504/234261665-f5ce7209-c9b1-4932-aedb-f1ba29ccdd32.png)

## 7. Let's practice.

Self joins are very useful for comparing data from one part of a table with another part of the same table.

Suppose you are interested in finding out how much the populations for each country changed from 2010 to 2015. You can visualize this change by performing a self join.

In this exercise, you'll work to answer this question by joining the populations table with itself. Recall that, with self joins, tables must be aliased. Use this as an opportunity to practice your aliasing!

Since you'll be joining the populations table to itself, you can alias populations first as p1 and again as p2. This is good practice whenever you are aliasing tables with the same first letter.

Perform an inner join of populations with itself ON country_code, aliased p1 and p2 respectively.
Select the country_code from p1 and the size field from both p1 and p2, aliasing p1.size as size2010 and p2.size as size2015 (in that order).
```
-- Select aliased fields from populations as p1
SELECT p1.country_code,
p1.size AS size2010,
p2.size AS size2015

-- Join populations as p1 to itself, alias as p2, on country code
FROM populations AS p1
INNER JOIN populations AS p2
ON p1.country_code = p2.country_code;
```

![image](https://user-images.githubusercontent.com/118057504/234262767-6bfa2416-956e-4dbd-8c28-fffad6daf945.png)

Since you want to compare records from 2010 and 2015, eliminate unwanted records by extending the WHERE statement to include only records where the p1.year matches p2.year - 5.
<b>Hint<b/>
Add p1.year = p2.year - 5 as an additional condition in the WHERE clause using the AND operator.
  
```
SELECT 
	p1.country_code, 
    p1.size AS size2010, 
    p2.size AS size2015
FROM populations AS p1
INNER JOIN populations AS p2
ON p1.country_code = p2.country_code
WHERE p1.year = 2010
-- Filter such that p1.year is always five years before p2.year
 AND p1.year = p2.year - 5;
  ```
  
  ![image](https://user-images.githubusercontent.com/118057504/234263168-296cf22d-0437-443a-865d-e3e34692f634.png)

  

