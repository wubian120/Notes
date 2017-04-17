#Java集合 HashMap


1. 基本使用

- 新建与插入元素

- 遍历
 几种遍历方法 与效率优劣

底层实现是数组

每次新建 HashMap， 会初始化table数组，table数组元素为Entry节点  Entry节点 是链表 
Entry 是内部类， 包含了key value, 下一个节点next， 以及hash值

**HashMap是Y轴方向是数组，X轴方向就是链表的存储方式**

- 初始化参数 初始容量，加载因子

容量表示 哈希表中槽的数量 （哈希数组的长度）。默认16。加载因子是 当前key的数量和容量的比值。 

2. 常用方法

```
public V put(K key, V value)
```
get

迭代方法




3. HashMap实现


Class HashMap<K,V>

java.lang.Object
  java.util.AbstractMap<K,V>
    java.util.HashMap<K,V>

类型参数:
  - K - the type of keys maintained by this map
  - V - the type of mapped values

执行接口：
 - Serializable, Cloneable, Map<K,V>

直接之类：
 - LinkedHashMap, PrinterStateReasons

哈希表执行 Map 接口。 允许null值和 null key。
HashMap Hashtable大致相当，除了 unsynchronized 和允许null。


Java 8之后 是 数组 + 链表 + 红黑树

##参考资料：
Java集合专题总结（1）：HashMap 和 HashTable 源码学习和面试总结
http://www.cnblogs.com/kubixuesheng/p/6144535.html?utm_source=tuicool&utm_medium=referral

 Java 集合系列之 HashMap详细介绍(源码解析)和使用示例
http://blog.csdn.net/qq_35101189/article/details/53893589?utm_source=tuicool&utm_medium=referral


 Java提高篇---HashMap
http://blog.csdn.net/qq_35101189/article/details/53707711?utm_source=tuicool&utm_medium=referral

 JAVA二进制.位运算.移位运算
http://blog.csdn.net/posonrick/article/details/52052238

Java transient关键字使用小记
http://www.importnew.com/21517.html?utm_source=tuicool&utm_medium=referral

Java中的关键字 transient
http://www.linuxidc.com/Linux/2016-12/138429.htm?utm_source=tuicool&utm_medium=referral


https://mingdroid.github.io/2016/10/24/%E8%BF%9E%E7%8E%AF%E6%9D%80%E6%89%8B/

http://blog.csdn.net/mynameishuangshuai/article/details/52748853

java Collection框架分析之HashMap实现分析
https://github.com/getletCodes/StudyNotes/blob/master/part1/java%20Collection%E6%A1%86%E6%9E%B6%E5%88%86%E6%9E%90%E4%B9%8BHashMap%E5%AE%9E%E7%8E%B0%E5%88%86%E6%9E%90.md

Java集合专题总结（1）：HashMap 和 HashTable 源码学习和面试总结
http://www.cnblogs.com/kubixuesheng/p/6144535.html?utm_source=tuicool&utm_medium=referral






























