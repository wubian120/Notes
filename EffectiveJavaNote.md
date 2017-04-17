# Effective Java Note




1. 用静态工厂方法代替构造器



2. 多个构造器参数时考虑用构建器




3. 用私有构造器或者枚举类型强化Singleton属性



4. 通过私有构造器强化不可实例化的能力

5. 避免创建不必要的对象

6. 消除过期的对象引用

消除过期引用最好的方法是让包含该引用的变量结束其生命周期。 

只要类是自己管理内存，程序员就应该警惕内存泄露问题。

** 下面程序会出现内存泄露 **
```
class StackTest {
    private Object[] elements;
    private int      size  = 0;
    private static final int DEFAULT_INITIAL_CAPACITY = 16;
    
    public StackTest(){
        elements = new Object[DEFAULT_INITIAL_CAPACITY];
    }
    
    private void ensureCapacity(){
        if(elements.length == size)
            elements = Arrays.copyOf(elements, 2*size +1);
    }
    
    public void push(Object e){
        ensureCapacity();
        elements[size++] = e;
    }
    
    public Object pop(){
        if(size == 0){
            throw new EmptyStackException();
        }
        return elements[--size]; //// 
    }
}

```
stack 内部维护着过期的引用。obsolete reference。
内存泄露会导致 磁盘交换 Disk Paging

//修改完的 
```
    public Object pop(){
        if(size == 0){
            throw new EmptyStackException();
        }
        Object result = elements[--size];
        elements[size] = null;
        return result;
    }
```

WeakHashMap

1. 一般而言，类自己管理内存， 程序员就应该警惕内存泄漏
2. 缓存容易内存泄漏
3. 监听器和回调 只保存弱引用

Heap Profiler









7. 避免使用终结方法



## 8. 覆盖equals时 请遵守通用约定

  - 不覆盖equals方法， 就是表示每个实例都只与它自身相等。
  
  - 类是私有的或者包级别 私有， 可以确定它的equals方法永远不会被调用。 这种情况下，应该覆盖equals方法。
   ** 这句话 未通 ** 

  - 应该覆盖Object.equals的情况通常是 “值类” value class。两个实例是否相等的含义是他们的属性值相等，就是相等。
  
  - 覆盖equals方法，必须要遵守的通用约定是， Object规范：
    - equals方法的等价关系 equivalence relation:
        - 自反性 reflexive
        - 对称性 symmetric
        - 传递性 transitive
        - 一致性 consistent
        - 非null的引用x， x.equals(null) 必须返回false。
    


9. 覆盖equals 总要覆盖hashCode()

Object规范
- 多次调用同一个对象，hashCode返回同一个整数
- 两个对象equals()方法比较是相等的，那么调用hashCode产生同样的整数结果。
- 为不同的对象产生不同的散列码。



相等的对象必须有相等的hash code。

计算 hash code 用 31。 是因为 `i* 31 =  i<<5 - i` 可以将该乘法 转化为位移运算和减法来代替。从而利用JVM的优化来提高性能。



把某个非0的常数值，比如17，保存在一个名为result的int类型的变量中。
对于对象中的每个域，做如下操作：
 - 为该域计算int类型的哈希值c：
 - 如果该域是boolean类型，则计算(f?1:0)
 - 如果该域是byte、char、short或者int类型，则计算(int)f
 - 如果该域是long类型，则计算(int)(f^(f>>>32))
 - 如果该域是float类型，则计算Float.floatToIntBits(f)
 - 如果该域是double类型，则计算Double.doubleToLongBits(f)，然后重复第三个步骤。
 - 如果该域是一个对象引用，并且该类的equals方法通过递归调用equals方法来比较这个域，同样为这个域递归的调用hashCode，如果这个域为null，则返回0。
 - 如果该域是数组，则要把每一个元素当作单独的域来处理，递归的运用上述规则，如果数组域中的每个元素都很重要，那么可以使用Arrays.hashCode方法。

把上面计算得到的hash值c合并到result中
```result = 31*result + c  ```

String中的Hashcode方法
String的hashcode的算法就充分利用了字符串内部字符数组的所有字符。生成hash码的算法的在string类中看起来像如下所示，注意“s“是那个字符数组，n是字符串的长度。

```s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1]  ```


Hashcode使用Eclipse IDE
现代IDE通过点击右键上下文菜单可以自动生成hashcode方法，通过Eclipse IDE 生成的hashcode像：
```
public int hashCode() {  
    final int prime = 31;  
    int result = 1;  
    result = prime * result + a;  
    return result;  
}  
```
但是并不推荐如上代码使用在企业级代码中，最好使用第三方库如Apache commons来生成hashocde方法。

**Apache commons HashcodeBuilder**
我们可以用Apache Commons hashcode builder来生成代码，使用这样一个第三方库的优势是可以反复验证尝试代码。下面代码显示了如何使用Apache Commons hash code 为一个自定义类构建生成hash code 。
```
public int hashCode(){  
       HashCodeBuilder builder = new HashCodeBuilder();  
       builder.append(mostSignificantMemberVariable);  
   ........................  
   builder.append(leastSignificantMemberVariable);  
       return builder.toHashCode();  
   }  
```
Java如何计算hashcode值
https://yq.aliyun.com/articles/24324

http://www.cnblogs.com/rayguo/p/3784891.html





















