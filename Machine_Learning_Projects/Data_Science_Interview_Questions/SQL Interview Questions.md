# SQL Interview Questions

## T-SQL 
***

### 1. What are the common commands in SQL?
- Select
- Insert
- Delete
- Update
- Where

### 2. What are DDL (Data Definition Language) commands?
- Create
- Alter 
- Drop

### 3. What are DML (Data Manipulation Language) commands?
- Select
- Insert
- Delete
- Update

### 4. What is the query order of execution?
From & Joins -> Where -> Group By -> Having -> Select -> Distinct -> Order By -> Limit/Offset

### 5. Explain Where clause?
__Where__ clause is used to filter records. It is used in combination to:
- Equality filter (=, <>, <=, >= ``` Where Gender = 'M' ```, ``` Where class <> 2 ```)
- Basic Comparison (<,> ``` Where score < 60```)
- Logical Comparison (AND - ```Where Type = 'A' AND Score > 75```, OR - ```Where Type = 'A' OR Type = 'B' ```, IN - ``` Where FirstName IN ('Tom', 'Dick', 'Harry')```, BETWEEN - ```Where Score Between 40 AND 80``` )
- String Comparison (Like - ```Where JobTitle Like '%Data%' ```, NOT Like - ```Where Name NOT Like '_ary' ```, wild cards - '%')
- NULL Comparison (```Where size is NULL```, ```Where Gender is NOT NULL```)

__Examples__

Use the web_events table to find all information regarding individuals who were contacted via any method except using organic or adwords methods.
```
SELECT *
FROM web_events
WHERE channel NOT IN ('organic', 'adwords')
```
Use the accounts table to find All the companies whose names do not start with 'C'.
```
SELECT *
FROM accounts
WHERE name NOT LIKE 'C%'
```
All companies whose names do not contain the string 'one' somewhere in the name.
```
SELECT *
FROM accounts
WHERE name NOT LIKE '%one%'
```
All companies whose names do not end with 's'.
```
SELECT *
FROM accounts
WHERE name NOT LIKE '%s'
```
Using the accounts table find all the companies whose names do not start with 'C' and end with 's'.
```
SELECT name 
FROM accounts
WHERE name NOT LIKE 'C%' AND name LIKE '%s';
```
Use the web_events table to find all information regarding individuals who were contacted via organic or adwords and started their account at any point in 2016 sorted from newest to oldest.
```
SELECT *
FROM web_events
WHERE channel IN ('organic', 'adwords') AND occurred_at BETWEEN '2016-01-01' AND '2017-01-01'
ORDER BY occurred_at DESC;
```
You will notice that using BETWEEN is tricky for dates! While BETWEEN is generally inclusive of endpoints, it assumes the time is at 00:00:00 (i.e. midnight) for dates. This is the reason why we set the right-side endpoint of the period at '2017-01-01'

Write a query that returns a list of orders where the standard_qty is zero and either the gloss_qty or poster_qty is over 1000.
```
SELECT *
FROM orders
WHERE standard_qty = 0 AND (gloss_qty > 1000 OR poster_qty > 1000);
```
Find all the company names that start with a 'C' or 'W', and the primary contact contains 'ana' or 'Ana', but it doesn't contain 'eana'.
```
SELECT *
FROM accounts
WHERE (name LIKE 'C%' OR name LIKE 'W%') AND ((primary_poc LIKE '%ana%' OR primary_poc LIKE '%Ana%') AND (primary_poc NOT LIKE '%eana%'));
```

### 6. What are aggregate functions?
__Aggregate functions__ are used to calculate values in a T-SQL query and returns a single result value. We can use multiple aggregate functions at once as well.
- Count()
- Avg()
- Max()
- Min()
- Sum()

```
Select COUNT(DISTINCT JobTitle) as 'Num_JobTitles' 
FROM Employee
```
All the aggregate functions ignore null values and are operated column wise. 
```
SELECT DISTINCT column1, column2, column3
FROM table1; 
```
__which would return the unique or DISTINCT or non-duplicate rows across all three columns__. 
- __Distinct__ can slow the query

### 7. What is group by clause?
__Group By__ clause is used in combination with aggregate function to group returned query output.
```
select city, count(*) as 'total_Count_city'
From Address
Group By City
```
The query will return the count of each distinct city. 

- Group By are used to aggregate unique values which can be a single column or combination of multiple columns resulting in uniqueness
- Any column in the SELECT statement that is not within an aggregator must be in the GROUP BY clause
- The order of column names in GROUP BY clause doesn’t matter, the results will be the same regardless
- As with ORDER BY, we can substitute numbers for column names in the GROUP BY clause


__Note__: To find the unique records in a table we can use group by instead of distinct. 

### 8. Explain order by clause ?
__Order by__ clause is used to sort the final output result data set. It always comes at the end of slect statement. We can use column alias and column numbers in order by clause. When we are dealing with multiple columns we can sort both of them in ascending and descending order.   

```
select * from table_name ORDER BY col1 ASC, col2 DESC;
```

### 9. what is the difference between having and where clause? 
__WHERE__ and __HAVING__ both filters out records based on one or more conditions. The difference are:
- WHERE clause can only be applied on a static non-aggregated column whereas HAVING on aggregated columns
- HAVING clause always comes after WHERE clause
- WHERE clause can be used on select, insert and update statements but having is only used on select
- Aggregate functions cannot be used in the WHERE clause, unless it is in a sub query contained in a HAVING clause, whereas, aggregate functions can be used in Having clause

How many accounts spent more than 30,000 usd total across all orders?
```
SELECT a.id, a.name, SUM(o.total_amt_usd) total_spent
FROM accounts a
JOIN orders o
ON a.id = o.account_id
GROUP BY a.id, a.name
HAVING SUM(o.total_amt_usd) > 30000
ORDER BY total_spent;
```
__Note__: Having clause acts like where clause when there is no group by clause.

### 10. Why do we use joins?
We use joins to combine rows from two or more related tables and present as one output result.

### 11. What are different types of joins?
- Inner Join
- Left Join
- Right Join
- Outer Join
- Self Join
- Cross join

### 12. How does inner join work?
It is used to select rows where participating tables have identical columns.It combines the rows from two tables and create a result set containing only such records that are present in both the tables based on the joining condition (predicate).

Inner join returns rows when there is at least one match in both tables If none of the record matches between two tables, then INNER JOIN will return a NULL set.

```
select A.colA, B.colA
FROM Table A 
inner join Table B
on A.colA = B.colA
```

### 13. Difference between inner and left join?

### 14. How does outer join work?
Outer join will return matching rows from both tables as well as any unmatched rows from one or both the tables.

```
select A.colA, B.colA
FROM Table A 
outer join Table B
on A.colA = B.colA
```
- Say for example we have a scenario where each account who has a sales rep and each sales rep that has an account (all of the columns in these returned rows will be full)
- But also each account that does not have a sales rep and each sales rep that does not have an account (some of the columns in these returned rows will be empty)

```
SELECT *
FROM accounts
FULL JOIN sales_reps ON accounts.sales_rep_id = sales_reps.id
WHERE accounts.sales_rep_id IS NULL OR sales_reps.id IS NULL
```
### 15. Give a scenario where inner join works the same as outer join?

### 16. What is a self join?
__Self Join__ is the act of joining one table with itself. It is often very useful to convert a hierarchical structure into a flat structure.

Say for example we have employee table keeping the manager ID of each employee in the same row as that of the employee. This is an example of how a hierarchy (in this employee-manager hierarchy) is stored in the RDBMS table. Now, suppose if we need to print out the names of the manager of each employee right beside the employee, we can use self join. See the example below:

```
SELECT e.name EMPLOYEE, m.name MANAGER
FROM EMPLOYEE e, EMPLOYEE m
WHERE e.mgr_id = m.id (+)
```

|EMPLOYEE	|MANAGER|
|---------|-------|
|Pete	|Hash|
|Darl	|Hash|
|Inno	|Hash|
|Robo	|Hash|
|Tomiti	|Robo|
|Anno	|Robo|
|Privy	|Robo|
|Meme	|Pete|
|Bhuti|Tomiti|
|Hash	|      |

The (+) sign on the m id side of the predicate indicates that m.id is the outer table here.

### 17. Advantages and diadvantages of self join? 



### 18. What is cross join? 
Say we have two tables shapes and colors.

|__Shapes__|
|----------|
|Circle|
|Square|

|__Colors__|
|----------|
|Red|
|Blue|

What if we wanted to determine all the possible combinations of shapes and colors? We can accomplish this using a cross join as follows:
```
SELECT * FROM Shapes CROSS JOIN Colors
```
|__Shapes__|__Colors__|
|----------|----------|
|Circle|Red|
|Square|Red|
|Circle|Blue|
|Square|Blue|

we can also use an inner join in SQL to mimic a cross join by doing something like this: 
```
SELECT * FROM Table_A INNER JOIN Table_B ON 1 = 1
or
SELECT * FROM Table_A INNER JOIN Table_B ON 'Common_field' = 'Common_field'
```
### 19. What is the difference between JOIN and UNION?
While joins allow us to combine data in a horizontal fashion, unions allow us to combine data in a vertical fashion.
SQL JOIN allows us to “lookup” records on other table based on the given conditions between two tables. UNION operation allows us to add 2 similar data sets to create resulting data set that contains all the data from the source data sets. 

- Union does not require any condition for joining
- For union operation to work we need to have the same number of columns, same column names, same column data types, etc

If column names are different in one table then we can use alias to perform union operation(Values and data types should be the same)
```
SELECT Order ID, Order Date, Product ID, Sales FROM Orders_West
UNION
SELECT ID AS Order ID, Date AS Order Date, Product AS Product ID, Amount AS Sales FROM Orders_South
```

### 20. What is the difference between union and union all?
__UNION__ and __UNION ALL__ both combine two structurally similar data sets, but UNION operation returns only the unique records from the resulting data set whereas UNION ALL will return all the rows, even if one or more rows are duplicated to each other. 

```
SELECT * FROM EMPLOYEE WHERE ID = 5
UNION ALL
SELECT * FROM EMPLOYEE WHERE ID = 5
``` 
|ID	|MGR_ID	| DEPT_ID |	NAME |	SAL	|DOJ|
|---|-------|---------|-------|-----|---|
|5.0 |	2.0	|2.0	|Anno|	80.0	|01-Feb-2012|
|5.0	|2.0	|2.0	|Anno	|80.0	|01-Feb-2012|

```
SELECT * FROM EMPLOYEE WHERE ID = 5
UNION 
SELECT * FROM EMPLOYEE WHERE ID = 5
```

|ID	|MGR_ID	|DEPT_ID	|NAME	|SAL	|DOJ|
|----|------|--------|-----|-----|---|
|5.0	|2.0	|2.0	|Anno	|80.0	|01-Feb-2012|

Because __UNION ALL__ does not attempt to eliminate duplicates, the database has much less work to do to retrieve your result set. Thus, UNION ALL queries are significantly more performant than regular unions.

### 21. What is the difference among UNION, MINUS and INTERSECT?
__UNION__ combines the results from 2 tables and eliminates duplicate records from the result set.

__MINUS__ operator when used between 2 tables, gives us all the rows from the first table except the rows which are present in the second table.

__INTERSECT__ operator returns us only the matching or common rows between 2 result sets.

To understand these operators, let’s see some examples. We will use two different queries to extract data from our emp table and then we will perform UNION, MINUS and INTERSECT operations on these two sets of data.

UNION
```
SELECT * FROM EMPLOYEE WHERE ID = 5
UNION 
SELECT * FROM EMPLOYEE WHERE ID = 6
```

|ID	|MGR_ID	|DEPT_ID	|NAME	|SAL	|DOJ|
|----|-------|-------|------|----|---|
|5	|2	|2.0	|Anno	|80.0	|01-Feb-2012|
|6	|2	|2.0	|Darl	|80.0	|11-Feb-2012|

MINUS
```
SELECT * FROM EMPLOYEE
MINUS
SELECT * FROM EMPLOYEE WHERE ID > 2
```

|ID	|MGR_ID	|DEPT_ID	|NAME	|SAL	|DOJ|
|---|-------|--------|------|----|---|
|1		|2	|Hash	|100.0	|01-Jan-2012|
|2	|1	|2	|Robo	|100.0	|01-Jan-2012|

INTERSECT
```
SELECT * FROM EMPLOYEE WHERE ID IN (2, 3, 5)
INTERSECT
SELECT * FROM EMPLOYEE WHERE ID IN (1, 2, 4, 5)
```

|ID	|MGR_ID	|DEPT_ID	|NAME	|SAL	|DOJ|
|---|-------|--------|------|----|---|
|5	|2	|2	|Anno	|80.0	|01-Feb-2012|
|2	|1	|2	|Robo	|100.0	|01-Jan-2012|

### 22. What are the differences among ROWNUM, RANK and DENSE_RANK?
__ROW_NUMBER()__ is an analytical function which is used in conjunction to OVER() clause wherein we can specify ORDER BY and also PARTITION BY columns. It assigns contiguous, unique numbers from 1-N to a result set.

Suppose if you want to generate the row numbers in the order of ascending employee salaries for example:
```
SELECT name, sal, row_number() over(order by sal desc) rownum_by_sal
FROM EMPLOYEE o
```

|name	|Sal	|ROWNUM_BY_SAL|
|-----|----|-------------|
|Hash	|100	|1|
|Robo	|100	|2|
|Anno	|80	|3|
|Darl	|80	|4|
|Tomiti	|70	|5|
|Pete	|70	|6|

__RANK__ does not assign unique numbers—nor does it assign contiguous numbers. If two records tie for second place, no record will be assigned the 3rd rank as no one came in third, according to RANK. See below:

```
SELECT name, sal, rank() over(order by sal desc) rank_by_sal
FROM EMPLOYEE o
```
|name	|Sal	|RANK_BY_SAL|
|----|---|-------|
|Hash	|100	|1|
|Robo	|100	|1|
|Anno	|80	|3|
|Darl	|80	|3|
|Tomiti|70|5|
|Pete	|70	|5|
|Bhuti	|60	|7|
|Meme	|60	|7|

__DENSE_RANK__, like RANK, does not assign unique numbers, but it does assign contiguous numbers. Even though two records tied for second place, there is a third-place record. See below:

```
SELECT name, sal, dense_rank() over(order by sal desc) dense_rank_by_sal
FROM EMPLOYEE o
```

|name	|Sal	|DENSE_RANK_BY_SAL|
|-----|----|-----------------|
|Hash	|100	|1|
|Robo	|100	|1|
|Anno	|80	|2|
|Darl	|80	|2|
|Tomiti	|70	|3|
|Pete	|70	|3|
|Bhuti	|60	|4|
|Meme	|60	|4|
### 24. What is a window function?
They allow a calculation across a set of table rows that are somehow related to the current row.
- We cannot use window functions and standard aggregations in the same query.
- We can't include window functions in a GROUP BY clause. 

```
SELECT standard_amt_usd,
       DATE_TRUNC('year', occurred_at) as year,
       SUM(standard_amt_usd) OVER (PARTITION BY DATE_TRUNC('year', occurred_at) ORDER BY occurred_at) AS running_total
FROM orders
```
The __ORDER__ and __PARTITION__ define what is referred to as the “window”—the ordered subset of data over which calculations are made. Removing ORDER BY just leaves an unordered partition; in our query's case, each column's value is simply an aggregation (e.g. sum, count, average, minimum, or maximum) of all the standard_qty values in its respective account_id.

__Using Alias Function__

This is made up of two parts: the Alias (uses WINDOW <name> AS expression) between the WHERE clause and the GROUP BY clause; and the Windows function part (normally with PARTITION statement). Here's an example:
```
SELECT id,
       account_id,
       DATE_TRUNC('year',occurred_at) AS year,
       DENSE_RANK() OVER account_year_window AS dense_rank,
       total_amt_usd,
       SUM(total_amt_usd) OVER account_year_window AS sum_total_amt_usd,
       COUNT(total_amt_usd) OVER account_year_window AS count_total_amt_usd,
       AVG(total_amt_usd) OVER account_year_window AS avg_total_amt_usd,
       MIN(total_amt_usd) OVER account_year_window AS min_total_amt_usd,
FROM orders 
WINDOW account_year_window AS (PARTITION BY account_id ORDER BY DATE_TRUNC('year',occurred_at))
```
 
__LAG__
Used to compare data in the preceding row. Use LAG(column_name)

__LEAD__
Used to compare data in the following row. Use LEAD(column_name).

Example: You want to determine how the current order's total revenue (i.e. from sales of all types of paper) revenue compares to the next order's total revenue.

```
SELECT occurred_at,
       total_sales,
       LEAD(total_sales) OVER (ORDER BY occurred_at) AS lead,
       LEAD(total_sales) OVER (ORDER BY occurred_at) - total_sales AS lead_difference
FROM (
SELECT occurred_at, SUM(total_amt_usd) AS total_sales
  FROM orders 
 GROUP BY 1
 ) sub
 ```
__NTILE__

This identifies what percentile (or quartile, or any other subdivision) a given row falls into. The syntax is NTILE(*# of budgets*). ORDER BY is often used to determine which column to use to determine the quartiles (or whatever number of tiles you specify). 
```
NTILE(4) OVER (ORDER BY standard_qty) AS quartile 
or 
NTILE(100) OVER (ORDER BY standard_qty) AS percentile
```
Note that this doesn't work as well for relatively few rows as there isn't enough data for a meaningful spread.

### 23. What is a primary key?
 A __primary key__ is a field in a database table that is used to uniquely identify records. It is a must in a table.
 
### 24. What is a foreign key?
 A __foreign key__ is a primary key from one table that shows up as column/field in another table where there is a relation between the tables. There can be several foreign keys.
 
 ![Pk_fk](https://github.com/konchada2/DataScienceLearning/blob/master/Machine_Learning_Projects/Data_Science_Interview_Questions/Posts/DAND_PK_FK_Example.png)
 
### 25. What is normalization?
It is the process of organizing the columns using attributes(Foreign or primary key) and tables using their relationships to minimize data redundancy. Normalization is used to break the tables into smaller tables to avoid redundancy without losing information. It is used to improve the performance and bring the database to consistent state. There are 3 main normals forms:

- __1NF__ : Eliminates repeating groups of entities into individual tables. An entity referes to a person or place or a thing. (Remove duplicates)
- __2NF__: Create seperate tables for set of values that apply to mutiple records.
- __3NF__: Eliminate fields that does not depend on the primary key.

__Database Normalisation__: This concept is about how data will be stored and organised. There are essentially three aspects to it:

- Are the tables storing logical groupings of data?
- Can I make changes in a single location, rather than in many tables for the same information?
- Can I access and manipulate data quickly and efficiently?

### 26. What is the use of case expressions?
To substitute stored database values with our own values in the output results. 
```
select 
CASE columnname
     WHEN <condition1> THEN <Result1>
     WHEN <condition2> THEN <Result2>
     ELSE <Results3>
END AS New_columnname
FROM Table;     
```
- The CASE statement always goes in the SELECT clause.
- CASE must include the following components: WHEN, THEN, and END. ELSE is an optional component to catch cases that didn’t meet any of the other previous CASE conditions.
- You can make any conditional statement using any conditional operator (like WHERE) between WHEN and THEN. This includes stringing together multiple conditional statements using AND and OR.
- You can include multiple WHEN statements, as well as an ELSE statement again, to deal with any unaddressed conditions.
- Using just a WHERE clause will produce the same results. However, this only works if there's one condition. If there are multiple, then use a CASE.
__Example__
```
SELECT s.name, COUNT(*) num_ords,
     CASE WHEN COUNT(*) > 200 THEN 'top'
     ELSE 'not' END AS sales_rep_level
FROM orders o
JOIN accounts a
ON o.account_id = a.id 
JOIN sales_reps s
ON s.id = a.sales_rep_id
GROUP BY s.name
ORDER BY 2 DESC;
```
### 27. What is GO command?
__Go__ command is recognised by SSMS and is not a T-SQL Statement. It is used as signal to perform a task. It can not occupy the same line as a T-SQL statement. 

### 28. How can we transpose a table using SQL (changing rows to column or vice-versa) ?
The usual way to do it in SQL is to use CASE statement or DECODE statement.

### 29. What are the commands to modify data?
- Insert 
``` 
Insert into <schema>.<tablename> ([ColA], [ColB])
Values ('A', 'B') 
```
Insert data from another table
```
INSERT INTO tablename( column 1, column2,... )
SELECT column1, column2,...
FROM another_tablename;
```
- Update 
``` 
Update <schema>.<tablename> 
Set ColA = 'Value'
Where <condition> 
```
- Delete 
``` 
Delete from <schema>.<tablename> 
Where <condition> 
```
- Merge
MERGE is used to combine the data of multiple tables. It combines the INSERT and UPDATE elements. It is defined in the SQL:2003 standard.
```
MERGE INTO table1 USING table2 
ON ( condition ) 
WHEN MATCHED THEN UPDATE SET 
column1 = value1, column2 = value2,...
WHEN NOT MATCHED THEN INSERT 
( column1, column2,... ) 
VALUES( value1, value2,... );
```
```
MERGE INTO dept_dw tgt USING dept src 
on( src.deptno = tgt.deptno )
WHEN MATCHED THEN UPDATE SET
tgt.dname = src.dname, tgt.loc = trc.loc
WHEN NOT MATCHED THEN INSERT 
( deptno, dname, loc) VALUES 
( src.deptno, src.dname, src.loc);
```
### 30. How to do performance tuning in SQL?
One way to make a query run faster is to reduce the number of calculations that need to be performed. Some of the high-level things that will affect the number of calculations a given query will make include:

- Table size
- Joins
- Aggregations

Query runtime is also dependent on some things that you can’t really control related to the database itself:

- Other users running queries concurrently on the database
- Database software and optimization (e.g. Postgres is optimized differently than Redshift)

Tips to limit query run time:

- Put the LIMIT statement in a sub-query to limit data as it is executed first. Not the outer query otherwise it will still just compute the full query and then LIMIT the results in the last step. This is redundant.
- Limit your query to a small time window if you have time-series data.
- Test your query on a subset of data first.
- Reduce table sizes first before joining them using pre-aggregations or other filters with WHERE.
- You can use EXPLAIN at the start of your query to see which statements are executed first and how long they take. The modify the steps that are expensive. Then run EXPLAIN again to see if time has been reduced.
- FULL JOIN and COUNT run pretty fast. COUNT(DISTINCT) is slow.
----------------------------------------------------------------------------------------------------------------------------------------
## Subqueries
***

__Example__

If you wanted to find the average number of events for each day for each channel. The first table will provide us the number of events for each day and channel, and then we will need to average these values together using a second query.
```
SELECT channel, AVG(events) AS average_events
FROM (SELECT DATE_TRUNC('day',occurred_at) AS day,
             channel, COUNT(*) as events
      FROM web_events 
      GROUP BY 1,2) sub
GROUP BY channel
ORDER BY 2 DESC;
```
- You should not include an __alias when you write a subquery in a conditional statement__. This is because the subquery is treated as an individual value (or set of values in the IN) case rather than as a table.
- If you return an entire column from your subquery, you need to use IN to perform a logical argument. If you returned an entire table, then you must use an ALIAS for the table, and perform additional logic on the entire table

```
SELECT AVG(standard_qty) mean_standard, AVG(gloss_qty) mean_gloss, AVG(poster_qty) mean_poster, SUM(total_amt_usd) sum_total_spent
FROM orders
WHERE DATE_TRUNC('month', occurred_at) =
    (SELECT DATE_TRUNC('month', MIN(occurred_at)) AS     min_month
    FROM orders)
```
__WITH__

The WITH statement is often called a Common Table Expression or CTE. Though these expressions serve the exact same purpose as subqueries, they are more common in practice, as they tend to be cleaner for a future reader to follow the logic.

These are effectively like writing functions in programming, and calling them when they need to be run. Instead of a function name, you just use an ALIAS name.

Comparison of subquery vs WITH:

Subquery:
```
SELECT channel, AVG(events) AS average_events
FROM (SELECT DATE_TRUNC('day',occurred_at) AS day,
             channel, COUNT(*) as events
      FROM web_events 
      GROUP BY 1,2) sub
GROUP BY channel
ORDER BY 2 DESC;
```

WITH:
```
WITH events AS (SELECT DATE_TRUNC('day',occurred_at) AS day, 
                        channel, COUNT(*) as events
          FROM web_events 
          GROUP BY 1,2)

SELECT channel, AVG(events) AS average_events
FROM events
GROUP BY channel
ORDER BY 2 DESC;
```
Creating multiple WITH tables
When having multiple WITH tables, you don't list WITH on each one. Only the first table.
```
WITH table1 AS (
          SELECT *
          FROM web_events),

     table2 AS (
          SELECT *
          FROM accounts)


SELECT *
FROM table1
JOIN table2
ON table1.account_id = table2.id;
```
### 2. What is the lifetime average amount spent in terms of total_amt_usd for the top 10 total spending accounts?
```
WITH t1 AS (
    SELECT a.name, SUM(o.total_amt_usd) total_spend
    FROM accounts a
    JOIN orders o
    ON o.account_id = a.id
    GROUP BY 1
    ORDER BY 2 DESC
    LIMIT 10 )
SELECT AVG(total_spend) FROM t1
```
----------------------------------------------------------------------------------------------------------------------------------------
## Indexing
***

### 1. What is an index? What are the types of index and why do we use indexes?

### 2. Where do we use unique index?

### 3. Where do we use non-unique index?

### 4. Can we combine 2 columns to create an index?

### 5. What are clustered and non clustered index and when do we use them?

### 6. Can we do indexing if we have null values?

----------------------------------------------------------------------------------------------------------------------------------------
## Stored Procedure, Functions and Triggers
***

### 1. What is a stored procedure and when do we use? What is the usage in run time?
__Stored procedure__ consists of 1 or more T-SQL queries which are stored for future re-usage. 
They reduce network traffic and improve performance as they are already stored on server. They also provide security by applying permissions to few settings and are easier to maintain as they are stored at one place only. They are also used to accept incoming parameters and return values. 

There are 3 types of stored procedure:
- User-defined: Defined by user
- temporary: SP stored in tempdb
- system: SP used by database engine which should not be interfered. 

```
Create Proc <SP_name>
as
<SQL QUERY>
```
To execute a Stored procedure simply select the name of the SP and click on run or type ``` Execute <SP_name>```

### 2. Where is a stored procedure located

### 3. What is a function?

In SQL Server there are 3 types of Functions (or UDFs):

- __Scalar function__: Returns a single value of any datatype except text, ntext, image, cursor & timestamp.
- __Table Valued function__: Returns a table i.e. a record-set. The function body contains just a single TSQL statement.
- __Multi-statement Table Valued function__: Also returns a table (record-set) but can contain multiple TSQL statements or scripts and is defined in BEGIN END block.

### 4. What are single row functions?
These functions work on single rows only and return one result per row. 

- Manipulate data items
- Accept arguments and return one value
- Act on each row returned
- Return one result per row
- May modify the data type
- Can be nested

There are different type of single row functions:

__Character functions__: which accepts character input and returns both character and number values.
- LOWER
- UPPER
- INITCAP
- CONCAT: Concatenates two character literals
- SUBSTR
- LENGTH
- LPAD 
- RPAD
- TRIM
- REPLACE

__Number functions__: accepts numeric input and return numeric values.
- ROUND(m,n): Rounds value to specified decimal ```SELECT ROUND(123.9994, 3), ROUND(123.9995, 3);``` (123.9990, 124.0000)
  ROUND always returns a value. If length is negative and larger than the number of digits before the decimal point, ROUND returns 0.
  
|    Examples	    |Result |
|-----------------|-------|
|ROUND(748.58, -1)|	750.00|
|ROUND(748.58, -2)|	700.00|
|ROUND(748.58, -4)| 0     |

__Using ROUND to truncate__: ```SELECT ROUND(150.75, 0, 1);``` (150)

- TRUNC
- MOD : Returns remainder of division ``` Select MOD(5,2) From emp; ```
- CEIL: Returns the largest integer value that is greater than or equal to a number ```SELECT CEIL( 23.2 ) FROM dual; ``` (24)
- FLOOR: Returns the smallest integer value that is greater than or equal to a number ```SELECT FLOOR( 56.99 ) FROM dual; ``` (56)

__Date functions__: Date functions accepts date type and return date values except MONTHS_BETWEEN.
- MONTHS_BETWEEN
- ADD_MONTHS
- NEXT_DAY
- LAST_DAY
- ROUND
- TRUNC

__conversion functions__: conversion functions are used to convert from one data type to another data type.
- TO_CHAR
- TO_NUM
- TO_DATE

__General functions__:
- NVL: will convert the NULL value to the required value. ```SELECT sal + NVL( comm, 0 ) FROM emp; ```
- NVL2
- NULLIF
- COALSECE
- DECODE
- CASE

### 4. What is the difference between a function and stored procedure?

### 5. What is a trigger?

### 6. What are views? What are the advantages of using view?
Views are virtual tables based on the results of a SQL Query. 
Advantages of views are:
- Combine one or more tables into one
- Security mechanism (To hide sensitive information by not selecting those columns while creating)
- Data in a view is always current (Upto date)
```
Create View as schema.viewname
as
<SQL QUERY>
```

----------------------------------------------------------------------------------------------------------------------------
## SQL Problems
***

### 1. Explain the output of the following query:
```
select * from employee 
where 1 = 0
order by 1 desc
```
No data is retrieved. It is used to check the server connection, existence of a table and columns. 

### 2. Write a query to remove duplicate values from a table?

```
delete from table A

select col_A, count(*) as 'count_of_values'
from table A
group by Col_A
having count_of_values > 1
```

### 3. How to generate row number in SQL Without ROWNUM?

```
SELECT name, sal, (SELECT COUNT(*)  FROM EMPLOYEE i WHERE o.name >= i.name) as row_num
FROM EMPLOYEE o
order by row_num
```
|NAME	|SAL	|ROW_NUM|
|-----|----|-------|
|Anno	|80	|1|
|Bhuti|60 |2|
|Darl	|80	|3|
|Hash	|100|4|

column or the combination of columns should be unique. The column that is used in the row number generation logic is called “sort key”. Here sort key is “name” column. For this technique to work, the sort key needs to be unique. We have chosen the column “name” because this column happened to be unique in our Employee table. If it was not unique but some other collection of columns was, then we could have used those columns as our sort key (by concatenating those columns to form a single sort key).

Also notice how the rows are sorted in the result set. We have done an explicit sorting on the row_num column, which gives us all the row numbers in the sorted order. But notice that name column is also sorted (which is probably the reason why this column is referred as sort-key). If you want to change the order of the sorting from ascending to descending, you will need to change “>=” sign to “<=” in the query.

### 4. How to select first 5 records from a table?

SQL Server
```
SELECT TOP 5 * FROM EMP;
```

Generic solution
```
SELECT  name 
FROM EMPLOYEE o
WHERE (SELECT count(*) FROM EMPLOYEE i WHERE i.name < o.name) < 5
```

Column should be distinct for the above query to work. 

### 5. How many minutes worth of video does an average publisher have?

__Table Schema__

|__Publisher_Info__|                                                      
|------------------|                           
| Publisher_id     |                              
| Video_id         |                                  
| video_duration(in minutes)|         

|__Consumption_Info__|
|--------------------|
|  Video_id          |
|  User_id           |
|  User_timespent    |

```
select sum(video_duration)/count(distinct Publisher_id) as Min_Avg_Publisher
from Publisher_info;
```
### 6. How many publishers have atleast one user who watched their videos?

```
select count(Distinct Publisher_id)
From Publisher_info P
Inner JOIN Consumption info C
Where P.Video_id = C.Video_id
```
----------------------------------------------------------------------------------------------------------------------------------------
## SQL Server Integration Sevices (SSIS)

### 1. What is SSIS? What is it used for?
It is a platform for building enterprise level data integration and data transformation solutions. It consists rich set of built-in tasks and transformation tools that are used for integrating packages

It is used for:
- solving complex business problems by copying and downloading files, emails 
- Updating data warehouses
- Cleaning and mining data
- Managing SQL server objects and data
- Extracting and transforming data from various sources ex: XML, flat files

SQL server Data tools (SSDT) will install the business intelligence templates inside visual studio that is where we can access SSIS from.

### 2. What is a SSIS package?
A package is an organized collection of connnections, control flows elements, data flow elements, event handlers, variables, parameters and configuration assembled when a new integration project is created. 

### 3. Explain the workflow of ETL using SSIS?
- Add and configure flat file configuration manager
- Add and configure OLE DB configuration manager
- Add a Data flow task to package
- Add and configure flat file source
- Add and configure lookup transformation
- Add and configure OLE DB destination
- Test SSIS package
