#Postgresql 

## 目录

- JDBC连接查询数据库
- 数据库常用优化办法
- PostgreSQL MySQL 数据类型映射
- 表设计，一对多，多对多，一对一



[PostgreSQL MySQL 数据类型映射](https://yq.aliyun.com/articles/69388?utm_campaign=wenzhang&utm_medium=article&utm_source=QQ-qun&utm_content=m_10150)


## 数据库常用优化办法


[性能优化之数据库优化](http://www.trinea.cn/android/database-performance/)
[面试常见问题--数据库优化 百万数据怎么优化](http://blog.csdn.net/libing13820393394/article/details/48634525)
[]()
[]()
[]()
[]()
[]()


##JDBC连接查询数据库

```
public class ConnectionTest {

    public static final String driver = "org.postgresql.Driver";
    public static final String url = "jdbc:postgresql://localhost:5432/demo";
    public static final String user = "postgres";
    public static final String pass = "123456";
    public static void main(String[] args) throws Exception{

        Connection con      = null;
        Statement statement = null;
        ResultSet result    = null;

        Class.forName(driver);
        con = DriverManager.getConnection(url,user,pass);
        System.out.println(con);
        statement = con.createStatement();
        result = statement.executeQuery("select * from public.user");
        while (result.next()){   
            String name = result.getString("name");
            System.out.println(name);
        }
        con.close();
    }
}

```

## 表设计，一对多，多对多，一对一

- 关联映射,一对多

球员与球队，老师与课程
一对多， 球队角度，一个球队可以拥有多个球员，一个老师可以教多个课程   一对多
多对一， 球员角度，多个球员属于一个球队，   多个课程可以属于一个老师  多对一

![](./pics/db_one_to_many.jpg)

- 关联映射，一对一 两种，外键关联，主键关联

一对一关系就如球队与球队所在地址之间的关系，一支球队仅有一个地址，而一个地址区也仅有一支球队。
![](./pics/db_one_to_one_fk.jpg)

一对一主键关联：要求两个表的主键必须完全一致，通过两个表的主键建立关联关系。图示如下：
![](./pics/db_one_to_one_pk.jpg)

关联映射：多对多
多对多关系也很常见，例如学生与选修课之间的关系，一个学生可以选择多门选修课，而每个选修课又可以被多名学生选择。
数据库中的多对多关联关系一般需采用中间表的方式处理，将多对多转化为两个一对多。

![](./pics/db_many_to_many.jpg)

[数据库设计(一对一、一对多、多对多）](http://www.cnblogs.com/lgxlsm/archive/2013/05/15/3080994.html)
##参考资料

[JAVA JDBC](http://blog.csdn.net/iquicksandi/article/details/8545066)
