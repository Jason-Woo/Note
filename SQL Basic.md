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
* LIKE
* REGEXP

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
FROM customers
WHERE points >= 1000 AND points <= 3000
```

使用BETWEEN，注意两端的值也是包含在内

```mysql
SELECT * 
FROM customers
WHERE points BETWEEN 1000 AND 3000
```

```mysql
SELECT *
FROM customers
WHERE birth_date BETWEEN '1990-01-01' AND '2000-01-01'
```

### LIKE操作符

以b开头的所有客户

```mysql
SELECT *
FROM customers
WHERE last_name LIKE 'b%'
```
名字包含b的客户
```mysql
SELECT *
FROM customers
WHERE last_name LIKE '%b%'
```
使用_来精确匹配
```mysql
SELECT *
FROM customers
WHERE last_name LIKE 'b_'
```

### 正则表达式REGEXP

* ^ beginning
* $ end
* | logical or
* [abcd] a|b|c|d
* [a-f] range



```mysql
SELECT * 
FROM customers
--WHERE last_name LIKE '%field%'
WHERE last_name REGEXP 'field'
```

后两行等价

使用^来表示字符串的开头

```mysql
WHERE last_name REGEXP '^field'
```

找到所有以filed开头的人

使用$来表示字符串的结尾
```mysql
WHERE last_name REGEXP 'field$'
```

找到所有以filed结尾的人

使用|来表示or

```mysql
WHERE last_name REGEXP 'field|mac'
```
找到所有名字包含mac或者filed的人

```mysql
WHERE last_name REGEXP 'field$|mac|rose'
```
找到名字以filed结尾，或者包含mac或rose的人

```mysql
WHERE last_name REGEXP '[gim]e'
```
找到所有名字包含ge，ie或者me的人，[]可以放在任意位置

```mysql
WHERE last_name REGEXP '[a-h]e'
```
可以用-来表示范围



## 缺失值

```mysql
SELECT *
FROM customers
WHERE phone IS NULL
```



##  排序ORDER BY

```mysql
SELECT *
FROM customers
ORDER BY fist_name
```
逆序
```mysql
SELECT *
FROM customers
ORDER BY fist_name DESC
```
根据多列排序

```mysql
SELECT *
FROM customers
ORDER BY state DESC, fist_name
```
MySQL的合法表达

```mysql
SELECT first_name, last_name, 10 AS points
FROM customers
ORDER BY points, first_name
```
但是其他数据库不允许对未选择的列排序，因为SELECT是最后执行的

复杂语句

```mysql
SELECT *, quality * unit_price AS total_price
FROM order_items
WHERE order_id = 2
ORDER BY quality * unit_price DESC
```



## LIMIT clause

```mysql
SELECT *
FROM customers
LIMIT 3
```

只返回前三个
```mysql
SELECT *
FROM customers
LIMIT 6, 3
```

这里6是偏移量，表示跳过前六个，然后再选三个（得到7，8，9）

LIMIT clause是在语句的末尾



## 从多个表中选择列

```mysql
SELECT order_id, first_name, last_name
FROM orders
JOIN customers ON orders.customer_id = customer.cutomer_id
```

INNER JOIN = JOIN

order_id在表order中，first_name, last_name在表customers中

如果
```mysql
SELECT order_id, first_name, last_name, customer_id
FROM orders
JOIN customers ON orders.customer_id = customer.cutomer_id
```
会报错，因为customer_id存在多个表中

正确写法

```mysql
SELECT order_id, first_name, last_name, orders.customer_id
FROM orders
JOIN customers ON orders.customer_id = customer.cutomer_id
```
一种简化表格的写法

```mysql
SELECT order_id, first_name, last_name, o.customer_id
FROM orders o
JOIN customers ON o.customer_id = customer.cutomer_id
```


## 合并多个数据库中表格中的列

```mysql
USE sql_store;

SELECT *
FROM order_items oi
JOIN sql_inventory.product p
	ON oi.product_id = p.product_id
```



## Self JOIN

例如员工的表格，有一列reports_to表示其上司的id，而上司也作为员工被存储在同一张表

```
USE sql_hr;

SELECT
	e.emplyee_id, e.first_name
	m.first_name AS manager
FROM employees e
JOIN employees m
	ON e.reports_to = m.employee_id
```



## 连接两个以上的表

```mysql
USE sql_store;

SELECT 
	o.order_id,
	o.order_date,
	c.first_name,
	c.last_name,
	os.name AS status
FROM orders o
JOIN customers c
	ON o.customer_id = c.customer_id
JOIN order_statuses os
	ON o.status = os.order_status_id
```



## 复合的连接条件

对于有两个primarily key的表格，例如同一个order的同一个product有两条note

```mysql
SELECT * 
FROM order_items oi
JOIN order_item_note oin
	ON oi.order_id = oin.order_id
	AND oi.product_id = oin.product_id
```



## Implicit joint syntax 隐式联合语法

对于

```mysql
SELECT * 
FROM orders o
JOIN customers c
	ON o.customer_id = c.customer_id
```

可以写为
```mysql
SELECT * 
FROM orders o, customers c
WHERE o.customer_id = c.cunstomer_id
```
最好使用显式的语法，因为隐式的WHERE可能会出错



## Outer Joins

