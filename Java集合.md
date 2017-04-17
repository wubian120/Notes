# Java 集合
Java的集合类都在 Java.utils包下。
分为五个方面： List 列表接口， Map映射接口， Set 集合接口， 迭代器， 工具类。 
List接口：数组，队列，链表，栈。
Map接口：key-value键值对，HashMap,WeakHashMap,AbstractMap通过适配器模式实现Map接口中的大部分函数。
## 1.集合的接口与实现分离
队列 queue 队尾部添加元素，队列头部删除元素。 先进先出。

两种实现方式， 循环数组，链表。

CircularArrayQueue<E> implements Queue<E>

LinkedListQueue<E> implements Queue<E>


Java集合类库中   ArrayDeque 循环数组， LinkedList链表队列。
```
//Collection接口
public interface Collection<E>
{
    boolean add(E element);
    Iterator<E> iterator();
}
//Iterator接口
public interface Iterator<E>
{
    E next();
    boolean hasNext();
    void remove();
}
```

Java中的迭代器与C++中迭代器不同，查找一个元素的唯一方法是调用next()，迭代器可以认为是两个元素之间。调用next()，迭代器越过到下一个元素，并返回刚刚越过的那个元素的引用。

Iterator接口，删除，是删除上次调用next()时返回的元素。

ddd

<meta http-equiv="refresh" content="2">




