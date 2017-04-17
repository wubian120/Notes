#Spring JPA


###多表查询
多表查询在spring data jpa中有两种实现方式，第一种是利用hibernate的级联查询来实现，第二种是创建一个结果集的接口来接收连表查询后的结果，这里主要第二种方式。

[Spring Boot系列(五)：spring data jpa的使用](http://www.jianshu.com/p/63ab3b985144)


###Spring 中 Service Component 区别
In Spring 2.0 and later, the @Repository annotation is a marker for any class that fulfills the role or stereotype (also known as Data Access Object or DAO) of a repository. Among the uses of this marker is the automatic translation of exceptions.

Spring 2.5 introduces further stereotype annotations: @Component, @Service, and @Controller. @Component is a generic stereotype for any Spring-managed component. @Repository, @Service, and @Controller are specializations of @Component for more specific use cases, for example, in the persistence, service, and presentation layers, respectively.

Therefore, you can annotate your component classes with @Component, but by annotating them with @Repository, @Service, or @Controller instead, your classes are more properly suited for processing by tools or associating with aspects. For example, these stereotype annotations make ideal targets for pointcuts.

Thus, if you are choosing between using @Component or @Service for your service layer, @Service is clearly the better choice. Similarly, as stated above, @Repository is already supported as a marker for automatic exception translation in your persistence layer.

 Annotation | Meaning                                             |
+------------+-----------------------------------------------------+
| @Component | generic stereotype for any Spring-managed component |
| @Repository| stereotype for persistence layer                    |
| @Service   | stereotype for service layer                        |
| @Controller| stereotype for presentation layer (spring-mvc) 



[](http://blog.csdn.net/xiao190128/article/details/54890759)

### 参考资料：
[dyc87112/SpringBoot-Learning](https://github.com/dyc87112/SpringBoot-Learning)
[程序猿DD](http://blog.didispace.com/)
[JPA EntitManager进阶](http://www.tuicool.com/articles/BFBJfqa)
[JPA EntityManager高级](http://blog.csdn.net/han_yankun2009/article/details/45401935)
[ JPA EntitManager进阶](http://blog.csdn.net/han_yankun2009/article/details/45395271)
[持久化API（JPA）系列(四)管理器EntityManager--执行数据库更新](http://blog.csdn.net/zhaolijing2012/article/details/44783049?utm_source=tuicool&utm_medium=referral)
[持久化API（JPA）系列(三)实体Bean的开发技术-建立与数据库的连接](http://blog.csdn.net/zhaolijing2012/article/details/44775589?utm_source=tuicool&utm_medium=referral)
[JPA中的EntityManager](http://blog.csdn.net/wangyongxia921/article/details/44224479?utm_source=tuicool&utm_medium=referral)


[Spring Boot中使用Spring-data-jpa让数据访问更简单、更优雅](http://blog.didispace.com/springbootdata2/?utm_source=tuicool&utm_medium=referral)
[JPA - hibernate 的各种常见用法](http://www.cnblogs.com/fengru/p/5922793.html?utm_source=tuicool&utm_medium=referral)
[Spring Boot JPA使用](http://www.cnblogs.com/ityouknow/p/5891443.html)
[Sping Data Jpa 一对多单向映射](http://blog.csdn.net/strommaybin/article/details/54582598?utm_source=tuicool&utm_medium=referral)
[ JPA学习笔记（10）——映射双向多对多关联关系](http://blog.csdn.net/u010837612/article/details/47808975?utm_source=tuicool&utm_medium=referral)
[JPA](http://blog.csdn.net/u010837612/article/category/5732811)
[]()
[JavaEE – JPA（3）：Spring Framework中的事务管理](http://www.importnew.com/22570.html?utm_source=tuicool&utm_medium=referral)
[]()
[jpa多表查询](http://www.cnblogs.com/linjiqin/archive/2011/02/11/1951375.html)

[Spring Hibernate JPA 联表查询 复杂查询](http://www.cnblogs.com/jiangxiaoyaoblog/p/5635152.html)
[](JPA的查询语言—JPQL的关联查询)
[JavaEE – JPA（4）：EntityManager相关核心概念](http://www.importnew.com/22576.html?utm_source=tuicool&utm_medium=referral)

[使用 Spring JPA 建立一对多关联](http://blog.tangjiujun.com/jpa-one-to-many.html?utm_source=tuicool&utm_medium=referral)
[使用 Spring JPA 建立一对一双向关联](http://blog.tangjiujun.com/jpa-one-to-one.html?utm_source=tuicool&utm_medium=referral)
[JPA 菜鸟教程](http://www.jianshu.com/c/1b694d6f459a)
[]()





