## Casting with CAST()

Values can be converted temporarily from one type to another through a process called casting. When you cast a column as a different type, the data is converted to the new type only for the current query. To change a value's type, use the cast function, first, specify the value you want to cast. This can be a single value or the name of a column. Then use the keyword AS. Finally, specify the name of the type you want to convert the data to. Here's an example of casting the single numeric value 3-point-7 as an integer. Casting from numeric to integer rounds the value to the nearest integer, which is 4. To convert the type of an entire column, enter the name of the column as the value. Here, a column called total is converted to type integer. We need a from clause to specify which table the column comes from.

![image](https://github.com/liubovkyry/SQL/assets/118057504/c3b221e4-ae22-4c65-a5e1-eaab2fd499fc)

## Casting with ::

There's an alternate notation for casting values: a double colon. It does the same thing as the cast function, but it's more compact. Put the value to convert before the double colon and the type to cast it as after the double colon. The examples here are the same as those on the previous slide, except with the double colon notation instead of the cast function.
![image](https://github.com/liubovkyry/SQL/assets/118057504/298fc21f-27cd-4982-a8fc-75229b486fde)

```
SELECT '3.2'::numeric,
       '-123'::numeric,
       '1e3'::numeric,
       '1e-3'::numeric,
       '02314'::numeric,
       '0002'::numeric;
```

![image](https://github.com/liubovkyry/SQL/assets/118057504/5970b9b7-fd0e-4290-b325-3c1b0e26e500)

```
-- Select the original value
SELECT profits_change, 
	   -- Cast profits_change
       CAST(profits_change AS integer) AS profits_change_int
  FROM fortune500;
 ```

![image](https://github.com/liubovkyry/SQL/assets/118057504/dd374061-6377-4a6a-b9e1-c15a3b1e0a50)


 


