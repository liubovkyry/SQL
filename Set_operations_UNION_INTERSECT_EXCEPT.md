
##  Set theory for SQL Joins
 
 In this chapter, we explore a new way of joining data: set operations. Along the way, we'll highlight key differences between set operations and the joins we have seen so far.

##  Venn diagrams and set theory

SQL has three main set operations, UNION, INTERSECT and EXCEPT. The Venn diagrams shown visualize the differences between them. We can think of each circle as representing a table. The green parts represent what is included after the set operation is performed on each pair of tables.
![image](https://user-images.githubusercontent.com/118057504/234401047-568b8c45-b6e6-4119-be6f-48927dc9f2a8.png)

## 3. Venn diagrams and set theory

In this lesson, we will cover UNION and UNION ALL. Let's take a closer look at how they work.

#  UNION diagram

In SQL, the UNION operator takes two tables as input, and returns all records from both tables. The diagram shows two tables: left and right, and performing a UNION returns all records in each table. If two records are identical, UNION only returns them once. To illustrate this, the first two records of the right table have been faded out. Our result has seven records.
![image](https://user-images.githubusercontent.com/118057504/234401301-beb6a814-991d-4c16-9365-4bcf95505001.png)

## 1. UNION ALL diagram

In SQL, there is a further operator for unions called UNION ALL. In contrast to UNION, given the same two tables, UNION ALL will include duplicate records. Therefore, performing UNION ALL on this data will return nine records, whereas UNION only returned seven. No records have been faded out.
![image](https://user-images.githubusercontent.com/118057504/234401498-9423fa72-a061-4be1-a122-9e306bf327d6.png)

## 2. UNION and UNION ALL syntax

The syntax for performing all the set operations in this chapter is highly similar. We perform a SELECT statement on our first table, a SELECT statement on our second table, and specify a set operation (in this example, either UNION or UNION ALL) between them. Note that set operations do not require a field to join ON. This is because they do not do quite the same thing as join operations we covered in earlier chapters. Rather than comparing and merging tables on the left and right, they stack fields on top of one another.
![image](https://user-images.githubusercontent.com/118057504/234401706-d162a968-1ec3-492b-ad7d-1d751f4cfed8.png)

## 3. UNION and UNION ALL syntax

For all set operations, the number of selected columns and their respective data types must be identical. For instance, we can't stack a number field on top of a character field.

## 4. UNION and UNION ALL syntax

The result will only use field names (or aliases, if used) of the first SELECT statement in the query.
![image](https://user-images.githubusercontent.com/118057504/234403934-b2b3f364-f198-4760-b799-af837e0db465.png)

## 5. To the monarchs table

We now return to our world leaders example. In this example, we use one more table from our leaders database: the monarchs table. Of course, this table is only a sample of monarchs, and not a comprehensive list of monarchs across the world.
![image](https://user-images.githubusercontent.com/118057504/234404109-2042663b-5d7f-4e94-912f-d166fac71f8d.png)

## 6. Prime ministers, meet the monarchs

We can use UNION on the monarchs and prime_ministers tables to show all the different prime ministers and monarchs in these two tables. Note that the monarch field has been aliased as 'leader'. As we have learned, the monarch and prime_minister fields will be combined under the leader field, even though we only aliased the monarch field. We'll LIMIT to the first 10 results so that they fit on our screen.
![image](https://user-images.githubusercontent.com/118057504/234621325-0b7404a8-b8de-448d-a647-a5cdb56defbf.png)


## 7. After the UNION

Our UNION returns the top 10 monarchs, prime_ministers and their corresponding countries. Does something stand out here? Recall that the monarchs for Oman and Brunei are also prime ministers. UNION only lists them once each, because they are the same person. Norway is listed twice, however, because its monarch and prime minister are different people.

![image](https://user-images.githubusercontent.com/118057504/234621757-8f3a4119-3dad-47f2-8577-ab510aae58d6.png)


## 8. UNION ALL with the leaders

This is where a UNION ALL is helpful! A UNION ALL will tell us if there are any monarchs who also act as prime ministers. The syntax for this is shown.
![image](https://user-images.githubusercontent.com/118057504/234622332-e4456c78-80bd-435b-9184-f726038150b3.png)

## 9. UNION ALL result

After UNION ALL, Brunei and Oman are now listed twice.
![image](https://user-images.githubusercontent.com/118057504/234622492-1d6a3fbf-9355-4c2d-a629-94480c42472f.png)


##  Let's practice

### Comparing global economies

In this exercise, you have two tables, economies2015 and economies2019, available to you under the tabs in the console. You'll perform a set operation to stack all records in these two tables on top of each other,<i> excluding duplicates.</i>

When drafting queries containing set operations, it is often helpful to write the queries on either side of the operation first, and then call the set operator. The instructions are ordered accordingly.

Begin your query by selecting all fields from economies2015.
Create a second query that selects all fields from economies2019.
Perform a set operation to combine the two queries you just created, ensuring you do not return duplicates.

```
-- Select all fields from economies2015
SELECT *
FROM economies2015   
-- Set operation
UNION
-- Select all fields from economies2019
SELECT *
FROM economies2019
ORDER BY code, year;
```
![image](https://user-images.githubusercontent.com/118057504/234624173-ce7a79ea-6ae6-41ef-9141-7180fcb04ce3.png)

### Comparing two set operations
```
SELECT code AS country_code, year
FROM economies
UNION
SELECT country_code, year
FROM populations
ORDER by country_code, year;
```
![image](https://user-images.githubusercontent.com/118057504/234624776-d8ca83fa-9a1f-4a45-abce-1be92a552e1e.png)

```
SELECT code, year
FROM economies
-- Set theory clause
UNION ALL
SELECT country_code, year
FROM populations
ORDER BY code, year;
```

![image](https://user-images.githubusercontent.com/118057504/234624683-5bf891b7-de5e-43db-8986-2aa444f0d448.png)

# At the INTERSECT

## 1. INTERSECT Venn diagram

INTERSECT takes two tables as input, and returns only the records that exist in both tables.

## 2. INTERSECT diagram

In the diagram shown, we have two tables, left_table and right_table. The result of performing INTERSECT on these tables is only the records common to both tables: the first record. All records that are not of interest to the INTERSECT operation are faded out.

![image](https://user-images.githubusercontent.com/118057504/234633689-0bc2d5a6-c6a4-4da5-baab-1844023b2650.png)

## 3. INTERSECT syntax

The syntax for this set operation is very similar to that of UNION and UNION ALL. We perform a SELECT statement on our first table, a SELECT statement on our second table, and specify our set operator between them.

![image](https://user-images.githubusercontent.com/118057504/234633940-ef906deb-628c-4cdd-a368-5ebf08f20506.png)

## 4. INTERSECT vs. INNER JOIN on two columns

Let's compare INTERSECT to performing an INNER JOIN on two fields with identical field names. Similar to UNION, for a record to be returned, INTERSECT requires all fields to match, since in set operations we do not specify any fields to match on. This is also why it requires the left and right table to have the same number of columns in order to compare records. In the figure shown, only records where both columns match are returned. In INNER JOIN, similar to INTERSECT, only results where both fields match are returned. INNER JOIN will return duplicate values, whereas INTERSECT will only return common records once. As we know from earlier lessons, INNER JOIN will add more columns to the result set.

![image](https://user-images.githubusercontent.com/118057504/234634202-469e0de1-0ad4-47a2-bd83-7aa806c81a87.png)

## 5. Countries with prime ministers and presidents

Let's have a look at how we can use INTERSECT to determine all countries that have both a prime minister and a president. Each SELECT statement in our query has the same number of columns, of identical data types, in order for the set operation to be performed. The result of the query is the four countries with both a prime minister and a president in our leaders database. As with UNION, the result set uses the field names provided for the left table, whether aliased or not.

![image](https://user-images.githubusercontent.com/118057504/234634461-0d94f7d4-b61a-4c8b-98a3-802eb2946863.png)

## 6. INTERSECT on two fields

Next, let's think about what would happen if we selected two columns, country and prime_minister, instead of just country, as in our previous example. The code shown does just that. What will the result of this query be? Will this also give us the names of countries that have both a prime minister and a president? The actual result is an empty table. Why is that? As we saw in our INTERSECT diagrams, INTERSECT requires data from both fields in the left table to match their corresponding fields in the right table. The search did not find any countries where the prime minister and president share the same name.
![image](https://user-images.githubusercontent.com/118057504/234634765-f4d69da6-4f89-4aa6-ada4-c054dbdcf9e9.png)


## 7. Countries with prime ministers and monarchs

However, recall that there are monarchs in our database who also act as prime_ministers. In the example shown, performing an INTERSECT on the prime_ministers and monarchs tables using both country and leader name does not return an empty result.
![image](https://user-images.githubusercontent.com/118057504/234635990-7d37dd55-7b70-42e7-b7ab-de84b5a92730.png)

## Let's practice

Let's say you are interested in those countries that share names with cities. Use this task as an opportunity to show off your knowledge of set theory in SQL!
```
SELECT name
FROM countries
INTERSECT
SELECT name
FROM cities
```
![image](https://user-images.githubusercontent.com/118057504/234639098-2ba68145-c4cf-41d9-85f4-c91055cd5b45.png)



# EXCEPT

## 1. EXCEPT Venn diagram

EXCEPT allows us to identify the records that are present in one table, but not the other. More specifically, <i>it retains only records from the left table that are not present in the right table.</i>

## 2. EXCEPT diagram

The diagram shown illustrates the workings of the EXCEPT operation. All records that are not of interest to the EXCEPT operation are faded out. Only the last two records of the left_table are returned. Note that while the id 4 does exist in the right_table, the whole record does not match, which is why the last record of left_table is not faded out.
![image](https://user-images.githubusercontent.com/118057504/234639935-ae469c07-3ef1-41cd-8a1c-089f357140fd.png)


## 3. EXCEPT syntax

We saw earlier that there are some monarchs that also act as the prime minister for their country. Let's say we were interested in monarchs that do NOT also hold the title of prime minister. The EXCEPT clause is really handy for this! The SQL code shown selects the monarch and country field from monarchs and then looks for common entries in the prime_minister and country fields in the prime_ministers table, looking to exclude those entries. In the result, we see only the three monarchs from our leaders database who do not also serve the role of prime minister.
![image](https://user-images.githubusercontent.com/118057504/234640383-4d7e2e56-2d91-4b8c-8c5a-3abb053416d6.png)

## Let's practice!
Just as you were able to leverage INTERSECT to find the names of cities with the same names as countries, you can also do the reverse, using EXCEPT.

In this exercise, you will find the names of cities that <b>do not</b> have the same names as their countries.

```
-- Return all cities that do not have the same name as a country
SELECT name
FROM cities
EXCEPT
SELECT name
FROM countries
ORDER BY name;
```
![image](https://user-images.githubusercontent.com/118057504/234641221-923ee85f-540d-4ae4-98e8-f64bec428c19.png)

