# Java Basic 泛型

泛型是通过擦除来实现的。因此泛型只在编译时强化它的类型信息，而在运行时丢弃(或者擦除)它的元素类型信息。擦除使得使用泛型的代码可以和没有使用泛型的代码随意互用。

原始类型和带参数类型 之间的主要区别是：

在编译时编译器不会对原始类型进行类型安全检查，却会对带参数的类型进行检查
通过使用 Object 作为类型，可以告知编译器该方法可以接受任何类型的对象，比如String 或 Integer
你可以把任何带参数的类型传递给原始类型 List，但却不能把 List< String> 传递给接受 List< Object> 的方法，因为泛型的不可变性，会产生编译错误。

## 泛型类
```java
public class Point<T> {
    private T x;
    private T y;

    public T getX() {return x;}
    public void setX(T x) {this.x = x;}
    public T getY() {return y;}
    public void setY(T y) {this.y = y;}
}

```

## 泛型接口
```java
public interface Message<T> {
    T getMessage();
    void setMessage(T t);
}

public class MessageImpl implements Message<String>  {
    private String message;
    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }
    @Override
    public String toString() {
        return "Message: " + message;
    }

    public static void main(String[] args){

        MessageImpl messageImpl = new MessageImpl();
        messageImpl.setMessage("hahahahahahahaha");

        System.out.println(messageImpl);
    }

}

public interface Generator<T> {
    T next();
}
/** 泛型接口
 * Created by Brady on 2017/5/5.
 */
public class FruitGenerator implements Generator<String> {

    private String[] fruits = new String[]{"Apple","Pear", "Banana"};
    
    public String next() {
        Random random = new Random();
        return fruits[random.nextInt(3)];
    }

    public static void main(String[] args){

        FruitGenerator fruitGenerator = new FruitGenerator();
        System.out.println(fruitGenerator.next());
        System.out.println(fruitGenerator.next());
        System.out.println(fruitGenerator.next());
    }

}


```



## 泛型函数

```java
public class GenericFunction {

    public static <T> void staticMethod(T a){

        System.out.println(a);
    }
    // 普通方法
    public <T> void regularMethod(T a){
        System.out.println(a);
    }

    public static void main(String[] args){

        String s = "123141";

        staticMethod(s);

        GenericFunction gf = new GenericFunction();
        gf.regularMethod(s);
        gf.<String>regularMethod(s); // 都可以 这样更明确
    }

}

```


## 泛型数组

由于擦除的存在，没有泛型数组

## 泛型限定符  



```java
public static <T extends Comparable> T min(T[] a)

```
T extends BoundingType1 & BoundingType2;  T 应该是BoundingType的子类型，用 & 分隔。T可以是类也可以是接口， 如果多个绑定类型，类只能有一个，且放在第一个位置。

## 擦除

Java 中的泛型和 C++ 中的模板有一个很大的不同：

- C++ 中模板的实例化会为每一种类型都产生一套不同的代码，这就是所谓的代码膨胀。
- Java 中并不会产生这个问题。虚拟机中并没有泛型类型对象，所有的对象都是普通类。

（摘自：http://blog.csdn.net/fw0124/article/details/42295463）

实际上泛型程序也是首先被转化成一般的、不带泛型的 Java 程序后再进行处理的，编译器自动完成了从 Generic Java 到普通 Java 的翻译，Java 虚拟机运行时对泛型基本一无所知。

当编译器对带有泛型的java代码进行编译时，它会去执行**类型检查**和**类型推断**，然后生成普通的不带泛型的字节码，这种普通的字节码可以被一般的 Java 虚拟机接收并执行，这在就叫做 **类型擦除（type erasure）**。

### 擦除的实现原理  

In short Generics in Java is syntactic sugar and doesn’t store any type related information at runtime. All type related information is erased by Type Erasure, this was the main requirement while developing Generics feature in order to reuse all Java code written without Generics.

Java 编辑器会将泛型代码中的类型完全擦除，使其变成原始类型。

当然，这时的代码类型和我们想要的还有距离，接着 Java 编译器会在这些代码中加入类型转换，将原始类型转换成想要的类型。这些操作都是编译器后台进行，可以保证类型安全。

总之泛型就是一个语法糖，它运行时没有存储任何类型信息。

### 擦除导致的泛型不可变性

泛型中没有逻辑上的父子关系，如 List 并不是 List 的父类。两者擦除之后都是List，所以形如下面的代码，编译器会报错：
```java
/**
 * 两者并不是方法的重载。擦除之后都是同一方法，所以编译不会通过。
 * 擦除之后：
 * 
 * void m(List numbers){}
 * void m(List strings){} //编译不通过，已经存在相同方法签名
 */
void method(List<Object> numbers) {}

void method(List<String> strings) {}
```
泛型的这种情况称为 不可变性，与之对应的概念是 协变、逆变：

- 协变：如果 A 是 B 的父类，并且 A 的容器（比如 List< A>） 也是 B 的容器（List< B>）的父类，则称之为协变的（父子关系保持一致）  
- 逆变：如果 A 是 B 的父类，但是 A 的容器 是 B 的容器的子类，则称之为逆变（放入容器就篡位了）
- 不可变：不论 A B 有什么关系，A 的容器和 B 的容器都没有父子关系，称之为不可变
Java 中数组是协变的，泛型是不可变的。

如果想要让某个泛型类具有协变性，就需要用到边界。

如果没有指明边界，类型参数将被擦除为 Object。

如果我们想要让参数保留一个边界，可以给参数设置一个边界，泛型参数将会被擦除到它的第一个边界（边界可以有多个），这样即使运行时擦除后也会有范围。

比如：
```java
public class GenericErasure {
    interface Game {
        void play();
    }
    interface Program{
        void code();
    }

    public static class People<T extends Program & Game>{
        private T mPeople;

        public People(T people){
            mPeople = people;
        }

        public void habit(){
            mPeople.code();
            mPeople.play();
        }
    }
}
```
上述代码中, People 的类型参数 T 有两个边界,编译器事实上会把类型参数替换为它的第一个边界的类型。

## 通配符

 <?\>   无限制通配符
 <? extends E \>  extends 关键字声明了类型的上界， 表示类型参数可以能是E类型或E的子类  

```java
private <K extends ChildBean, E extends BookBean> E test(K arg1, E arg2){

}
```




 <? super E>      super   关键字声明了类型的下界， 表示类型参数可以能是E类型或E的父类


 - < ? super E> 用于灵活写入或比较，使得对象可以写入父类型的容器，使得父类型的比较方法可以应用于子类对象。
 - < ? extends E> 用于灵活读取，使得方法可以读取 E 或 E 的任意子类型的容器对象。

用《Effective Java》 中的一个短语来加深理解：

为了获得最大限度的灵活性，要在表示 生产者或者消费者 的输入参数上使用通配符，使用的规则就是：生产者有上限、消费者有下限  
PECS: producer-extends, costumer-super


因此使用通配符的基本原则：

- 如果参数化类型表示一个 T 的生产者，使用 < ? extends T>;  
- 如果它表示一个 T 的消费者，就使用 < ? super T>；  
- 如果既是生产又是消费，那使用通配符就没什么意义了，因为你需要的是精确的参数类型。  

总结：
- T 的生产者的意思就是结果会返回 T，这就要求返回一个具体的类型，必须有上限才够具体；
- T 的消费者的意思是要操作 T，这就要求操作的容器要够大，所以容器需要是 T 的父类，即 super T；

## 泛型规则

- 泛型的参数类型只能是类（包括自定义类），不能是简单类型。
- 同一种泛型可以对应多个版本（因为参数类型是不确定的），不同版本的泛型类实例是不兼容的。
- 泛型的类型参数可以有多个
- 泛型的参数类型可以使用 extends 语句，习惯上称为“有界类型”
- 泛型的参数类型还可以是通配符类型，例如 Class


### 桥方法  bridge method
编译器的中间办法

### 泛型字母的习惯

 E — Element，常用在java Collection里，如：List<E>,Iterator<E>,Set<E>
 K,V — Key，Value，代表Map的键值对
 N — Number，数字
 T — Type，类型，如String，Integer等等




## 代码项目

JavaBasic
  +cn.brady.generic 



## 资料
[ 深入理解 Java 泛型](http://blog.csdn.net/u011240877/article/details/53545041)
[夯实JAVA基本之一 —— 泛型详解(1):基本使用](http://blog.csdn.net/harvic880925/article/details/49872903)
[JVM如何理解Java泛型类(转)](http://www.cnblogs.com/ggjucheng/p/3352519.html)
[Java系列：关于Java中的桥接方法](http://www.cnblogs.com/strinkbug/p/5019453.html?utm_source=tuicool&utm_medium=referral)
[]()
