# SQL Basic

## Data Base Management System(DBMS)

* relational

数据存储在用relationship互相连接的表中

SQL是relational DBMS的操作语言

* non relational

无法使用SQL



## retrieve data from single table

```sql
USE name_of_database
```

SQL大小写不敏感，USE和use都可以

```sql
SELECT customer_id(column)
FROM customers(table)
```

语句之间用分号隔开

```sql
USE sql_store;

SELECT *
FROM customers
```

```sql
USE sql_store;

SELECT *
FROM customers
WHERE customer_id = 1
ORDER BY fisrt_name
```
使用--注释
```mysql
USE sql_store;

SELECT *
FROM customers
--WHERE customer_id = 1
ORDER BY fisrt_name
```

注意语句的顺序SELECT，FROM，WHERE，ORDER BY

换行不敏感

```mysql
USE sql_store;

SELECT * FROM customers WHERE customer_id = 1 ORDER BY fisrt_name
```



## SELECT clause

选取指定的列

```mysql
SELECT first_name, last_name
FROM customers
```

使用数学表达式

```mysql
SELECT first_name, last_name, points + 10
FROM customers
```
将结果存储为新的列
```mysql
SELECT 
	first_name, 
	last_name,
	points,
	(points + 10) * 100 AS discount_factor
FROM customers
```
将结果存储为新的列，列名包含空格
```mysql
SELECT 
	first_name, 
	last_name,
	points,
	(points + 10) * 100 AS ‘discount factor’
FROM customers
```
如果一个列中有多个重复项，但是显示时要去重，使用DISTINCT

```mysql
SELECT DISTINCT state
FROM customers
```


## Where clause

用于过滤

```mysql
SELECT *
FROM Customers
WHERE points > 3000
```

逻辑操作符

* $ >, >=, <, <=$
* $=, !=, <>$ 第三个也是不等于

```mysql
SELECT *
FROM Customers
WHERE state = 'VA'
```

字符串要加引号，大小写没有区别
```mysql
SELECT *
FROM Customers
WHERE state = 'va'
```
```mysql
SELECT *
FROM Customers
WHERE birth_date > '1990-01-01'
```
多个过滤条件

```mysql
SELECT *
FROM Customers
WHERE birthday > '1990=01-01' AND points > 1000
```

逻辑操作符

* AND, OR（操作符顺序AND>OR）
* NOT
* IN
* BETWEEN

### IN操作符

原本的语句

```mysql
SELECT *
FROM Customers
WHERE state = 'VA' OR state = 'GA' OR state = 'FL'
```

使用IN

```
SELECT * 
FROM Customers
WHERE state IN ('VA', 'FL', 'GA')
```

### BETWEEN操作符

原本的语句

```
SELECT * 
FROM Customers
WHERE points >= 1000 AND points <= 3000
```



