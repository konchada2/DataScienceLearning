# SQL Interview Questions

## T-SQL 

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
### 7. What is group by clause?
__Group By__ clause is used in combination with aggregate function to group returned query output.
```
select city, count(*) as 'total_Count_city'
From Address
Group By City
```
The query will return the count of each distinct city. 

__Note__: To find the unique records in atable we can use group by instead of distinct. 

### 8. Explain order by clause ?
__Order by__ clause is used to sort the final output result data set. It always comes at the end of slect statement. We can use column alias and column numbers in order by clause. When we are dealing with multiple columns we can sort both of them in ascending and descending order.   

### 9. what is the difference between having and where clause? 
__WHERE__ and __HAVING__ both filters out records based on one or more conditions. The difference are:
- WHERE clause can only be applied on a static non-aggregated column whereas HAVING on aggregated columns
- HAVING clause always comes after WHERE clause
- WHERE clause can be used on select, insert and update statements but having is only used on select
- Aggregate functions cannot be used in the WHERE clause, unless it is in a sub query contained in a HAVING clause, whereas, aggregate functions can be used in Having clause

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
It is used to select rows where participating tables have identical columns.
```
select A.colA, B.colA
FROM Table A 
inner join Table B
on A.colA = B.colA
```

### 13. Difference between inner and left join?

### 14. How does outer join work?

### 15. Give a scenario where inner join works the same as outer join?

### 16. What is a self join?
__Self Join__ is the act of joining one table with itself. It is often very useful to convert a hierarchical structure into a flat structure.

Say for example we have employee table keeping the manager ID of each employee in the same row as that of the employee. This is an example of how a hierarchy (in this case employee-manager hierarchy) is stored in the RDBMS table. Now, suppose if we need to print out the names of the manager of each employee right beside the employee, we can use self join. See the example below:

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

### 17. Advantages and diadvantages of self join? 

### 18. What is the difference between JOIN and UNION?
SQL JOIN allows us to “lookup” records on other table based on the given conditions between two tables. UNION operation allows us to add 2 similar data sets to create resulting data set that contains all the data from the source data sets. 

- Union does not require any condition for joining


### 19. What is the difference between union and union all?
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

### 20. What is the difference among UNION, MINUS and INTERSECT?
___UNION__ combines the results from 2 tables and eliminates duplicate records from the result set.

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

### 21. What are the differences among ROWNUM, RANK and DENSE_RANK?
__ROW_NUMBER()__ is an analytical function which is used in conjunction to OVER() clause wherein we can specify ORDER BY and also PARTITION BY columns. It assigns contiguous, unique numbers from 1-N to a result set.

Suppose if you want to generate the row numbers in the order of ascending employee salaries for example:
```
SELECT name, sal, row_number() over(order by sal desc) rownum_by_sal
FROM EMPLOYEE o
```

|name	|Sal	|ROWNUM_BY_SAL|
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
|Hash	|100	|1|
|Robo	|100	|1|
|Anno	|80	|2|
|Darl	|80	|2|
|Tomiti	|70	|3|
|Pete	|70	|3|
|Bhuti	|60	|4|
|Meme	|60	|4|



### 18. What is a primary key?
 A __primary key__ is a field in a database table that is used to uniquely identify records. It is a must in a table.
 
### 19. What is a foreign key?
 A __foreign key__ is a primary key from one table that shows up as column/field in another table where there is a relation between the tables. There can be several foreign keys.
 
### 20. What is normalization?
It is the process of organizing the columns using attributes(Foreign or primary key) and tables using their relationships to minimize data redundancy. Normalization is used to break the tables into smaller tables to avoid redundancy without losing information. It is used to improve the performance and bring the database to consistent state. There are 3 main normals forms:

- __1NF__ : Eliminates repeating groups of entities into individual tables. An entity referes to a person or place or a thing. (Remove duplicates)
- __2NF__: Create seperate tables for set of values that apply to mutiple records.
- __3NF__: Eliminate fields that does not depend on the primary key.

### 21. What is the use of case expressions?
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
### 22. What is GO command?
__Go__ command is recognised by SSMS and is not a T-SQL Statement. It is used as signal to perform a task. It can not occupy the same line as a T-SQL statement. 

### 23. How can we transpose a table using SQL (changing rows to column or vice-versa) ?
The usual way to do it in SQL is to use CASE statement or DECODE statement.

### 23. What are the commands to modify data?
- Insert 
``` 
Insert into <schema>.<tablename> ([ColA], [ColB])
Values ('A', 'B') 
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


----------------------------------------------------------------------------------------------------------------------------------------
## Indexing

### 1. What is an index? What are the types of index and why do we use indexes?

### 2. Where do we use unique index?

### 3. Where do we use non-unique index?

### 4. Can we combine 2 columns to create an index?

### 5. What are clustered and non clustered index and when do we use them?

### 6. Can we do indexing if we have null values?

----------------------------------------------------------------------------------------------------------------------------------------
## Stored Procedure, Functions and Triggers

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
