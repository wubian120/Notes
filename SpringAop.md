# Spring AOP 

> AOP是Aspect Oriented Programing的简称，面向切面编程。

>AOP适合于那些具有横切逻辑的应用：如性能监测，访问控制，事务管理、缓存、对象池管理以及日志记录。
>AOP将这些分散在各个业务逻辑中的代码通过横向切割的方式抽取到一个独立的模块中。
>AOP 实现的关键就在于 AOP 框架自动创建的 AOP 代理，AOP 代理则可分为静态代理和动态代理两大类，
>其中静态代理是指使用 AOP 框架提供的命令进行编译，从而在编译阶段就可生成 AOP 代理类，因此也称为编译时增强；
>而动态代理则在运行时借助于 JDK 动态代理、CGLIB 等在内存中“临时”生成 AOP 动态代理类，因此也被称为运行时增强。 
代理对象的方法 = 增强处理 + 被代理对象的方法 



**AOP使用场景** 

 AOP用来封装横切关注点，具体可以在下面的场景中使用

+ Authentication 权限 
+ Caching 缓存 
+ Context passing 内容传递 
+ Error handling 错误处理 
+ Lazy loading　懒加载 
+ Debugging　　调试 
+ logging, tracing, profiling and monitoring　记录跟踪　优化　校准 
+ Performance optimization　性能优化 
+ Persistence　　持久化 
+ Resource pooling　资源池 
+ Synchronization　同步 
+ Transactions 事务 

**AOP相关概念**


- **方面（Aspect）**：一个关注点的模块化，这个关注点实现可能另外横切多个对象。事务管理是J2EE应用中一个很好的横切关注点例子。方面用Spring的 Advisor或拦截器实现。 

- **连接点（Joinpoint）**: 程序执行过程中明确的点，如方法的调用或特定的异常被抛出 
- **通知（Advice）**: 在特定的连接点，AOP框架执行的动作。各种类型的通知包括“around”、“before”和“throws”通知。通知类型将在下面讨论。许多AOP框架包括Spring都是以拦截器做通知模型，维护一个“围绕”连接点的拦截器链。Spring中定义了四个advice: BeforeAdvice, AfterAdvice, ThrowAdvice和DynamicIntroductionAdvice 

- **切入点（Pointcut）**: 指定一个通知将被引发的一系列连接点的集合。AOP框架必须允许开发者指定切入点：例如，使用正则表达式。 Spring定义了Pointcut接口，用来组合MethodMatcher和ClassFilter，可以通过名字很清楚的理解， MethodMatcher是用来检查目标类的方法是否可以被应用此通知，而ClassFilter是用来检查Pointcut是否应该应用到目标类上 

- **引入（Introduction）**: 添加方法或字段到被通知的类。 Spring允许引入新的接口到任何被通知的对象。例如，你可以使用一个引入使任何对象实现 IsModified接口，来简化缓存。Spring中要使用Introduction, 可有通过DelegatingIntroductionInterceptor来实现通知，通过DefaultIntroductionAdvisor来配置Advice和代理类要实现的接口 

- **目标对象（Target Object）**: 包含连接点的对象。也被称作被通知或被代理对象。POJO 
- **AOP代理（AOP Proxy）**: AOP框架创建的对象，包含通知。 在Spring中，AOP代理可以是JDK动态代理或者CGLIB代理。 

- **织入（Weaving）**: 组装方面来创建一个被通知对象。这可以在编译时完成（例如使用AspectJ编译器），也可以在运行时完成。Spring和其他纯Java AOP框架一样，在运行时完成织入。 


Spring 支持四种类型AOP：

- 基于代理的经典SpringAOP
- 纯POJO切面
- @AspectJ 注解驱动的切面
- 注入式AspectJ切面 适用于Spring各版本


[] Spring AOP first
[] 如何配置AOP xml  


日志应用： 
实现登陆和日志管理（使用Spring AOP 

1）LoginService   LogService   TestMain 

2）用Spring 管理  LoginService 和 LogService 的对象 

3）确定哪些连接点是切入点，在配置文件中 

4）将LogService封装为通知 

5）将通知植入到切入点 

6）客户端调用目标 



    <aop:config> 
        <aop:pointcut expression="execution(* cn.com.spring.service.impl.*.*(..))" id="myPointcut"/> 
        <!--将哪个--> 
        <aop:aspect id="dd" ref="logService"> 
          <aop:before method="log" pointcut-ref="myPointcut"/> 
        </aop:aspect> 
    </aop:config> 

execution(* * cn.com.spring.service.impl.*.*(..)) 

1)* 所有的修饰符 

2)* 所有的返回类型 

3)* 所有的类名 

4)* 所有的方法名 

5)* ..所有的参数名 


1.ILoginService.java 


    package cn.com.spring.service; 

    public interface ILoginService { 
        public boolean login(String userName, String password); 
    } 

2.LoginServiceImpl.java 


    package cn.com.spring.service.impl; 

    import cn.com.spring.service.ILoginService; 

    public class LoginServiceImpl implements ILoginService { 

        public boolean login(String userName, String password) { 
            System.out.println("login:" + userName + "," + password); 
            return true; 
        } 

    } 

3.ILogService.java 


    package cn.com.spring.service; 

    import org.aspectj.lang.JoinPoint; 

    public interface ILogService { 
        //无参的日志方法 
        public void log(); 
        //有参的日志方法 
        public void logArg(JoinPoint point); 
        //有参有返回值的方法 
        public void logArgAndReturn(JoinPoint point,Object returnObj); 
    } 


4.LogServiceImpl.java 


    package cn.com.spring.service.impl; 

    import org.aspectj.lang.JoinPoint; 

    import cn.com.spring.service.ILogService; 

    public class LogServiceImpl implements ILogService { 

        @Override 
        public void log() { 
            System.out.println("*************Log*******************"); 
        } 
        
        //有参无返回值的方法 
        public void logArg(JoinPoint point) { 
            //此方法返回的是一个数组，数组中包括request以及ActionCofig等类对象 
            Object[] args = point.getArgs(); 
            System.out.println("目标参数列表："); 
            if (args != null) { 
                for (Object obj : args) { 
                    System.out.println(obj + ","); 
                } 
                System.out.println(); 
            } 
        } 

        //有参并有返回值的方法 
        public void logArgAndReturn(JoinPoint point, Object returnObj) { 
            //此方法返回的是一个数组，数组中包括request以及ActionCofig等类对象 
            Object[] args = point.getArgs(); 
            System.out.println("目标参数列表："); 
            if (args != null) { 
                for (Object obj : args) { 
                    System.out.println(obj + ","); 
                } 
                System.out.println(); 
                System.out.println("执行结果是：" + returnObj); 
            } 
        } 
    } 

5.applicationContext.java 


    <?xml version="1.0" encoding="UTF-8"?> 
    <beans xmlns="http://www.springframework.org/schema/beans" 
        xmlns:aop="http://www.springframework.org/schema/aop" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
        xmlns:p="http://www.springframework.org/schema/p" 
        xsi:schemaLocation="http://www.springframework.org/schema/beans 
        http://www.springframework.org/schema/beans/spring-beans-2.5.xsd 
        http://www.springframework.org/schema/aop 
        http://www.springframework.org/schema/aop/spring-aop-2.5.xsd"> 

        <bean id="logService" class="cn.com.spring.service.impl.LogServiceImpl"></bean> 
        <bean id="loginService" class="cn.com.spring.service.impl.LoginServiceImpl"></bean> 

        <aop:config> 
            <!-- 切入点 --> 
            <aop:pointcut 
                expression="execution(* cn.com.spring.service.impl.LoginServiceImpl.*(..))" 
                id="myPointcut" /> 
            <!-- 切面： 将哪个对象中的哪个方法，织入到哪个切入点 --> 
            <aop:aspect id="dd" ref="logService"> 
                <!-- 前置通知 
                <aop:before method="log" pointcut-ref="myPointcut" /> 
                <aop:after method="logArg" pointcut-ref="myPointcut"> 
        --> 
                <aop:after-returning method="logArgAndReturn" returning="returnObj" pointcut-ref="myPointcut"/> 
            </aop:aspect> 
        </aop:config> 
    </beans> 

6.TestMain.java 


    public class TestMain { 
    public static void testSpringAOP(){ 
            ApplicationContext ctx = new ClassPathXmlApplicationContext("app*.xml"); 
            
            ILoginService loginService = (ILoginService)ctx.getBean("loginService"); 
            loginService.login("zhangsan", "12344"); 
    } 
    public static void main(String[] args) { 
    testSpringAOP(); 
    } 
    } 

7.输出结果： 

    login:zhangsan,12344 
    目标参数列表： 
    zhangsan, 
    12344, 

    执行结果是：true 

解析:1.先调用了login()方法System.out.println("login:" + userName + "," + password); 

     2.再调用了logArgAndReturn()方法输出了日志，并且返回了login()方法是否成功 


### AspectJ 第一个例子

通过注解 和 AspectJ 

[彻底征服 Spring AOP 之 实战篇](https://segmentfault.com/a/1190000007469982)


###参考资料：

[springAOP与自定义注解实现细粒度权限控制管理](http://blog.csdn.net/s8460049/article/details/52182533)
[ 关于 Spring AOP (AspectJ) 你该知晓的一切](http://blog.csdn.net/javazejian/article/details/56267036?utm_source=tuicool&utm_medium=referral)
[Spring in Acton 4 读书笔记之 AOP 原理及 Spring 对 AOP 的支持](http://tantanit.com/springinacton4-du-shu-bi-ji-zhi-aop-yuan-li-ji-spring-dui-aop-de-zhi-chi/?utm_source=tuicool&utm_medium=referral)
[]()
[]()
[]()
[]()
[]()
[]()
[]()
[]()
[]()
[]()
[]()
[]()
[]()
[]()
[]()
[]()
[]()
[]()
[]()
[]()






