从头开始
====

>SQL是对关键字和标识符大小写不敏感的语言，只有在标识符用双引号包围时才能保留它们的大小写。 

SQL语言
----

* 类型
```sql
--以下描述仅为猜想，orz
int  --整型int
smallint  --短整型short
real  --单精度浮点型float
double precision  --双精度浮点型double
char(N)
varchar(N)
date  --可自行解释类型
time
timestamp
interval
point
```

* 创建一个新表
```sql
--创建表
CREATE TABLE tablename (
    name       int, --name是列名，int是列的类型，"--"是注释的开头
    ...
); 

--删除表
DROP TABLE tablename; 
```
* 在表中增加行
```sql
--插入一行
INSERT INTO tablename (name1, name2, name3, name5)  --此行括号内容可缺省，加入括号可选择插入哪些列
    VALUES (value1, value2, value3, value5);

--从文本文件中装载大量数据
--源文件的文件名必须在运行后端进程的机器上是可用的， 而不是在客户端上，因为后端进程将直接读取该文件
COPY tablename FROM '/home/user/tablename.txt';
```

- 查询一个表

    一个查询可以使用WHERE子句"修饰"，它指定需要哪些行。WHERE子句包含一个布尔（真值）表达式，只有那些使布尔表达式为真的行才会被返回。在条件中可以使用常用的布尔操作符（AND、OR和NOT）
```sql
SELECT * FROM tablename;  --这里*是"所有列"的缩写。等于：
SELECT name, ..., ... FROM tablename;

SELECT city, (temp_hi+temp_lo)/2 AS temp_avg, date FROM weather;  
--AS子句是如何给输出列重新命名的（AS子句是可选的）

SELECT * FROM weather
    WHERE city = 'San Francisco' AND prcp > 0.0;

SELECT * FROM weather
    ORDER BY city;  --ORDER BY为排序
    
SELECT DISTINCT city
    FROM weather;  --DISTINCT可以查询的结果中消除重复的行
```

- 在表之间连接
    
    类似于拿 weather表每行的city列和cities表所有行的name列进行比较， 并选取那些在该值上相匹配的行对。但这里只是一个概念上的模型。连接通常以比实际比较每个可能的行对更高效的方式执行
```sql
SELECT W1.city, W1.temp_lo AS low, W1.temp_hi AS high,  
    W2.city, W2.temp_lo AS low, W2.temp_hi AS high
--如果在两个表里有重名的列，你需要限定列名来说明你究竟想要哪一个
    FROM weather W1, weather W2
--可以把一个表和自己连接起来。这叫做自连接
--在这里我们把weather表重新标记为W1和W2以区分连接的左部和右部。你还可以用这样的别名在其它查询里节约一些敲键
    WHERE W1.temp_lo < W2.temp_lo
    AND W1.temp_hi > W2.temp_hi;  

--到目前为止，这种类型的连接查询也可以用下面这样的形式写出来
SELECT *
    FROM weather INNER JOIN cities ON (weather.city = cities.name);

--如果我们没有找到匹配的行，那么我们需要一些"空值"代替cities表的列。 
--这种类型的查询叫外连接 （我们在此之前看到的连接都是内连接）
SELECT *
    FROM weather LEFT OUTER JOIN cities ON (weather.city = cities.name);  --此为左外连接
```

* 聚集函数

    理解聚集和SQL的WHERE以及HAVING子句之间的关系对我们非常重要。WHERE和HAVING的基本区别如下：WHERE在分组和聚集计算之前选取输入行（因此，它控制哪些行进入聚集计算）， 而HAVING在分组和聚集之后选取分组行。因此，WHERE子句不能包含聚集函数； 因为试图用聚集函数判断哪些行应输入给聚集运算是没有意义的。相反，HAVING子句总是包含聚集函数（严格说来，你可以写不使用聚集的HAVING子句， 但这样做很少有用。同样的条件用在WHERE阶段会更有效）
```sql
--例如，行集合上计算count（计数）、sum（和）、avg（均值）、max（最大值）和min（最小值）的函数
SELECT max(temp_lo) FROM weather;
SELECT city FROM weather WHERE temp_lo = max(temp_lo);     --错误
--聚集max不能被用于WHERE子句中
--（存在这个限制是因为WHERE子句决定哪些行可以被聚集计算包括；因此显然它必需在聚集函数之前被计算）

SELECT city FROM weather
    WHERE temp_lo = (SELECT max(temp_lo) FROM weather);  
--可以使用子查询，因为子查询是一次独立的计算，它独立于外层的查询计算出自己的聚集
    
SELECT city, max(temp_lo)
    FROM weather
    GROUP BY city;  --GROUP BY为分组

SELECT city, max(temp_lo)
    FROM weather
    WHERE city LIKE 'S%'(1)  --LIKE操作符进行模式匹配
    GROUP BY city
    HAVING max(temp_lo) < 40;
```

* 更新和删除
```sql
--更新
UPDATE weather
    SET temp_hi = temp_hi - 2,  temp_lo = temp_lo - 2
    WHERE date > '1994-11-28';

--删除
DELETE FROM weather WHERE city = 'Hayward';
DELETE FROM tablename;  --如果没有一个限制，DELETE将从指定表中删除所有行，把它清空。做这些之前系统不会请求你确认
```

高级特性
----

- 视图

    假设天气记录和城市为止的组合列表对我们的应用有用，但我们又不想每次需要使用它时都敲入整个查询。我们可以在该查询上创建一个视图，这会给该查询一个名字，我们可以像使用一个普通表一样来使用它  
    对视图的使用是成就一个好的SQL数据库设计的关键方面。视图允许用户通过始终如一的接口封装表的结构细节，这样可以避免表结构随着应用的进化而改变
```sql
CREATE VIEW myview AS  --创建视图
    SELECT city, temp_lo, temp_hi, prcp, date, location
        FROM weather, cities
        WHERE city = name;

SELECT * FROM myview;
```
