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

```mysql
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

对于有两个primarily key的表格，例如同一个order的同一个product有两条note，同时需要order_id和product_id来确定唯一的一个对象

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



## OUTER JOIN

```mysql
SELECT 
	c.customer_id
	c.first_name
	o.order_id
FROM customer c
JOIN orders o
	ON c.customer_id = o.customer_id
ORDER BY c.cunstomer_id
```

使用inner join会发生这种情况：最后输出的表格只能看到有订单的客户，因为在inner join时是根据

```
c.customer_id = o.customer_id
```

来合并的，没有订单的客户在o.customer_id中没有值。

```mysql
SELECT 
	c.customer_id
	c.first_name
	o.order_id
FROM customer c
LEFT JOIN orders 0
	ON c.customer_id = o.customer_id
ORDER BY c.cunstomer_id
```

使用LEFT JOIN时，所有左边表格的值都会被返回，无论条件是否满足

使用RIGHT JOIN时，所有右边表格的值都会被返回，无论条件是否满足，在上面的情况下和inner join输出的结果是一样的。



## 多张表的OUTER JOIN

```mysql
SELECT 
	c.customer_id
	c.first_name
	o.order_id
	sh.name AS shipper
FROM customer c
LEFT JOIN orders 0
	ON c.customer_id = o.customer_id
LEFT JOIN shippers sh
	ON o.shipper_id = sh.shipper_id
ORDER BY c.cunstomer_id
```

最好避免RIGHT JOIN来避免阅读的复杂性



## self OUTER JOIN

还是那个employee的例子，直接使用JOIN会确实manager的行，所以使用left join

```mysql
USE sql_hr;

SELECT
	e.employee_id
	e.first_name
	m.first_name As manager
FROM employees e
LEFT JOIN employees m
	ON e.reports_to = m.employee_id
```



## USING clause

在mysql中，可以做如下替换，尽在列名相同时

```mysql
SELECT 
	o.order_id,
	c.first_name
FROM customer o
JOIN orders c
	-- ON o.customer_id = c.customer_id
	USING (customer_id)
ORDER BY c.cunstomer_id
```

```mysql
SELECT *
FROM order_items oi
JOIN order_item_notes oin
	-- ON oi.order_id = oin.order_id
	-- ANd oi.product_id = oin.product_id
	USING (order_id, product_id)
	
```



## Natural JOIN

也是mysql中独有的

```mysql
SELECT 
	o.order_id,
	c.first_name
FROM order o
NATURAL JOIN customer c
```

不需要自己指出join的列，系统会根据公共列自己join tables

不建议使用



## CROSS JOIN

将第一张表中的所有记录与第二张表中的所有记录join

```mysql
SELECT
	c.first_name AS customer,
	p.name AS product
FROM customers c
CROSS JOIN products p
ORDER BY c.first_name
```

这种写法也不需要指定列

隐式写法：

```mysql
SELECT
	c.first_name AS customer,
	p.name AS product
FROM customers c, orders o
ORDER BY c.first_name
```



## Unions

每一个订单都有order_date，我们想要添加一个标签来表示这个订单是不是今年被下达的

```mysql
SELECT
	order_id,
	order_date,
	'Active' As status
FROM orders
WHERE order_id >= '2019-01-01'
UNION
SELECT
	order_id,
	order_date,
	'Archived' As status
FROM orders
WHERE order_id < '2019-01-01'
```

unions时把两张表在行上进行了结合

UNIONS也可以在两张不同的表上进行，要注意的是两次SELECT返回的列数应该是相等的

最后输出的列名为第一次SELECT的列名





## 插入，更新，删除数据

### 列的属性

![image-20200810160544438](img_sql\image-20200810160544438.png)

如上图，pk表示primary key，NN表示Not Null不可为空，AI表示Automatically increase。我们一般用VARCHAR来存储字符串，char是定长的，所以长度不足规定的会被自动补全空格



### 插入行

```mysql
INSERT INTO customers
VALUES (
    DEFAULT, 
    'John', 
    'Smith', 
    NULL,
	DEFAULT,
	'adress',
	'city',
	'CA',
	DEFAULT)
```
不需要插入所有的值，顺序是不重要的
```mysql
INSERT INTO customers (
	first_name,
	last_name,
	adress,
	city,
	state)
VALUES (
    'John', 
    'Smith', 
	'adress',
	'city',
	'CA')
```



### 插入多行

![image-20200810161453052](img_sql\image-20200810161453052.png)

```mysql
INSERT INTO shippers(name)
VALUES ('Shipper1'),
	('Shipper2'),
	('Shipper3')
```



### 多个表中插入行

![image-20200810162153323](img_sql\image-20200810162153323.png)

![image-20200810162220424](img_sql\image-20200810162220424.png)

如上图，一个oder会对应多个order_items。order_id是在插入order表中是走动生成的

```mysql
INSERT INTO orders(customer_id, order_date, status)
VALUES(1, '2019-01-02', 1);

INSERT INTO order_itmes
VALUES
	(LAST_INSERT_ID(), 1, 1, 2.95)
	(LAST_INSERT_ID(), 2, 1, 3.95)
```

LAST_INSERT_ID()函数存储了上次插入的id



### 从表格中复制数据

```mysql
CREATE TABLE order_archived AS
SELECT * FROM orders
```

但是mysql会忽略列的PK和AI这两条属性

```mysql
INSERT INTO order_archived
SELECT *
FROM orders
WHERE order_date < '2019-01-01'
```

嵌套的语句

```mysql
USE sql_invoicing;

CREATE TABLE invoices_archive AS
SELECT
	i.invoice_id,
	i.number,
	c.name As client,
	i.invoice_toal,
	i.payment_toal,
	i.invoice_date,
	i.payment_date,
	i.due_date
FROM invoices i
JOIN clients c
	ON i.client_id = c.client_id
WHERE i.payment_date IS NOt NULL
```



###  更新一行

```mysql
UPDATE invoices
SET payment_toal = 10, payment_date = '2019-03-01'
WHERE invoice_id = 1
```

带有表达式的

```sql
UPDATE invoices
SET
	payment_total = invoice_toal * 0.5
	payment_date = due_date
WHERE invoice_id = 3
```



### 更新多行

```sql
UPDATE invoices
SET
	payment_total = invoice_toal * 0.5
	payment_date = due_date
WHERE invoice_id = IN(3, 4)
```



```mysql
USE sql_store;

UPDATE cutomers
SET points = points + 50
WHERE birth_date < '1990-01-01'
```



### 在更新语句中使用子查询

假设我们只知道名字，不知道id

```mysql
UPDATE invoices
SET
	payment_total = invoice_toal * 0.5
	payment_date = due_date
WHERE invoice_id = 
                (SELECT client_id
                FROM clients
                WHERE name = 'Myworks')
```

```mysql
UPDATE invoices
SET
	payment_total = invoice_toal * 0.5
	payment_date = due_date
WHERE invoice_id IN 
                (SELECT client_id
                FROM clients
                WHERE name IN ('CA', 'NY'))
```

为了保证更新的正确，一般我们先写出SELECT语句核对结果后再写UPDATE



### 删除行

```mysql
DELETE FROM invoices
WHERE client_id = 
			(SELECT *
			FROM clients
			WHERE name = 'MYworks')
```



## Aggregate Functions聚合函数

多输入 单输出

* MAX() MIN()
* AVG() SUM() COUNT()

```mysql
SELECT 
	MAX(invoice_total) AS highest,
	MIN(invoice_total) AS lowests,
	AVG(invoice_total * 1.1) As avergae
FROM invoices
```

如果出现空值不会被处理

```mysql
COUNT(invoice_total)
COUNT(*) AS total_records
```

第二句才能返回真实大小

同时COUNT()函数忽视重复值

```mysql
COUNT(client_id)
COUNT (DISTINCT client_id)
```

第二句才能返回有多少独立的客户



##  GROUP BY clause

```mysql
SELECT 
	client_id,
	SUM(invoice_toal) As total_sales
FROM invoices
WHERE invoice_date >= '2019-07-01'
GROUP BY client_id
ORDER BY total_sales DESC
```

根据客户来求和

多行的group by

```mysql
SELECT 
	state,
	city,
	SUM(invoice_toal) As total_sales
FROM invoices
JOIN clients USING (client_id)
GROUP BY state, city
```



## HAVING Clause
```mysql
SELECT 
	client_id,
	SUM(invoice_toal) As total_sales
FROM invoices
GROUP BY client_id
```

假设我们现在只想要total_sales超过200的客户，不能用WHERE，因为GROUP BY在WHERE之后执行

```mysql
SELECT 
	client_id,
	SUM(invoice_toal) As total_sales
FROM invoices
GROUP BY client_id
HAVING total_sales > 500
```

```mysql
SELECT 
	client_id,
	SUM(invoice_toal) As total_sales,
	COUNT(*) As number_of_invoices
FROM invoices
GROUP BY client_id
HAVING total_sales > 500 AND number_of_invoices > 5
```

HAVING 中的条件一定要在SELECT内！但是WHERE不用

```mysql
USE sql_store;

SELECT 
	c.customer_id,
	c.first_name,
	c.last_name,
	SUM(oi.quantity * oi.unit_price) AS total_sales
FROM customers c
JOIN orders o
	ON c.customer_id = o.customer_id
JOIN order_items oi
	ON oi.order_id = o.order_id
WHERE state = 'VA'
GROUP BY 
	c.customer_id,
	c.first_name,
	c.last_name
HAVING total_sales > 100
```



## ROLLUP operator

仅在mysql中存在

```mysql
SELECT 
	client_id,
	SUM(invoice_toal) As total_sales
FROM invoices
GROUP BY client_id WITHROLLUP
```

会在结尾添加一行求和结果

```mysql
SELECT 
	state,
	city,
	SUM(invoice_toal) As total_sales
FROM invoices
JOIN client c USING (client_id)
GROUP BY state, city WITH ROLLUP
```

会根据每一个city生成一次求和，然后所有的state生成一次求和

