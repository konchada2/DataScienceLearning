# SQL Interview Questions

### 1. What are the common commands in SQL?
- Select
- Insert
- Delete
- Update
- Where

### 2. What is the query order of execution?
From & Joins -> Where -> Group By -> Having -> Select -> Distinct -> Order By -> Limit/Offset

### 3. Explain Where clause?
__Where__ clause is used to filter records. It is used in combination to:
- Equality filter (=, <>, <=, >= ``` Where Gender = 'M' ```, ``` Where class <> 2 ```)
- Basic Comparison (<,> ``` Where score < 60```)
- Logical Comparison (AND - ```Where Type = 'A' AND Score > 75```, OR - ```Where Type = 'A' OR Type = 'B' ```, IN - ``` Where FirstName IN ('Tom', 'Dick', 'Harry')```, BETWEEN - ```Where Score Between 40 AND 80``` )
- String Comparison (Like - ```Where JobTitle Like '%Data%' ```, NOT Like - ```Where Name NOT Like '_ary' ```, wild cards - '%')
- NULL Comparison (```Where size is NULL```, ```Where Gender is NOT NULL```)

### 4. What are aggregate functions?
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
### 5. What is group by clause?
__Group By__ clause is used in combination with aggregate function to group returned query output.
```
select city, count(*) as 'total_Count_city'
From Address
Group By City
```
The query will return the count of each distinct city. 

__Note__: To find the unique records in atable we can use group by instead of distinct. 

### 6. what is the difference between having and where clause? 

### 7 . Why do we use joins?
We use joins to combine rows from two or more related tables and present as one output result.

### 8. What are different types of joins?
- Inner Join
- Left Join
- Right Join
- Outer Join

### 9. How does inner join work?
It is used to select rows where participating tables have identical columns.
```
select A.colA, B.colA
FROM Table A 
inner join Table B
on A.colA = B.colA
```

### 10. Difference between inner and left join?

### 11. How does outer join work?

### 12. Give a scenario where inner join works the same as outer join?

### 13. What is a self join?

### 14. Advantages and diadvantages of self join?


### 15. What is a primary key?
 A __primary key__ is a field in a database table that is used to uniquely identify records. It is a must in a table.
 
### 16. What is a foreign key?
 A __foreign key__ is a primary key from one table that shows up as column/field in another table where there is a relation between the tables. There can be several foreign keys.
 
### 17. What is normalization?
It is the process of organizing the columns using attributes(Foreign or primary key) and tables using their relationships to minimize data redundancy. Normalization is used to break the tables into smaller tables to avoid redundancy without losing information. It is used to improve the performance and bring the database to consistent state. There are 3 main normals forms:

- __1NF__ : Eliminates repeating groups of entities into individual tables. An entity referes to a person or place or a thing. (Remove duplicates)
- __2NF__: Create seperate tables for set of values that apply to mutiple records.
- __3NF__: Eliminate fields that does not depend on the primary key.

### 18. What is the use of case expressions?
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
### 19. What is GO command?
__Go__ command is recognised by SSMS and is not a T-SQL Statement. It is used as signal to perform a task. It can not occupy the same line as a T-SQL statement. 

### 20. What are the commands to modify data?
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

