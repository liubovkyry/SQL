
## 1. Set theory for SQL Joins
 
 In this chapter, we explore a new way of joining data: set operations. Along the way, we'll highlight key differences between set operations and the joins we have seen so far.

## 2. Venn diagrams and set theory

SQL has three main set operations, UNION, INTERSECT and EXCEPT. The Venn diagrams shown visualize the differences between them. We can think of each circle as representing a table. The green parts represent what is included after the set operation is performed on each pair of tables.
![image](https://user-images.githubusercontent.com/118057504/234401047-568b8c45-b6e6-4119-be6f-48927dc9f2a8.png)

## 3. Venn diagrams and set theory

In this lesson, we will cover UNION and UNION ALL. Let's take a closer look at how they work.

## 4. UNION diagram

In SQL, the UNION operator takes two tables as input, and returns all records from both tables. The diagram shows two tables: left and right, and performing a UNION returns all records in each table. If two records are identical, UNION only returns them once. To illustrate this, the first two records of the right table have been faded out. Our result has seven records.
![image](https://user-images.githubusercontent.com/118057504/234401301-beb6a814-991d-4c16-9365-4bcf95505001.png)

## 5. UNION ALL diagram

In SQL, there is a further operator for unions called UNION ALL. In contrast to UNION, given the same two tables, UNION ALL will include duplicate records. Therefore, performing UNION ALL on this data will return nine records, whereas UNION only returned seven. No records have been faded out.
![image](https://user-images.githubusercontent.com/118057504/234401498-9423fa72-a061-4be1-a122-9e306bf327d6.png)

## 6. UNION and UNION ALL syntax

The syntax for performing all the set operations in this chapter is highly similar. We perform a SELECT statement on our first table, a SELECT statement on our second table, and specify a set operation (in this example, either UNION or UNION ALL) between them. Note that set operations do not require a field to join ON. This is because they do not do quite the same thing as join operations we covered in earlier chapters. Rather than comparing and merging tables on the left and right, they stack fields on top of one another.
![image](https://user-images.githubusercontent.com/118057504/234401706-d162a968-1ec3-492b-ad7d-1d751f4cfed8.png)

## 7. UNION and UNION ALL syntax

For all set operations, the number of selected columns and their respective data types must be identical. For instance, we can't stack a number field on top of a character field.

## 8. UNION and UNION ALL syntax

The result will only use field names (or aliases, if used) of the first SELECT statement in the query.
![image](https://user-images.githubusercontent.com/118057504/234403934-b2b3f364-f198-4760-b799-af837e0db465.png)

## 9. To the monarchs table

We now return to our world leaders example. In this example, we use one more table from our leaders database: the monarchs table. Of course, this table is only a sample of monarchs, and not a comprehensive list of monarchs across the world.
![image](https://user-images.githubusercontent.com/118057504/234404109-2042663b-5d7f-4e94-912f-d166fac71f8d.png)

## 10. Prime ministers, meet the monarchs

We can use UNION on the monarchs and prime_ministers tables to show all the different prime ministers and monarchs in these two tables. Note that the monarch field has been aliased as 'leader'. As we have learned, the monarch and prime_minister fields will be combined under the leader field, even though we only aliased the monarch field. We'll LIMIT to the first 10 results so that they fit on our screen.
![image](https://user-images.githubusercontent.com/118057504/234621325-0b7404a8-b8de-448d-a647-a5cdb56defbf.png)


## 11. After the UNION

Our UNION returns the top 10 monarchs, prime_ministers and their corresponding countries. Does something stand out here? Recall that the monarchs for Oman and Brunei are also prime ministers. UNION only lists them once each, because they are the same person. Norway is listed twice, however, because its monarch and prime minister are different people.

![image](https://user-images.githubusercontent.com/118057504/234621757-8f3a4119-3dad-47f2-8577-ab510aae58d6.png)


## 12. UNION ALL with the leaders

This is where a UNION ALL is helpful! A UNION ALL will tell us if there are any monarchs who also act as prime ministers. The syntax for this is shown.
![image](https://user-images.githubusercontent.com/118057504/234622332-e4456c78-80bd-435b-9184-f726038150b3.png)

## 13. UNION ALL result

After UNION ALL, Brunei and Oman are now listed twice.
![image](https://user-images.githubusercontent.com/118057504/234622492-1d6a3fbf-9355-4c2d-a629-94480c42472f.png)


## 14. Let's practice!

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

