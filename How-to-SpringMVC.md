#How-To-SpringMVC note

## 搭建web环境

- pom.xml
- web.xml
- Springconfig.xml
- MainControler.java
- Product.java
- ProductRepo.java

###pom.xml
```
<properties>
    <spring.version>4.3.0.RELEASE</spring.version>
    <hibernate.version>5.1.0.Final</hibernate.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>org.springframework.data</groupId>
      <artifactId>spring-data-jpa</artifactId>
      <version>1.10.1.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.hibernate</groupId>
      <artifactId>hibernate-entitymanager</artifactId>
      <version>${hibernate.version}</version>
    </dependency>

    <dependency>
      <groupId>org.hibernate</groupId>
      <artifactId>hibernate-c3p0</artifactId>
      <version>${hibernate.version}</version>
    </dependency>

    <dependency>
      <groupId>com.mchange</groupId>
      <artifactId>c3p0</artifactId>
      <version>0.9.5.2</version>
    </dependency>

    <dependency>
      <groupId>commons-io</groupId>
      <artifactId>commons-io</artifactId>
      <version>2.2</version>
    </dependency>

    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
    </dependency>

    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>

    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>jstl</artifactId>
      <version>1.2</version>
    </dependency>

    <dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>jsp-api</artifactId>
      <version>2.2</version>
      <scope>provided</scope>
    </dependency>

    <dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.3.1</version>
    </dependency>

    <dependency>
      <groupId>org.postgresql</groupId>
      <artifactId>postgresql</artifactId>
      <version>9.4.1209</version>
    </dependency>

    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.8.0</version>
    </dependency>

    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-core</artifactId>
      <version>2.8.0</version>
    </dependency>

    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-annotations</artifactId>
      <version>2.8.0</version>
    </dependency>
  </dependencies>
```

### springconfig.xml
```
  <context:component-scan base-package="cn.brady.controller"/>
    <mvc:default-servlet-handler/>
    <mvc:annotation-driven/>

    <bean id="jspViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
        <property name="prefix" value="/pages/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
    <jpa:repositories base-package="cn.brady.repository"/>
    <!--链接到persistence.xml-->
    <bean id="entityManagerFactory" class="org.springframework.orm.jpa.LocalEntityManagerFactoryBean">
        <property name="persistenceUnitName" value="bradyPersistUnit"/>
    </bean>
    <!--事务管理-->
    <bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager">
        <property name="entityManagerFactory" ref="entityManagerFactory"/>
    </bean>
    <tx:annotation-driven transaction-manager="transactionManager"/>
```



```




```



```

```

### 单向多对一
多个产品Product 对应 一种产品类型ProductType
- 产品表
```
CREATE TABLE public.product
(
  id bigint NOT NULL,
  name character varying(200) NOT NULL,
  type_id integer, -- product_type id  fk
  CONSTRAINT pk_product PRIMARY KEY (id),
  CONSTRAINT fkajjopj7ffr42w11bav8gut0cp FOREIGN KEY (type_id)
      REFERENCES public.product_type (id) MATCH SIMPLE
      ON UPDATE NO ACTION ON DELETE NO ACTION,
  CONSTRAINT product_type_fk FOREIGN KEY (type_id)
      REFERENCES public.product_type (id) MATCH SIMPLE
      ON UPDATE NO ACTION ON DELETE NO ACTION
)
WITH (
  OIDS=FALSE
);
ALTER TABLE public.product
  OWNER TO postgres;
COMMENT ON TABLE public.product
  IS '产品表';
COMMENT ON COLUMN public.product.type_id IS 'product_type id  fk';

```

- 产品类型 表
```
CREATE TABLE public.product_type
(
  id integer NOT NULL,
  name character varying,
  CONSTRAINT product_type_pk PRIMARY KEY (id)
)
WITH (
  OIDS=FALSE
);
ALTER TABLE public.product_type
  OWNER TO postgres;
COMMENT ON TABLE public.product_type
  IS '产品类型， 与产品关联';

```

- Product
```
@Entity
@Table(name="product",schema = "public", catalog = "demo")
public class ProductEntity {

    private int id;
    private String name;
    private ProductType type;

    @Id
    @Column(name = "id", nullable = false)
    public int getId() {return id;}
    public void setId(int id) {this.id = id;}

    @Basic
    @Column(name = "name", nullable = true)
    public String getName() {return name;}
    public void setName(String name) {this.name = name;}

    @ManyToOne(optional = true)
    @JoinColumn(name = "type_id")
    public ProductType getType(){return type;}
    public void setType(ProductType type) {this.type = type;}

    @Override
    public String toString() {
        return "Id: "+ id+" name: "+name+" type: "+ type;
    }
}

```

- ProductType
单向 多对一   就是一这边 没有多的那一方的集合
```
@Entity
@Table(name = "product_type", schema = "public", catalog = "demo")
public class ProductType {

    private int id;
    private String name;

    @Id
    @Column(name = "id", nullable = false)
    public int getId() {return id;}
    public void setId(int id) {this.id = id;}

    @Basic
    @Column(name = "name", nullable = true)
    public String getName() {return name;}
    public void setName(String name) {this.name = name;}

    @Override
    public String toString() {
        return "Id: "+id + " name: "+ name;
    }
}
```

- 单向多对一单元测试
```
public class Many2OneTest {
  private static EntityManagerFactory entityManagerFactory = null;
  private EntityManager entityManager = null;

  @BeforeClass
  public static void setUpBeforeClass() throws Exception {
    entityManagerFactory = Persistence.createEntityManagerFactory("bradyPersistUnit");
  }

  // 要求：一次性保存1个产品类型，保存2个产品
  // 单向多对一保存的时候必须先保存一方，否则会出现多余的update语句，从而影响性能
  @Before
  public void persist() throws Exception {
    entityManager = entityManagerFactory.createEntityManager();
    entityManager.getTransaction().begin();

    ProductType type = new ProductType();
    type.setId(1);
    type.setName("产品类型1");

    // 传入type的本质是处理数据库product表的type_id外键
    ProductEntity product1 = new ProductEntity();
    product1.setId(103);
    product1.setName("pen103");
    product1.setType(type);
    ProductEntity product2 = new ProductEntity();
    product2.setId(105);
    product2.setName("book105");

    System.out.println("保存之前：" + type);
    entityManager.persist(type);// JPA会自动把保存后的主键放到当前对象的id里面
    System.out.println("保存之后：" + type);
    entityManager.persist(product1);
    entityManager.persist(product2);
    System.out.println("++++++++++++++++++++");
  }

  // 可以通过多方Product获取一方ProductType是否为null，来判断是否有产品产品类型
  @Test
  public void find() throws Exception {
    ProductEntity product = entityManager.find(ProductEntity.class, 103);
    System.out.println(product);
    ProductType type = product.getType();
    if (type == null) {
      System.out.println("当前产品是没有产品类型的");
    } else {
      System.out.println("当前产品是有产品类型的");
    }
  }

  @After
  public void tearDown() throws Exception {
    entityManager.getTransaction().commit();
    if (entityManager != null && entityManager.isOpen())
      entityManager.close();
  }

  @AfterClass
  public static void tearDownAfterClass() throws Exception {
    if (entityManagerFactory != null && entityManagerFactory.isOpen())
      entityManagerFactory.close();
  }

}



```

###单向一对多 

就是反过来， ‘一’的一方 定义 个‘多’的集合， 
```
@Entity
@Table(name = "t_product_type")
public class ProductType {
  @Id
  @GeneratedValue
  private Long id;
  private String name;
  // 一对多:集合Set
  @OneToMany
  @JoinColumn(name = "type_id")
  private Set<Product> products = new HashSet<Product>();

  public Long getId() { return id;}
  public void setId(Long id) {    this.id = id;}

  public String getName() { return name;}
  public void setName(String name) {this.name = name;}

  public Set<Product> getProducts() { return products;}
  public void setProducts(Set<Product> products) {   this.products = products;}

  @Override
  public String toString() {
    return "ProductType [id=" + id + ", name=" + name + "]";
  }

}
```

### 双向一对多

- 一 ProductType, 多 Product
- 一的 维护多的 集合：
```
private Set<ProductEntity> products = new HashSet<ProductEntity>();
@OneToMany(mappedBy = "type", orphanRemoval = true)
public Set<ProductEntity> getProducts() {return products;}
public void setProducts(Set<ProductEntity> products) {this.products = products;}
```

- 多的 维护 一的 对象：
```
private ProductType type;
// optional=false表示外键type_id不能为空
@ManyToOne(optional = true)
@JoinColumn(name = "type_id")
public ProductType getType(){return type;}
public void setType(ProductType type) {this.type = type;}
```

mappedBy 单向关系不需要设置该属性，双向关系必须设置，避免双方都建立外键字段。
数据库中1对多的关系，关联关系总是被多方维护的即外键建在多方，我们在单方对象的@OneToMany（mappedby=” “）
[对Jpa中Entity关系映射中mappedBy的理解](http://blog.csdn.net/time1118/article/details/50629143)

　  a） 只有OneToOne,OneToMany,ManyToMany上才有mappedBy属性，ManyToOne不存在该属性；
　　b） mappedBy标签一定是定义在the owned side（被拥有方的），他指向the owning side（拥有方）；
　　c） 关系的拥有方负责关系的维护，在拥有方建立外键。所以用到@JoinColumn
　　d）mappedBy跟JoinColumn/JoinTable总是处于互斥的一方


@OneToMany：
一对多映射注解，双向一对多关系中，一端是关系维护端(owner side)，只能在一端添加mapped属性。多端是关系被维护端(inverse side)。建表时在关系被维护端(多端)建立外键指向关系维护端(一端)的主键列。
用法：@OneToMany(mappedBy = "维护端(一端)主键", cascade=CascadeType.ALL)
注意：在Hibernate中有个术语叫做维护关系反转，即由对方维护关联关系，使用inverse=false来表示关系维护放，在JPA的注解中，mappedBy就相当于inverse=false，即由mappedBy来维护关系。

[JPA总结——实体关系映射（一对一@OneToOne）](http://www.cnblogs.com/shiddong/p/5580485.html)
[JPA总结——实体关系映射（一对多@OneToMany）](http://www.cnblogs.com/shiddong/p/5580490.html)
映射策略
- 外键关联：两个表的关系定义在一个表中；
- 表关联：两个表的关系单独定义一个表中通过一个中间表来关联。

实体Company：公司。
实体Employee：雇员。

Company和Employee是一对多关系。
JPA使用@OneToMany和@ManyToOne来标识一对多的双向关联。一端(Company)使用@OneToMany,多端(Employee)使用@ManyToOne。

在JPA规范中，一对多的双向关系由多端(Employee)来维护。就是说多端(Employee)为关系维护端，负责关系的增删改查。一端(Company)则为关系被维护端，不能维护关系。

一端(Company)使用@OneToMany注释的mappedBy="company"属性表明Company是关系被维护端。

多端(Employee)使用@ManyToOne和@JoinColumn来注释属性company,@ManyToOne表明Employee是多端，@JoinColumn设置在employee表中的关联字段(外键)。


[JPA的一对多映射(双向)](http://www.cnblogs.com/luxh/archive/2012/05/27/2520501.html)


mappedBy= "a"  a是为表名 还是为‘多’的一方实体的属性名  。 如果为一的一方的表名 则报错，如下：
```
 mappedBy reference an unknown target entity property: cn.brady.model.ProductEntity.product_type in cn.brady.model.ProductType.products

```
如下的设置 才是正确的 。
```
ProductType.java

    @OneToMany(mappedBy = "type", orphanRemoval = true)
    public Set<ProductEntity> getProducts() {return products;}
    public void setProducts(Set<ProductEntity> products) {this.products = products;}

Product.java

  @ManyToOne(optional = true)
  @JoinColumn(name = "type_id")
  private ProductType type;

 ```


### 双向多对多

多对多中，包含一张中间关系表，两个实体表， 实体表中一方为关系维护方，可以操作中间表。另一方为被维护方，不能操作中间表。

- ddl 3张表  student  teacher   teacher_student 
```

CREATE TABLE public.student
(
  id bigint NOT NULL,
  sname character varying,
  CONSTRAINT student_pk PRIMARY KEY (id)
)
WITH (
  OIDS=FALSE
);
ALTER TABLE public.student
  OWNER TO postgres;
COMMENT ON TABLE public.student
  IS '学生，与teacher, 多对多 ';

CREATE TABLE public.teacher
(
  id bigint NOT NULL,
  name character varying, -- teacher name
  CONSTRAINT teacher_pk PRIMARY KEY (id)
)
WITH (
  OIDS=FALSE
);
ALTER TABLE public.teacher
  OWNER TO postgres;
COMMENT ON TABLE public.teacher
  IS '老师';
COMMENT ON COLUMN public.teacher.name IS 'teacher name';



CREATE TABLE public.teacher_student
(
  t_id bigint, -- teacher id
  s_id bigint, -- student id
  CONSTRAINT teacher_student_s_fk FOREIGN KEY (s_id)
      REFERENCES public.student (id) MATCH SIMPLE
      ON UPDATE NO ACTION ON DELETE NO ACTION,
  CONSTRAINT teacher_student_t_fk FOREIGN KEY (t_id)
      REFERENCES public.teacher (id) MATCH SIMPLE
      ON UPDATE NO ACTION ON DELETE NO ACTION
)
WITH (
  OIDS=FALSE
);
ALTER TABLE public.teacher_student
  OWNER TO postgres;
COMMENT ON TABLE public.teacher_student
  IS '多对多 关系表';
COMMENT ON COLUMN public.teacher_student.t_id IS 'teacher id';
COMMENT ON COLUMN public.teacher_student.s_id IS 'student id';

```




[JPA 菜鸟教程 7 双向多对多](http://www.jianshu.com/p/e0736493e433)
