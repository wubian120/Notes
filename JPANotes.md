##Spring Data JPA note



持久化 最重要的两个方面：
1. 事务管理   Transaction Management
2. 对象关系映射 Object Relational Mapping

### 事务 Transaction 

- 原子性 Atomicity
- 隔离性 Isolation
- 一致性 Consistency
- 持久性 Durability

ACID 用来描述**事务**的特点 

JPA Java Persistence Api Java持久化的规范

Java EE 事务两类：
1. Resource-local 事务
2. Container      事务

Resource -local    JDBC DataSource接口打交道， 数据库事务。
Container 事务     JTA Java Transaction API 事务。  将企业级资源涵盖到一个事务中。



事务划分(Transaction Demarcation)

我们已经知道了事务实际上是将一系列操作打了个包，形成了一个”原子”操作。那么事务划分所要解决的问题就是如何规定事务从哪里开始，到哪里结束。

- Resource-local事务

对于这种最基本的事务类型，都是由开发人员直接通过编码地方式来完成事务划分。比如下面这类常见操作：

tx.begin();    // 事务开始
// ......
tx.commit();  // 事务提交

所以事务划分听上去有点吓人，实际上放到Resource-local事务类型中来，就是两行代码而已(如果考虑到可能出现的tx.rollback()，则是三行代码)。

- Container事务

对于容器中的事务，除了像上面那样由开发人员直接划分，还多了一种选择，就是让容器来帮你划分。归纳一下就是下面的两种方案：

使用JTA接口在应用中编码完成显式划分
在容器的帮助下完成自动划分
由于JPA作为JavaEE规范的一部分，对同属于JavaEE规范中的EJB作了充分考虑，因此对于EJB而言，上面的两种方案可以被具体描述为：

对于第一种情况，利用JTA提供的接口完成的划分，可以被称为基于Bean的事务(Bean-managed Transaction，BMT)。 
对于第二种情况，利用容器完成事务的自动划分的，可以被成为基于容器的事务(Container-managed Transaction，CMT)。

### Spring Framework事务管理概要

上面两种方案放到Spring Framework这个语境中，是这样描述的：

对于第一种情况，被称为编程式的事务管理(Programmatic Transaction Management)。 
对于第二种情况，被称为声明式的事务管理(Declarative Transaction Management)。

Spring Framework通过引入一个名为事务策略(Transaction Strategy)的概念来建立这个抽象。具体而言，表现为下面这个接口：

```
public interface PlatformTransactionManager {
    TransactionStatus getTransaction(TransactionDefinition definition) throws TransactionException;
    void commit(TransactionStatus status) throws TransactionException;
    void rollback(TransactionStatus status) throws TransactionException;
}
```

### JPA 核心概念

- EntityManager 
- EntityManagerFactory
- PersistenceContext
- PersistenceUnit
- Persistence

![JPA core concept](./pics/jpa-concept.png)

```
public interface EntityManager {
    public void persist(Object entity);
    public <T> T merge(T entity);
    public void remove(Object entity);
    public <T> T find(Class<T> entityClass, Object primaryKey);
    // ......
}
```

只有@Entity 才能被JPA与数据库中记录相互转换。
两种方式实现 实体状态的访问：

1. Field    注解放在 字段上
2. Property 也就是Getter/Setter， 

3. 混合模式
@Access 注解 


- 在类声明上使用@Access(AccessType.FIELD)来表明此类是基于字段的访问方式。
- 在Getter方法使用@Access(AccessType.PROPERTY)来表明此字段是基于Getter的访问方式。 

```
@Entity
@Access(AccessType.FIELD)
public class Employee {

    public static final String LOCAL_AREA_CODE = "613";
    @Id 
    private int id;
    @Transient 
    private String phoneNum;

    public int getId() { return id; }
    public void setId(int id) { this.id = id; }
    public String getPhoneNumber() { return phoneNum; }
    public void setPhoneNumber(String num) { this.phoneNum = num; }

    @Access(AccessType.PROPERTY) 
    @Column(name="PHONE")
    protected String getPhoneNumberForDb() {
        if (phoneNum.length() == 10)
            return phoneNum;
        else
            return LOCAL_AREA_CODE + phoneNum;
    }
    protected void setPhoneNumberForDb(String num) {
        if (num.startsWith(LOCAL_AREA_CODE))
            phoneNum = num.substring(3);
        else
            phoneNum = num;
        }
    }
}
```


ORM注解类型
1. 逻辑关系注解 logical annotation
2. 物理关系注解 physical Annotation


###表映射

最少的注解需要：
- @Entity
- @Id

所有的其它配置都会使用默认值。比如数据库表名，如果不指定的话，默认使用的就是实体类型的名称，比如上面的Employee类型，映射的数据库表名即为Employee。

如果需要指定表名，则需要使用@Table注解，@Table除了支持表名的指定外，还支持schema以及catalog的指定。当然，每种数据库对它们的支持都有所不同。下面列举了几种主流数据库的支持情况：


###主键映射以及生成策略

- 简单类型：byte，int，short，long，char
- 相应的包装类型：Byte，Integer，Short，Long，Character
- 字符串类型：String
- 时间类型：Date

主键的生成策略通过@GeneratedValue
- 预先分配
- 持久化时分配

具体类型：@GerneratedValue strategy设定
1. AUTO  默认生成策略，
2. TABLE  提供一种与具体数据库无关的主键生成策略，
这种方式使用一张数据库表来完成主键的生成。这张表中应该有两列，一列的类型为字符串，表明生成器的名字，它也是这张表中的主键。另一列的类型应该是一个整型，它用来记录最后分配过的数值：

3. SEQUENCE
4. IDENTITY
``` //AUTO
@Id
@GeneratedValue(strategy = GenerationType.AUTO)
private Long id;
```

```//TABLE 
@TableGenerator(name = "Emp_Gen", table = "ID_GEN", pkColumnName = "GEN_NAME", valueColumnName = "GEN_VAL")
@Id
@GeneratedValue(strategy = GenerationType.TABLE, generator = "Emp_Gen")
private Long id;
```
声明了用于生成主键的表名为ID_GEN，表中的GEN_NAME列用来保存生成器的名字，即上述Emp_Gen，GEN_VAL用来保存最后分配过的主键值。
可以推断出ID_GEN的结构可以是这样的：
GEN_NAME    GEN_VAL
Emp_Gen     0

上述Emp_Gen生成器的GEN_VAL为0表示当前还没有分配过主键。如果在生成这个表的事后希望给GEN_VAL一个初始值，还可以试用initialValue属性进行指定。

为了避免频繁更新ID_GEN表中的记录，可以试用allocationSize来声明需要预分配多少个主键。它的默认值是50，这表示应用程序可以最多分配50个主键后才需要再次访问和更新ID_GEN表。
```
@TableGenerator(name="Address_Gen", table="ID_GEN", pkColumnName="GEN_NAME", valueColumnName="GEN_VAL", pkColumnValue="Addr_Gen", initialValue=10000, allocationSize=100)
@Id 
@GeneratedValue(generator="Address_Gen")
private int id;

```
GEN_NAME    GEN_VAL
Emp_Gen         0
Addr_Gen       10000

``` //SEQUENCE
@SequenceGenerator(name="Emp_Gen", sequenceName="Emp_Seq")
@Id 
@GeneratedValue(generator="Emp_Gen")
private int id;

```
在没有开启结构自动生成时，需要保证序列是存在的：(需要在数据库先生成序列)


``` //IDENTITY  
@Id 
@GeneratedValue(strategy=GenerationType.IDENTITY)
private int id;
```
常使用MySQL的开发人员肯定知道自增列这一特性，这其实就是IDENTITY这种生成策略所依赖的关键特性。这种策略并非像TABLE和SEQUENCE那样会预分配一段可用的主键，它生成的主键只有在数据Commit之后才会可见。因此在采用该生成策略时，对于未被托管的实体对象而言，其主键通常都是不可用的。JPA实现需要在完成实体的持久化之后再次读取该记录，以此来获取被数据库分配的主键信息。该策略的配置方法如下所示：

###ORM核心注解 

####基础类型映射
Java 数据类型 与 数据库 数据类型 映射 包括9种：

1. 简单类型：byte，int，short，long，boolean，char，float以及double
2. 简单类型对应的包装类型：Byte，Integer，Short，Long，Boolean，Character，Float以及Double
3. 字节以及字符数组：byte[]，Byte[]，char[]以及Character[]
4. 能够表示大数值的类型：java.math.BigInteger以及java.math.BigDecimal
5. 字符串类型：String
6. Java时间类型：java.util.Date以及java.util.Calendar
7. JDBC时间类型：java.sql.Date，java.sql.Time以及java.sql.Timestamp
8. 枚举类型
9. 可序列化的对象(Serializable Object)

@Basic 标明某个属性是否需要被持久化。

@Column 注解  
列物理映射
![列映射](./pics/column_annotation.png)

##### 懒加载
```
@Basic(fetch=FetchType.LAZY)
@Column
private String article;
```

#### 大对象 Large Object
```
@Entity
public class Employee {
    @Id
    private int id;

    @Basic(fetch=FetchType.LAZY)
    @Lob 
    @Column(name="PIC")
    private byte[] picture;
    // ...
}
```

#### 枚举类型 Enumerated Type

**ordinal()和name()它们的作用分别是得到枚举值的声明顺序和获取枚举值的声明名称。**
当枚举类型被映射到数据库表中的时候，默认使用的是ordinal()方法的返回值。如果需要使用name()方法的返回值作为映射后的值，可以使用下面的方式声明：
```
@Enumerated(EnumType.STRING)
private EmployeeType type;

```

#### 时间类型 Temporal Type
- Java时间类型：java.util.Date以及java.util.Calendar
- JDBC时间类型：java.sql.Date，java.sql.Time以及java.sql.Timestamp

对于第二种JDBC的时间类型，不需要任何注解就能完成正确的映射和转换。 
对于第一种Java的时间类型，就需要使用一些注解了。主要是为了告知JPA实现在映射和转换时使用哪一种JDBC时间类型(因此，最终还是使用的JDBC时间类型)。通过@Temporal注解来指定：
```
@Temporal(TemporalType.DATE)
private Calendar dob;

@Temporal(TemporalType.TIMESTAMP)
private Date startTime;

```

#### 瞬态属性(Transient State Attribute)

如果一个属性它属于某个实体类型，但是在持久化到数据库中的时候，又不需要将它也映射到表结构中，就可以将它设置成”瞬态属性”。

可以通过两种方式实现：

使用transient关键字
使用@Transient注解

至于它俩的区别？那要从transient这个关键字的其它用法说起了。我们都知道transient是Java语言中的一个关键字，它并非为JPA而设计。该关键字最典型意义是当一个对象参与到序列化的过程中时，被transient修饰的字段是不会参与序列化的。知道了这一点，这两种实现方式的区别也就很明确了：被@Transient注解修饰的属性还是会正常参与到序列化过程中，但是被transient关键字修饰的就不会了。

#### 嵌套类型 (Embedded Type)

嵌套类型视总是需要依赖于另一个实体类型，不能单独存在。举个例子，地址这种实体概念就非常适合被定义为一个嵌套类型，比如家庭地址，收件地址等等，都拥有共通的几个字段，但是它们通常是属于另外一个实体类型，比如客户这一实体，它就能够拥有家庭地址以及收件地址。

如果从UML(统一建模语言)来审视这个概念的话，通常适用于使用嵌套类型来表达的实体类型一般都体现在它能够被”组合(Composition)”到另外一个实体中：



```
@Embeddable
public class Address {
    private String street;
    private String city;
    // ...
}

@Entity
public class Employee {
    @Id 
    private int id;
    private String name;
    private long salary;
    @Embedded 
    private Address address;
    // ...
}

```
注意@Embeddable 声明嵌套，@Embedded 使用嵌套 

@AttributeOverrides和@AttributeOverride 什么含义？
```
@Entity
public class Company {
    @Id 
    private int id;
    private String name;
    @Embedded
    @AttributeOverrides({
        @AttributeOverride(name="street", column=@Column(name="area")),
        @AttributeOverride(name="city", column=@Column(name="province"))
    })
    private Address address;
    // ...
}

```

####关系类型
[JPA关系类型](./JPA-Note-关系类型.md)

### 参考资料

[JPA 1. 事务的基础概念](http://blog.csdn.net/dm_vincent/article/details/52566964)
[2. EJB中的事务管理](http://blog.csdn.net/dm_vincent/article/details/52579719)
[Spring-Transaction Management](http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/#transaction)
[JPA EntityManager高级](http://blog.csdn.net/han_yankun2009/article/details/45401935?utm_source=tuicool&utm_medium=referral)
[SpringBoot第二讲利用Spring Data JPA实现数据库的访问(一)](http://www.jianshu.com/p/5c10a4c6ceb0)
[SpringBoot第四讲扩展和封装Spring Data JPA(一)_自定义Repository和创建自己的BaseRepository](http://www.jianshu.com/p/73f48095a7bf)

[springboot教学资源源码](https://github.com/ynkonghao/resource/tree/master/src/springboot)

[JPA 菜鸟教程 3 单向多对一](http://blog.csdn.net/je_ge/article/details/53493897)


