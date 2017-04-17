# Spring MyBatis


1. 新建 spring mvc 项目

2. 配置pom.xml 


配置web.xml 
```
<web-app xmlns="http://java.sun.com/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
          http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         version="3.0">
  <display-name>Archetype Created Web Application</display-name>

  <servlet>
    <servlet-name>main-dispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:gate-servlet.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
    <!--异步支持-->
    <async-supported>true</async-supported>
  </servlet>

  <servlet-mapping>
    <servlet-name>main-dispatcher</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>

  <servlet>
    <servlet-name>DruidStatView</servlet-name>
    <servlet-class>com.alibaba.druid.support.http.StatViewServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>DruidStatView</servlet-name>
    <url-pattern>/druid/*</url-pattern>
  </servlet-mapping>

  <filter>
    <filter-name>druidWebStatFilter</filter-name>
    <filter-class>com.alibaba.druid.support.http.WebStatFilter</filter-class>
    <init-param>
      <param-name>exclusions</param-name>
      <param-value>/public/*,*.js,*.css,/druid*,*.jsp,*.swf</param-value>
    </init-param>
  </filter>
<filter-mapping>
  <filter-name>druidWebStatFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>

  <filter>
    <filter-name>encodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <async-supported>true</async-supported>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
      <param-name>forceEncoding</param-name>
      <param-value>true</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>encodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
  
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
  </welcome-file-list>

  <error-page>
    <exception-type>java.lang.NullPointerException</exception-type>
    <location>/pages/error.html</location>
  </error-page>
  <error-page>
    <error-code>404</error-code>
    <location>/pages/error.html</location>
  </error-page>
  <error-page>
    <error-code>500</error-code>
    <location>/pages/error.html</location>
  </error-page>
</web-app>

```


3. 配置web.xml ，gate-servlet.xml (SpringMVC.xml) , (mybatis.xml)

4. druid Druid是Java语言中最好的数据库连接池。Druid能够提供强大的监控和扩展功能。
[常见问题](https://github.com/alibaba/druid/wiki/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)


5. 

[](http://blog.csdn.net/freeland1)

[](http://www.cnblogs.com/liuyang0/p/6445211.html?utm_source=tuicool&utm_medium=referral)


##主要步骤

- pom.xml
- resources 下面配置文件:
  
  jdbc.properties, 数据库配置    config.properties 
  spring-mvc.xml,  spring mvc 配置
  spring-mybatis.xml,  mybatis 数据源  druid
  log4j.properties    日志配置文件
     




- Controller
- mapping/*Mapper.xml
- dao/*Mapper
- 



## 主要步骤

1. SqlMapConfig.xml

[](http://elim.iteye.com/blog/1841033)

##参考资料
[Spring MVC入门第2天--Spring、SpringMVC与MyBatis三大框架整合](http://blog.csdn.net/lutianfeiml/article/details/51804535?utm_source=tuicool&utm_medium=referral)
[手刃SSM(Spring+SpringMVC+Mybatis)--第一弹](http://www.jianshu.com/p/c4e8ff157bdc)
[springmvc+mybatis学习笔记(汇总)](http://blog.csdn.net/h3243212/article/details/51016271)
[IDEA下创建Maven项目，并整合使用Spring、Spring MVC、Mybatis框架](http://www.cnblogs.com/liuyang0/p/6445211.html?utm_source=tuicool&utm_medium=referral)
[Spring+SpringMVC+MyBatis+Maven整合](http://www.jianshu.com/p/1ceeada223cf)
[SpringMVC干货系列：从零搭建SpringMVC+mybatis（二）：springMVC原理解析及常用注解](http://www.jianshu.com/p/135693f589c4?utm_source=tuicool&utm_medium=referral)
[通过项目逐步深入了解Mybatis<三>](http://blog.csdn.net/tzs_1041218129/article/details/53456677)
[]()
[]()
[]()
[]()
[]()
[]()







