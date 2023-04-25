
## 1. Self joins

We'll now dive into a special kind of join, where a table is joined with itself. These types of joins are called self joins.

## 2. Self joins

Joining a table to itself may seem like an unusual thing to do. Why would we want to do that? Self joins are used to compare values from part of a table to other values from within the same table. Recall the prime_ministers table from earlier. Suppose all prime ministers are convening in summits on their own continents. We want to create a new table showing all countries in the same continent as pairs.

## 3. Prime minister, meet prime minister

Self joins don't have dedicated syntax as other joins we have seen do. We can't just write SELF JOIN in SQL code, for example. In addition, aliasing is required for a self join. Let's look at a chunk of INNER JOIN code using the prime_ministers table. The country column is selected twice, and so is the continent column. The prime_ministers table is on both the left and the right of the JOIN, making this both a self join and an INNER JOIN! The vital step here is setting the joining fields which we use to match the table to itself. For each country, we will find multiple matched countries in the right table, since we are joining on continent. Each of these matched countries will be returned as pairs. Since this query will return several records, we use LIMIT to return only the first 10 records.

## 4. Prime minister, meet prime minister

The results are a pairing of each country with every other country in the same continent. However, note that our join also paired countries with themselves, since they too are in the same continent as themselves. We don't want to include these, since a leader from Portugal does not need to meet with themselves, for example. Let's fix this.

## 5. Prime minister, meet prime minister

Recall the use of the AND clause to ensure multiple conditions are met in the ON clause. In our second condition, we use the not equal to operator to exclude records where the p1-dot-country and p2-dot-country fields are identical.

## 6. The self joined prime ministers table

Here's a look at our final table, showing combinations of countries in our database that are in the same continent, but excluding records where the two country fields are the same.

## 7. Let's practice.
