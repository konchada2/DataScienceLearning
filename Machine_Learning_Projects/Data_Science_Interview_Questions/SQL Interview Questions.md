# SQL Interview Questions

### 1. What is the query order of execution?
From & Joins -> Where -> Group By -> Having -> Select -> Distinct -> Order By -> Limit/Offset

### 2. what is the differenc between having and where clause? 


----------------------------------------------------------------------------------------------------------------------------------------
Indexing

### 1. What is an index? What are the types of index and why do we use indexes?

### 2. Where do we use unique index?

### 3. Where do we use non-unique index?

### 4. Can we combine 2 columns to create an index?

### 5. What are clustered and non clustered index and when do we use them?

### 6. Can we do indexing if we have null values?

----------------------------------------------------------------------------------------------------------------------------------------

Stored Procedure, Functions and Triggers

### 1. What is a stored procedure and when do we use? What is the usage in run time?

### 2. Where ia stored procedure located

### 3. What is a function?

### 4. What is the difference between a function and stored procedure?

### 5. What is a trigger?

----------------------------------------------------------------------------------------------------------------------------
SQL Problems

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
