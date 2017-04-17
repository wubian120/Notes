## JPA Note 关系类型

###角色 Role
两个参与方所组成的关系。比如我们常见的雇佣关系，就是企业和员工之间的一种关系，那么企业和员工在这层关系中就分别扮演着雇佣者和被雇佣者的角色。

### 方向 Directionality
主动建立的一方我们可以将其称为源角色(Source Role，简称Source)，而被动响应的一方则可以被称为目标角色(Target Role，简称Target)。


反映到程序中，关系的方向指的就是主动引用，比如我们在Employee类型中引用Department类型，那么就是由Employee指向Department的一层关系，Employee扮演的是Source，Department扮演的是Target。如果在Department类型中也引用了Employee类型，那么就是由Department指向Employee的一层关系，Department扮演的是Source，Employee扮演的是Target。

因此如果互相引用，那关系就是一个双向关系了。就好比我知道我爸爸是谁，我爸爸也知道我是谁。

而单向关系在这个世界中其实更多一些，比如我知道马云是谁，而马云肯定不知道我是谁。又或者一个男生暗恋一个女生，这些都是单向关系。

### 基数 Cardinality


关系的最后一个要素便是基数。所谓的基数描述的是参与到关系的两个角色，在数量上的特征。
比如法律上的一夫一妻制和一夫多妻制。

###关系映射

- 单值映射(Single-Valued Mapping)
- 集合映射(Collection-Viewed Mapping)

####一对一 One to One

Employee  Workspace 
```
@Entity
public class Employee {
    @Id 
    private int id;

    private String name;

    @OneToOne
    @JoinColumn(name="WSPACE_ID")
    private Workspace workspace;
    // ...
}

@Entity
public class Workspace {
    @Id 
    private int id;

    private String location;

    @OneToOne(mappedBy = "workspace")
    private Employee employee;
    // ...
}
```

@JoinColumn的注解，可以将这个注解理解成外键。而这个外键的列名则是通过@JoinColumn注解中的name属性进行声明。同时，还需要注意的是当@OneToOne和@JoinColumn一起使用的时候，这个外键所在的列实际上还需要满足唯一性的约束。因为每个Workspace的实例实际上是被唯一的一个Employee实例所独占的，所以在该外键列中不可能存在相等的值。

注意在上述的Workspace类中，也使用了@OneToOne注解来声明一个从Workspace指向Employee的关系。但是这里并没有使用@JoinColumn注解来声明外键的相关信息。也就是说，上述实体类对应的数据库表中并不会含有引用Employee类型的外键列。这是JPA中规定的对于双向一对一关系的映射方式。

尤其注意@OneToOne注解中的mappedBy属性，这个属性的值实际上是关系中源角色(Employee)一方引用目标角色(Workspace)一方的引用名称，假如我们把Employee类中的workspace改成ws，那么相应的Workspace类中@OneToOne的mappedBy属性也需要被改成ws。这种定义方式，保证了所定义的双向关系是由参与的两个@OneToOne来完成的。

最后补充一点，由于@JoinColumn是用来定义外键关系的，那么拥有该注解的一方可以被称为关系中的所有方(Owning Side)，而被引用的一方则被称为反转方(Inverse Side)。所以如果是定义双向关系的话，在反转方一般就需要使用mappedBy属性了。


#### 多对一 Many to One
```
@Entity
public class Employee {
    @Id 
    private int id;

    @ManyToOne
    @JoinColumn(name="DEPT_ID")
    private Department department;
    // ...
}
```
使用@ManyToOne注解来定义一个多对一关系。对于多对一关系而言，它一般需要一个**@JoinColumn来帮助定义其对应的外键信息**。因此根据上面的定义，多对一关系中的”多”方，总是可以被视为此段关系中的所有方(Owning Side)，而响应的”一”方，在这种情况下可以被视为反转方(Inverse Side)。因此，在上面的例子中，Employee的表结构中就会有一个名为DEPT_ID的列作为引用Department记录的外键列。


#### 一对多

一对多这种关系一般是不会单独出现的，比如我们只想定义从Department指向Employee的一层关系。假设一个Department实例中引用了五个Employee实例，在程序中使用集合类型来描述这层关系。但是考虑一下换到数据库表结构中，一行是无法引用多个(并且数量还不定)其它表结构中的记录的。

一对多一般伴随着多对一作为双向关系出现，由于@JoinColumn总是会出现在”多”方，
因此我们需要在@OneToMany注解中使用mappedBy属性来声明这一点，此时的”一”方是反转方(Inverse Side)，在反转方中就需要使用mappedBy属性，这是JPA规范中的一部分。

```

@Entity
public class Department {
    @Id 
    private int id;

    private String name;
    @OneToMany(mappedBy="department")
    private Collection<Employee> employees;
    // ...
}

```


如果你只想定义一个单向的一对多关系，那么可以不使用mappedBy属性，此时JPA会推断你想使用联接表(Join Table)来完成关系的定义，虽然这个联接表有其默认的命名规范，但是显式地声明它通常是更明智的方案，比如下面：
```

@Entity
public class Department {
    @Id 
    private int id;
    private String name;

    @OneToMany
    @JoinTable(name="DEPT_EMP",
        joinColumns=@JoinColumn(name="DEPT_ID"),
        inverseJoinColumns=@JoinColumn(name="EMP_ID"))
    private Collection<Employee> employees;
}

```
此时，Department和Employee的一对多关系会通过表DEPT_EMP进行管理。@JoinTable注解的joinColumns属性声明了联接表中引用此关系中的所有方的外键列名为DEPT_ID，inverseJoinColumns属性声明了联接表中引用此关系中的反转方的外键名为EMP_ID。


#### 多对多 Many to Many 

```
@Entity
public class Employee {
    @Id 
    private int id;
    private String name;

    @ManyToMany
    @JoinTable(name="EMP_PROJ",
        joinColumns=@JoinColumn(name="EMP_ID"),
        inverseJoinColumns=@JoinColumn(name="PROJ_ID"))
    private Collection<Project> projects;
}

@Entity
public class Project {
    @Id 
    private int id;
    private String name;

    @ManyToMany(mappedBy="projects")
    private Collection<Employee> employees;

}

```
主要还是取决于在Project类中声明的@ManyToMany(mappedBy="projects")，如果缺少了这个mappedBy属性的相关定义，那么JPA实现在分析这段代码中出现的两个@ManyToMany的时候，会认为这是两个单向而独立的多对多关系。因此会尝试读取两个联接表：一个是我们显式声明的表EMP_PROJ，另一个则是根据实体类的名字生成的默认联接表Project_Employee。显然生成两个独立的联接表在通常情况下并非我们的目的，所以在使用多对多关系的时候，一定要注意mappedBy属性的声明。


[JPA ORM的核心注解 - 关系类型](http://blog.csdn.net/dm_vincent/article/details/52877296)
[JPA 菜鸟教程 6 单向多对多](http://blog.csdn.net/je_ge/article/details/53495237)
[ JPA 菜鸟教程 7 双向多对多](http://blog.csdn.net/je_ge/article/details/53495190)