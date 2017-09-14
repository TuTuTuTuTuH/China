# 小计  

SQL是对关键字和标识符大小写不敏感的语言，只有在标识符用双引号包围时才能保留它们的大小写。  
```sql
--类型
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

--创建表
CREATE TABLE tablename (
    name       int, --name是列名，int是列的类型，"--"是注释的开头
    ...
); 

--删除表
DROP TABLE tablename; 

--插入一行
INSERT INTO tablename (name1, name2, name3, name5)  --此行括号内容可缺省，加入括号可选择插入哪些列
    VALUES (value1, value2, value3, value5);

--从文本文件中装载大量数据
--源文件的文件名必须在运行后端进程的机器上是可用的， 而不是在客户端上，因为后端进程将直接读取该文件
COPY tablename FROM '/home/user/tablename.txt';


```
