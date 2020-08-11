# SQL

## SQL是一种申明式语言

```sql
SELECT first_name, last_name FROM employees WHERE salary > 100000
```



## SQL的语法并不按照语法顺序执行

SQL的语法顺序是：

* SELECT[DISTINCT]
* FROM
* WHERE
* GROUP BY
* HAVING
* UNION
* ORDER BY

其执行顺序为：

- FROM
- WHERE
- GROUP BY
- HAVING
- SELECT
- DISTINCT
- UNION
- ORDER BY

有三个值得注意的地方

1. FROM 才是 SQL 语句执行的第一步，并非 SELECT。执行SQL语句的第一步时将数据从硬盘加载到数据缓冲区。
2. SELECT 是在大部分语句（FROM和GROUP BY）执行了之后才执行的。这就是你不能在 WHERE 中使用在 SELECT 中设定别名的字段作为判断条件的原因。例如：

```sql
SELECT A.x + A.y AS z
FROM A
WHERE z = 10 -- z 在此处不可用，因为SELECT是最后执行的语句！
```

正确的写法是：

```sql
SELECT A.x + A.y AS z
FROM A
WHERE (A.x + A.y) = 10
```

3. 无论在语法上还是在执行顺序上， UNION 总是排在在 ORDER BY 之前。



## SQL语言的核心是对表的引用

FROM 输出的结果被 WHERE 语句筛选后要经过 GROUP BY 语句处理，从而形成新的输出结果。

```sql
From a, b
```

假设a有a1条数据，a2个字段，b有b1条数据，b2个字段

那最后有a2+b2个字段，a1*b1个数据



## 灵活引用表使SQL更强大

```sql
FROM a1 JOIN a2 ON a1.id = a2.id, b
```

最终输出的表就有了 a1+a2+b 个字段了

 要理解 JOIN 是构建连接表的关键词，并不是 SELECT 语句的一部分。



## SQL语句中推荐使用表链接

尽量不要使用逗号来代替 JOIN 进行表的连接

```sql
FROM a, b
```

利用逗号来简化 SQL 语句有时候会造成思维上的混乱，记着要尽量使用 JOIN 进行表的连接，永远不要在 FROM 后面使用逗号连接表。



## JOIN

SQL 语句中，表连接的方式从根本上分为五种：

- EQUI JOIN
  - INNER JOIN（JOIN）
  - OUTER JOIN（LEFT 、 RIGHT、 FULL OUTER JOIN）



- SEMI JOIN
  - IN比 EXISTS 的可读性更好
  - EXISTS 比IN 的表达性更好（更适合复杂的语句）
  - 二者之间性能没有差异（但对于某些数据库来说性能差异会非常大）

需要那些在书名表中的书的作者信息。那我们就能这么写：

```sql
-- Using IN
FROM author
WHERE author.id IN (SELECT book.author_id FROM book)

-- Using EXISTS

FROM author
WHERE EXISTS (SELECT 1 FROM book WHERE book.author_id = author.id)
```

因为使用 INNER JOIN 也能得到书名表中书所对应的作者信息，所以很多初学者机会认为可以通过 DISTINCT 进行去重，然后将 SEMI JOIN 语句写成这样：

```sql
-- Find only those authors who also have books
SELECT DISTINCT first_name, last_name
FROM author
JOIN book ON author.id = book.author_id
```

这是一种很糟糕的写法，原因如下：

1. SQL 语句性能低下：因为去重操作（ DISTINCT ）需要数据库重复从硬盘中读取数据到内存中。（译者注：DISTINCT 的确是一种很耗费资源的操作，但是每种数据库对于 DISTINCT 的操作方式可能不同）。

2. 这么写并非完全正确：尽管也许现在这么写不会出现问题，但是随着 SQL 语句变得越来越复杂，你想要去重得到正确的结果就变得十分困难。



* ANTI JOIN

列出书名表里没有书的作者：

```sql
-- Using IN
FROM author
WHERE author.id NOT IN (SELECT book.author_id FROM book)

-- Using EXISTS
FROM author
WHERE NOT EXISTS (SELECT 1 FROM book WHERE book.author_id = author.id)
```



- CROSS JOIN

即将第一张表的每一条数据分别对应第二张表的每条数据。

```sql
-- Combine every author with every book
author CROSS JOIN book
```



- DIVISION

如果 JOIN 是一个乘法运算，那么 DIVISION 就是 JOIN 的逆过程。



## SQL中如同变量的派生表

QL 是一种声明性的语言，并且 SQL 语句中不能包含变量。但是你能写出类似于变量的语句，这些就叫做派生表：

```sql
-- A derived table with an alias
FROM (SELECT * FROM author) a
```

派生表可以有效的避免由于 SQL 逻辑而产生的问题。举例来说：如果你想重用一个用 SELECT 和 WHERE 语句查询出的结果，这样写就可以

```sql
-- Get authors' first and last names, and their age in days
SELECT first_name, last_name, age
FROM (
  SELECT first_name, last_name, current_date - date_of_birth age
  FROM author
)
-- If the age is greater than 10000 days
WHERE age > 10000
```



## SQL语句中GROUP BY是对表的引用进行的操作

当你用 GROUP BY 的时候，你能够对其进行下一级逻辑操作的列会减少，包括在 SELECT 中的列



## SQL 语句中的 SELECT 实质上是对关系的映射

其他语句的作用其实就是对表的不同形式的引用。而 SELECT 语句则把这些引用整合在了一起



## SQL 语句中的几个简单的关键词：DISTINCT,UNION ,ORDER BY 和 OFFSET

- 集合运算（ DISTINCT 和 UNION ）
  - DISTINCT 在映射之后对数据进行去重
  - UNION 将两个子查询拼接起来并去重
  - UNION ALL 将两个子查询拼接起来但不去重
  - EXCEPT 将第二个字查询中的结果从第一个子查询中去掉
  - INTERSECT 保留两个子查询中都有的结果并去重

- 排序运算（ ORDER BY，OFFSET…FETCH）

排序运算跟逻辑关系无关。

- 集合运算（ set operation）