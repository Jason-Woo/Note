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

```
SELECT A.x + A.y AS z
FROM A
WHERE (A.x + A.y) = 10
```

3. 无论在语法上还是在执行顺序上， UNION 总是排在在 ORDER BY 之前。



## SQL语言的核心是对表的引用







