# SQL Join 语句

SQL 中每一种连接操作都包括一个连接类型和连接条件。
**连接类型** 决定了如何处理连接条件不匹配的记录。

连接类型          返回结果
inner join        只包含左右表中满足连接条件的记录
left outer join   在内连接的基础上，加入左表中不与右表匹配的记录，剩余字段赋值为null
right outer join  在内连接的基础上，加入右表中不与左表匹配的记录，剩余字段赋值为null
full outer join   左外连接和右外连接的组合。
cross join        等价于没有连接条件的内连接（即产生笛卡尔乘积）

**连接条件** 决定两个表中哪些记录互相匹配以及连接结果中出现哪些属性。
连接条件            修饰位置      语义
natural             连接类型之前  连接两个表之间的所有公共字段相等的记录，合并相同的列
on <谓词>           连接类型之后  连接符合谓词的记录，不合并相同的列
using(A1, A2,…,An)  连接类型之后  natural 语义的子集，只连接两个表中（A1,A2,..An)的公共字段，合并相同的列

从上面的描述可以看到：连接操作是连接类型和连接条件的组合，只有在这个前提下才能真正的理解连接的功能。

course
+----+--------+------+
| id | cname  | tid  |
+----+--------+------+
|  1 | 数学   |    1 |
|  2 | 英语   |    2 |
|  3 | 语文   |    3 |
|  4 | 体育   |    1 |
|  5 | 物理   | NULL |
+----+--------+------+

teacher
+----+-----------+
| id | name      |
+----+-----------+
|  1 | 王老师    |
|  2 | 李老师    |
|  3 | 张老师    |
|  4 | 肖老师    |
|  5 | NULL      |
|  6 | 陈老师    |
+----+-----------+

student
+----+--------+
| id | name   |
+----+--------+
|  1 | 张三   |
|  2 | 李四   |
|  3 | 王二   |
|  4 | 初一   |
|  5 | 初二   |
+----+--------+



###外连接
1.概念：包括左向外联接、右向外联接或完整外部联接

2.左连接：left join 或 left outer join
![left outer join](./pics/left_outer_join.png)

(1)左向外联接的结果集包括 LEFT OUTER 子句中指定的左表的所有行，而不仅仅是联接列所匹配的行。如果左表的某行在右表中没有匹配行，则在相关联的结果集行中右表的所有选择列表列均为空值(null)。
(2)sql语句
select * from table1 left join table2 on table1.id=table2.id
-------------结果-------------
id name id score
------------------------------
1 lee 1 90
2 zhang 2 100
4 wang NULL NULL
------------------------------
注释：包含table1的所有子句，根据指定条件返回table2相应的字段，不符合的以null显示

3.右连接：right join 或 right outer join
(1)右向外联接是左向外联接的反向联接。将返回右表的所有行。如果右表的某行在左表中没有匹配行，则将为左表返回空值。
(2)sql语句
select * from table1 right join table2 on table1.id=table2.id
-------------结果-------------
id name    id score
------------------------------
1 lee      1 90
2 zhang    2 100
NULL NULL  3 70
------------------------------
注释：包含table2的所有子句，根据指定条件返回table1相应的字段，不符合的以null显示

### 完整外部联接:full join 或 full outer join 
(1)完整外部联接返回左表和右表中的所有行。当某行在另一个表中没有匹配行时，则另一个表的选择列表列包含空值。如果表之间有匹配行，则整个结果集行包含基表的数据值。
![full outer join](./pics/full_outer_join.png)
(2)sql语句
select * from table1 full join table2 on table1.id=table2.id
-------------结果-------------
id name id score
------------------------------
1 lee 1 90
2 zhang 2 100
4 wang NULL NULL
NULL NULL 3 70
------------------------------
注释：返回左右连接的和（见上左、右连接）

###内连接
1.概念：内联接是用比较运算符比较要联接列的值的联接

2.内连接：join 或 inner join
3.sql语句
select * from table1 join table2 on table1.id=table2.id
-------------结果-------------
id name id score
------------------------------
1 lee 1 90
2 zhang 2 100
------------------------------
注释：只返回符合条件的table1和table2的列,**内联合（inner join）只生成同时匹配表A和表B的记录集**

4.等价（与下列执行效果相同）
A:select a.*,b.* from table1 a,table2 b where a.id=b.id
B:select * from table1 cross join table2 where table1.id=table2.id  (注：cross join后加条件只能用where,不能用on)
![Inner Join](./pics/inner_join.png)


### 全外联合 用where排除 两边不想要的记录

```
SELECT <select_list>
FROM Table_A A
LEFT JOIN Table_B B
ON A.Key = B.Key
WHERE B.Key IS NULL
```
返回表A中存在但是B中不存在的记录
![](./pics/left_excluding_join.png)

```
SELECT <select_list> 
FROM Table_A A
RIGHT JOIN Table_B B
ON A.Key = B.Key
WHERE A.Key IS NULL
```
返回表 B 中存在但是表 A 中不存在的记录。
![](./pics/right_excluding_join.png)

```
SELECT <select_list>
FROM Table_A A
FULL OUTER JOIN Table_B B
ON A.Key = B.Key
WHERE A.Key IS NULL OR B.Key IS NULL
```
返回表 A 和表 B 中非共同存在的所有记录。

![](./pics/outer_excluding_join.png)

###交叉连接(完全)

1.概念：没有 WHERE 子句的交叉联接将产生联接所涉及的表的笛卡尔积。第一个表的行数乘以第二个表的行数等于笛卡尔积结果集的大小。（table1和table2交叉连接产生3*3=9条记录）

2.交叉连接：cross join (不带条件where...)

3.sql语句
select * from table1 cross join table2
-------------结果-------------
id name id score
------------------------------
1 lee 1 90
2 zhang 1 90
4 wang 1 90
1 lee 2 100
2 zhang 2 100
4 wang 2 100
1 lee 3 70
2 zhang 3 70
4 wang 3 70
------------------------------
注释：返回3*3=9条记录，即笛卡尔积

4.等价（与下列执行效果相同）
A:select * from table1,table2   


[常见的几种join的方法](http://bbs.csdn.net/topics/300232258)
[SQL JOIN用法](http://www.cnblogs.com/fatway/archive/2009/04/17/1693816.html)
[三言两语：SQL 连接（join）](http://blog.xiaohansong.com/2016/04/14/sql-join/?utm_source=tuicool&utm_medium=referral)
[SQL关联查询的使用技巧 （各种 join）](http://www.cnblogs.com/fzstruggle/p/5735301.html?utm_source=tuicool&utm_medium=referral)
[]()
[postgreSQL中的内连接和外连接](http://blog.csdn.net/u011008029/article/details/49884375)
[数据库基础：SQL join 语句](http://www.jianshu.com/p/3a47800bd159)
[画图解释 SQL join 语句](https://linux.cn/article-5837-rss.html?utm_source=tuicool&utm_medium=referral)
[PostgreSQL JOINS子句](http://www.yiibai.com/html/postgresql/2013/080569.html)



