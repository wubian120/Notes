# Java基础 Hashcode Equals


1. equals方法 非空对象引用上实现相等关系：

 - 自反性 对于任何非空引用值 x，x.equals(x) 都应返回 true。

 - 对称性 如果x.equals(y)返回是"true"，那么y.equals(x)也应该返回是"true"。
 
 - 传递性
 
 - 一致性 如果x.equals(y)返回是"true"，只要x和y内容一直不变，不管你重复x.equals(y)多少次，返回都是"true"。
 
 - 任何非空引用值x   x.equals(null); 都应返回false。 




2. equals()作用

equals() 定义在JDK的Object.java中。通过判断两个对象的地址是否相等(即，是否是同一个对象)来区分它们是否相等。

```
public boolean equals(Object obj) {
    return (this == obj);
}
```
 ** 类是否覆盖equals() 方法，分为两种情况： **
 
 - 若类没有覆盖equals()方法， 通过equals()比较两个对象时，实际上是比较是否是同一个对象，这时等价于通过"=="去比较。
 - 覆盖equals()方法， 通常做法是，若两个对象的内容相等，则equals()返回true, 否则返回false。
 
3. equals()与 == 区别

 == 作用是判断两个对象地址是否相等，即，是不是同一个对象。
  
4. hashCode() 作用

hashCode() 返回int 散列码，作用是确定该对象在哈希表中的索引位置。

hashCode() 定义在JDK的Object.java中，这就意味着Java中的任何类都包含有hashCode() 函数。

虽然，每个Java类都包含hashCode() 函数。
但是，仅仅当创建并某个“类的散列表”(关于“散列表”见下面说明)时，该类的hashCode() 才有用
(作用是：确定该类的每一个对象在散列表中的位置；其它情况下(例如，创建类的单个对象，或者创建类的对象数组等等)，类的hashCode() 没有作用。

 ** 散列表 **指的是：
 Java集合中本质是散列表的类，如HashMap，Hashtable，HashSet。

也就是说：hashCode() 在散列表中才有用，在其它情况下没用。
在散列表中hashCode() 的作用是获取对象的散列码，进而确定该对象在散列表中的位置。

5. hashCode() 和 equals() 关系

 - 不创建"类对应的散列表"
 
这里所说的“会创建类对应的散列表”是说：我们会在HashSet, Hashtable, HashMap等等这些本质是散列表的数据结构中，用到该类。
例如，会创建该类的HashSet集合。

这种情况下，equals() 用来比较该类的两个对象是否相等。而hashCode() 则根本没有任何作用，所以，不用理会hashCode()。

 - 创建类对应的散列表
 
1、如果两个对象相等，那么它们的hashCode()值一定相同。
这里的相等是指，通过equals()比较两个对象时返回true。

2.、如果两个对象hashCode()相等，它们并不一定相等。
因为在散列表中，hashCode()相等，即两个键值对的哈希值相等。
然而哈希值相等，并不一定能得出键值对相等。

补充说一句：“两个不同的键值对，哈希值相等”，这就是哈希冲突。

此外，在这种情况下。若要判断两个对象是否相等，除了要覆盖equals()之外，也要覆盖hashCode()函数。否则，equals()无效。
 
例如，创建Person类的HashSet集合，必须同时覆盖Person类的equals() 和 hashCode()方法。


** String 计算 hashcode 方法中的31  原因 参见 effective java chapter3 9item **

String 的hashCode源码
```
/**
     * Returns a hash code for this string. The hash code for a
     * {@code String} object is computed as
     * <blockquote><pre>
     * s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1]
     * </pre></blockquote>
     * using {@code int} arithmetic, where {@code s[i]} is the
     * <i>i</i>th character of the string, {@code n} is the length of
     * the string, and {@code ^} indicates exponentiation.
     * (The hash value of the empty string is zero.)
     *
     * @return  a hash code value for this object.
     */
    public int hashCode() {
        int h = hash;
        if (h == 0 && value.length > 0) {
            char val[] = value;

            for (int i = 0; i < value.length; i++) {
                h = 31 * h + val[i];
            }
            hash = h;
        }
        return h;
    }


 private int hash; // Default to 0
```

s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1] 
** 这个可以通过公式计算出来  h = 31 * h + val[i];  **


s[i]是string的第i+1个字符，s[0]表示第1个字符，n是String的长度。s[0]*31^(n-1)表示31的n-1次方再乘以第一个字符的值。



31   = 2^5    2的5次幂  二进制表示 11111

因为31是一个奇质数。一方面可以产生更分散的散列，即不同字符串hash值也一般不同，所以在选择系数的时候要选择尽量长的并且让乘法结果尽量不要溢出的系数，如果计算出来的hash地址越大，所谓的“冲突”就越少，查找起来效率也会提高。但是也不能过大，在java乘法中如果数字相乘过大会导致溢出的问题，从而导致数据的丢失。另一个方面主要是计算效率比较高，31 i=32 i - i=(i << 5) - i，这种位移与减法结合的计算相比一般的乘法运算快很多。

字符串哈希值可以做很多事情，通常是类似于字符串判等，判回文之类的。但是仅仅依赖于哈希值来判断其实是不严谨的，除非能够保证不会有哈希冲突，不过通常这一点很难做到。

就拿jdk中String类的哈希方法来举例，字符串"gdejicbegh"与字符串"hgebcijedg"具有相同的hashCode()返回值-801038016，并且它们具有reverse的关系。这个例子说明了用jdk中默认的hashCode方法判断字符串相等或者字符串回文，都存在反例。



### 参考资料
如何正确实现Java中的hashCode方法
http://mp.weixin.qq.com/s?__biz=MzA4MjA0MTc4NQ==&mid=504090072&idx=1&sn=019cb899777174518e1e49d0fd0bd2a5#rd

Java语言中String类的hashCode方法
http://www.jianshu.com/p/0938c5ecd6ba?utm_source=tuicool&utm_medium=referral

Java进阶---hashCode
http://blog.csdn.net/qq_35101189/article/details/53791772?utm_source=tuicool&utm_medium=referral


[Java提高篇（二六）——hashCode](http://cmsblogs.com/?p=631)

[Java 基础复习实践 --- Hashcode Equals](http://imxie.cc/2016/07/18/Review-the-Java-basic-equals-hashcode/)
[]()
[]()
[]()
[]()


