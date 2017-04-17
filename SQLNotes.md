## SQL语句 Notes


### create index 创建索引

在不读取整个表的情况下，索引使数据库应用程序可以更快地查找数据。

用户无法看到索引，它们只能被用来加速搜索/查询。

**注释：更新一个包含索引的表需要比更新一个没有索引的表更多的时间，这是由于索引本身也需要更新。因此，理想的做法是仅仅在常常被搜索的列（以及表）上面创建索引。**

SQL CREATE INDEX 语法

在表上创建一个简单的索引。允许使用重复的值：
```
CREATE INDEX index_name ON table_name (column_name)
```
注释："column_name" 规定需要索引的列。




### truncate
PostgreSQL的truncate table命令用于从现有的表删除完整的数据。还可以使用DROP TABLE命令删除整个表及表结构，它会删除数据库，需要再次重新创建该表。 

作为在每个表上的DELETE（删除）具有相同的效果，但是，因为它没有实际扫描的表，它的速度快。此外，紧接回收磁盘空间，而不是要求随后的VACUUM操作。在大表上，这是最有用。
[PostgreSQL TRUNCATE TABLE命令](http://www.yiibai.com/html/postgresql/2013/080676.html)


### alter

ALTER TABLE 语句用于在已有的表中添加、修改或删除列。
```
ALTER TABLE table_name ADD column_name datatype
```
要删除表中的列，请使用下列语法：
```
ALTER TABLE table_name  DROP COLUMN column_name
```

```
-- pg 中 给列加上 序列 用于自增
alter table trucklocation alter column id set default nextval('truck_id_seq');
```

```
ALTER TABLE Persons
ADD Birthday date
```
请注意，新列 "Birthday" 的类型是 date，可以存放日期。数据类型规定列中可以存放的数据的类型。


### distinct

关键词 DISTINCT 用于返回唯一不同的值。

如需从 Company" 列中仅选取唯一不同的值，我们需要使用 SELECT DISTINCT 语句：
SELECT DISTINCT Company FROM Orders 

### where
如需有条件地从表中选取数据，可将 WHERE 子句添加到 SELECT 语句。
---------------------------------
|操作符  | 描述
|=       | 等于
|<>      | 不等于
|>       | 大于
|<       | 小于
|>=      | 大于等于
|<=      | 小于等于
|BETWEEN | 在某个范围内
|LIKE    | 搜索某种模式
------------------------------
SQL 使用单引号来环绕文本值（大部分数据库系统也接受双引号）。如果是数值，请不要使用引号。
```
SELECT * FROM Persons WHERE FirstName='Bush'
```


### update
修改 表中的数据

UPDATE 表名称 SET 列名称 = 新值 WHERE 列名称 = 某值
```
UPDATE Person SET FirstName = 'Fred' WHERE LastName = 'Wilson' 

-- 更新某一行中的若干列
UPDATE Person SET Address = 'Zhongshan 23', City = 'Nanjing'
WHERE LastName = 'Wilson'
```

### 通配符
SQL 通配符必须与 LIKE 运算符一起使用。SQL 通配符可以替代一个或多个字符。
-----------------------------------
通配符            描述
%                 替代一个或多个字符
_                  仅替代一个字符
[charlist]        字符列中的任何单一字符
[^charlist]
或者
[!charlist]     不在字符列中的任何单
------------

#### %通配符
现在，我们希望从上面的 "Persons" 表中选取居住在以 "Ne" 开始的城市里的人：
我们可以使用下面的 SELECT 语句：
```
SELECT * FROM Persons WHERE City LIKE 'Ne%'
```

#### [charlist]通配符

现在，我们希望从上面的 "Persons" 表中选取居住的城市以 "A" 或 "L" 或 "N" 开头的人：
我们可以使用下面的 SELECT 语句：
```
SELECT * FROM Persons WHERE City LIKE '[ALN]%'
```

现在，我们希望从上面的 "Persons" 表中选取居住的城市不以 "A" 或 "L" 或 "N" 开头的人：
我们可以使用下面的 SELECT 语句：

```
SELECT * FROM Persons WHERE City LIKE '[!ALN]%'
```


#### _通配符

现在，我们希望从上面的 "Persons" 表中选取名字的第一个字符之后是 "eorge" 的人：
我们可以使用下面的 SELECT 语句：
```
SELECT * FROM Persons WHERE FirstName LIKE '_eorge'
```




### IN 操作符

```
SELECT column_name(s)
FROM table_name
WHERE column_name IN (value1,value2,...)
```












[锋利的SQL: 取出多列中的非空值](http://blog.csdn.net/zhanghongju/article/details/17562929)
[what-happens-when-zh_CN](https://github.com/skyline75489/what-happens-when-zh_CN)
[sql server日期比较  ](http://jsdjt.blog.163.com/blog/static/7771121200922092018404/)
[一些常用SQL语句大全](http://www.cnblogs.com/acpe/p/4970765.html)
[SQL查询重复记录](http://www.cnblogs.com/caotang/archive/2011/01/18/1937932.html)
[]()
[W3school SQL WHERE 子句](http://www.w3school.com.cn/sql/sql_where.asp)
[]()
[]()
[]()
[]()
[]()
[]()
[]()
[]()




