#Spring boot 笔记

## 简介
> Spring Boot ，个人理解主要目的是为了替代繁琐的xml配置文件。
> [官网](http://projects.spring.io/spring-boot/)

## hello world 示例
第一个示例，我没有采用网上教程普遍采用的IDEA的 Spring Initializr来初始化项目（加载非常慢）。通过创建maven项目，并配置pom.xml可以达到同样的效果。
**1.创建maven web项目，修改pom.xml**

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>cn.brady</groupId>
  <artifactId>SpringBootWebService</artifactId>
  <packaging>war</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>SpringBootWebService Maven Webapp</name>
  <url>http://maven.apache.org</url>

  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.1.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
  </parent>
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    <java.version>1.8</java.version>
  </properties>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web-services</artifactId>
    </dependency>
  </dependencies>
  <build>
    <finalName>SpringBootWebService</finalName>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>

</project>

```
**2.目录结构**
```
+- cn
  +- brady
    +- Application.java
    |
    +- controller
    |  +- MainController.java
    +- service
    |  +- service.java
    +- domain
    |  +- User.java
    |  
```

**3.Application.java**
```
@SpringBootApplication
//@ComponentScan(basePackages = {"cn.brady.*"})
public class Application {

    public static void main(String[] args){
        SpringApplication.run(Application.class, args);
    }
}

```

**4.controller**
 
```
@RestController
public class MainController {

    @RequestMapping("/hello")
    public String index(){
        return "Spring boot One";
    }
}
```

## RESTful Api
```
    @RequestMapping("/getUser")
    public User getUser(){
        User user = new User();
        user.setAge(20);
        user.setName("John");
        user.setId(10001L);

        return user;
    }
```



```
浏览器：http://localhost:8080/getUser
输出：{"id":10001,"name":"John","age":20}
```

## 单元测试
```
@SpringApplicationConfiguration(classes = MockServletContext.class) //已被废弃， 改为SpringBootTest,如下：
@SpringBootTest(classes = MockServletContext.class)
```
** 完整代码 ** 
```
@org.junit.runner.RunWith(SpringJUnit4ClassRunner.class)
@SpringBootTest(classes = MockServletContext.class)
@WebAppConfiguration
public class RestControllerTest {

    private MockMvc mvc;

    @Before
    public void setUp() throws Exception{
        mvc = MockMvcBuilders.standaloneSetup(new MainController()).build();
    }
    @Test
    public void getHello() throws Exception{
        mvc.perform(get("/hello").accept(MediaType.APPLICATION_JSON_UTF8))
                .andExpect(status().isOk())
                .andDo(MockMvcResultHandlers.print())
                .andReturn();
    }

    @Test
    public void UserControllerTests() throws Exception{
        RequestBuilder rb = null;

        rb = get("/getUser");
        mvc.perform(rb)
                .andExpect(status().isOk())
                .andExpect(content().string("{\"id\":10001,\"name\":\"John\",\"age\":20}"));
    }
    
}
```


##通过JPA集成PostgreSQL

**在resources目录下，增加application.properties**
```
spring.datasource.url=jdbc:postgresql://localhost:5432/demo
spring.datasource.username=postgres
spring.datasource.password=123456
spring.datasource.driver-class-name=org.postgresql.Driver

spring.jpa.properties.hibernate.hbm2ddl.auto=update
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQL9Dialect
spring.jpa.show-sql= true

```

**新建Entity**
```
@Entity
@Table(name="user", schema="public", catalog = "demo")
public class UserEntity implements Serializable {

    private int id;
    private String name;
    private Integer age;
    private Date birthday;
    private String password;

    @Id
    @Column(name = "id", nullable = false)
    public int getId() {return id;}
    public void setId(int id) {
        this.id = id;
    }
    @Basic
    @Column(name = "name", nullable = true, length = 100)
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    @Basic
    @Column(name = "age", nullable = true)
    public Integer getAge() {
        return age;
    }
    public void setAge(Integer age) {
        this.age = age;
    }
    @Basic
    @Column(name = "birthday", nullable = true)
    public void setBirthday(Date birthday) {this.birthday = birthday;}
    public Date getBirthday() {return birthday;}

    @Basic
    @Column(name = "password", nullable = true, length = 32)
    public String getPassword() {return password;}
    public void setPassword(String password) {this.password = password;}

    @Override
    public String toString() {
        return "ID: "+ id + " name: " + name + " age: " + age+ " birthday: "+ birthday.toString();
    }
}
```

**新建UserRepository.java**
```
public interface UserRepository extends JpaRepository<UserEntity, Integer> {

}
```

测试
```
@RunWith(SpringJUnit4ClassRunner.class)
@SpringBootTest(classes = Application.class)
public class UserRepositoryTests {

    @Autowired
    private UserRepository repository;

    @Test
    public void test() throws Exception{
        List<UserEntity> users = repository.findAll();
        for(UserEntity user : users){
            System.out.println(user.getName());
        }

        System.out.println(users.size());

    }
}
```

## 集成日志



[Spring Boot 揭秘与实战（三） 日志框架篇 - 如何快速集成日志系统](http://blog.720ui.com/2016/springboot_03_logging/?utm_source=tuicool&utm_medium=referral)


## 下一步... 持续更新
集成Redis





## 参考资料：

[把spring-boot项目部署到tomcat容器中](http://blog.csdn.net/javahighness/article/details/52515226)
[深入学习微框架：Spring Boot](http://www.infoq.com/cn/articles/microframeworks1-spring-boot)
[Spring Boot开发之流水无情（二）](https://my.oschina.net/u/1027043/blog/406558)
[Spring Boot系列(一)：Spring Boot 入门篇](https://zhuanlan.zhihu.com/p/24957789?refer=dreawer)
[Spring Boot构建RESTful API与单元测试](http://www.jianshu.com/p/9698bf98fab1)
[从零开始学习Spirng Boot—常见异常汇总](http://www.iteye.com/topic/1144685)


[ SpringBoot JPA实现增删改查、分页、排序、事务操作等功能](http://blog.csdn.net/linzhiqiang0316/article/details/52639265)


[SpringBoot+shiro整合学习之登录认证和权限控制](http://z77z.oschina.io/2017/02/13/SpringBoot+shiro%E6%95%B4%E5%90%88%E5%AD%A6%E4%B9%A0%E4%B9%8B%E7%99%BB%E5%BD%95%E8%AE%A4%E8%AF%81%E5%92%8C%E6%9D%83%E9%99%90%E6%8E%A7%E5%88%B6/)


[SpringBoot+Shiro学习之数据库动态权限管理和Redis缓存](http://z77z.oschina.io/2017/02/17/SpringBoot+Shiro%E5%AD%A6%E4%B9%A0%E4%B9%8B%E6%95%B0%E6%8D%AE%E5%BA%93%E5%8A%A8%E6%80%81%E6%9D%83%E9%99%90%E7%AE%A1%E7%90%86%E5%92%8CRedis%E7%BC%93%E5%AD%98/)
[SpringBootForBeginners](https://github.com/in28minutes/SpringBootForBeginners)
[Springboot的学习记录](https://github.com/zsl131/spring-boot-test)
[]()
[]()
[]()
[]()
[]()
[]()



