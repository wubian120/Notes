# Java集合 

1. 接口与实现分离
1.1 基本接口 Collection<E>
```
  public interface Collection<E>
  {
    boolean add(E element);
    Iterator<E> iterator();
  }
```

1.2 迭代器 Iterator接口
```
 public iterface Iterator<E>
 {
    E next();// 达到末尾会抛出 NoSuchElementException
    boolean hasNext();
    void remove(); //删除上次访问的对象。 如果上次访问之后 集合发生了变化， 方法将抛出IllegalStateException
 }
```

遍历集合，在 hasNext()返回true时 反复调用next方法。
```
public iterface Iterable<E>  
{
    Iterator<E> iterator();
}
```
Collection 接口扩展来 Iterable接口，

遍历集合，在 hasNext()返回true时 反复调用next方法。
public iterface Iterable<E>  
｛
    Iterator<E> iterator();
  ｝
Collection 接口扩展来 Iterable接口，
问题： Iterable接口 与 Iterator接口 区别。

1.3 删除元素

Iterator接口  remove() 删除 上次调用next()方法时返回的元素。 因此，remove() next()方法具有相互依赖性。
如果调用remove()之前没有调用next()会抛出异常 IllegalStateException异常。
删除相邻元素
it.remove();
it.next();
it.remove();

AbstractCollection 类

API:
java.util.Collection<E>

java.util.Iterator<E> 

2. 具体集合类

ArrayList                        数组列表
LinkedList                      链表列表
ArrayDeque                循环数组实现的双端队列
HashSet                         哈希 集合
TreeSet
EnumSet
LinkedHashSet
PriorityQueue
HashMap
TreeMap
EnumMap
LinkedHashMap
WeakHashMap
IdentityHashMap

数组和数组列表， 从数组中间删除或者插入对象，代价较大。 删除后，对象后面元素都要向前移动。

链表解决了 中间删除或插入代价大的问题。    java中链表 都是双向链接的 double linked。
2.1 链表 LinkedList
链表是个有序集合， ordered collection

反向遍历链表    
E previous(); //返回越过的对象

ListIterator<E> 返回一个实现了 ListIterator接口的迭代器对象。
```
interface ListIterator<E> extends Iterator<E>
{
    void add(E element);
    E prevois();           //
    bool hasPrevious();    // 
}
```
add()只依赖于迭代器的位置，即总是在元素的 右侧插入。    remove()依赖于迭代器的状态。 如果是next() 则删除左侧对象，  如果是previous() 则删除右侧 对象。

如果两个迭代器， 一个对集合遍历，另一个对集合 操作，则会引起混乱，会抛出异常 Cocurrent ModificationException。
如果迭代器发现它的集合被另一个 迭代器 修改了， 或者被该集合自身的方法修改了， 就会抛出CocurrentModificationException。

链表不支持快速 随机访问。 因此 如果需要采用索引访问数据时， 不选用链表。

Java迭代器 指向 两个元素之间的位置。  

previousIndex()   返回 下一次 调用 previous() 返回的元素的整数索引。
nextIndex()       返回 下一次 调用 next() 返回 的 元素的整数索引。

list.listIterator(n) 返回一个迭代器， 迭代器指向 索引为n的元素的前面的位置。

建议 避免使用以整数索引表示的链表中位置的 所有方法。 如果需要使用随机访问， 使用数组或者 ArrayList，不要使用链表。

java.util.List<E>


java.util.ListIterator<E>


java.util.LinkedList<E> 类

List 接口， 表示有序集合，提供两种访问方式， 迭代器和get set方法随机访问元素。但是随机访问元素 不适合链表。
ArrayList 封装了一个动态再分配的对象数组。


## Iterator  与 ListIterator 接口 区别



3. 散列表 HashSet

自定义的类，如果要放入集合，需要实现 hashCode(); 保证  a.equals(b) true   a 与 b 的hash code也相同。
Java中的散列表用链表数组实现。  每个列表称为 桶 bucket。
查找表中位置，需要先计算散列码，然后与桶总数取余， 所得到的结果就是 保存这个元素的桶的索引。
比如：散列码为 76268， 128个桶。则 该元素的桶的索引为 a1 % b1 = 108。如果该位置 已经有元素了，则会出现散列冲突hash collision。

为保持性能，需要制定初始的桶数。 桶数指 用于收集具有相同散列值的桶的数目。 ？？？ 这句话什么意思？ 桶数指的就是  需要放入的元素的数目 ？

标准类库 使用的桶数是2的幂。默认值是16.
如果无法知道要放入的元素的确切数目，导致散列表太满，则需要再散列 rehashed。 即创建桶数更多的表，并将所有元素插入到新表中，然后丢弃原来的表。

load factor 装填因子 决定何时对散列表 进行再散列。 例如： 默认装填因子为 0.75。即表中 超过75%的位置已经填入元素，这个表就会用双倍的桶数自动进行再散列。

hash table 实现 set，即HashSet类。

```
java.util.HashSet<E> 
HashSet() //空散列表

HashSet(int initialCapacity, float loadFactor)// 指定容量，装填因子

```


4. TreeSet

TreeSet是有序集合。  实现，基于红黑树 red-black tree。
TreeSet 默认假定插入的元素实现了Comparable 接口
public interface Comparable<T>
{
  int compareTo(T other);
}
如果排序 a位于b之前， a.compareTo(b); 返回负值； a<b；
如果     a位于b之后，                 返回正值。 a > b;
String 类 compareTo() 方法依据 字典序。



如果类 没有 实现 Comparable接口。 可以将Comparator对象传递给TreeSet 构造器来 告诉TreeSet使用不同的比较方法。
```
public interface Comparator<T>
{
    int compare(T a, T b);
}

```
compare 方法表示， a位于b前，返回负值，如果相等返回0，如果a>b 返回正直。
```
class ItemComparator implements Comparator<Item>
{
  public int compare(Item a, Item b)
  {
    String descA = a.getDescription();
    String descB = b.getDescription();
    return descA.compareTo(descB);
  }
}
```
然后将这个类的对象 传递给TreeSet构造器。
```
ItemComparator com = new ItemComparator();
SortedSet<Item> sortByDescrip = new TreeSet<>(com);
```
如果构造了一颗 带比较器的tree。 就可以在需要比较两个元素时使用这个对象。
比较器 没有任何 数据， 它只是比较函数的持有器。  这种对象称为 **函数对象**

用到的类和接口
```
java.lang.Comparable<T>
  int compareTo(T other)；

java.util.Comparator<T>
  int compare(T a, T b); // 两个对象 
 

java.util.SortedSet<E>


java.util.NavigableSet<E>

java.util.TreeSet<E>



```
5. 队列与双端队列

java.util.Queue<E> // 队列

java.util.Deque<E> //双端队列

java.util.ArrayDeque<E> 用初始容量16或 给定的容量构造一个无线双端队列。


6. 优先级队列 priority queue

优先级队列 中的元素 可以按照任意顺序插入， 却按照排序的顺序进行检索。  无论何时调用 remove方法， 总会获得 **当前优先队列** 中最小的元素。
优先队列使用 堆； 实现为 可以自我调整的二叉树，对树执行 add和remove，可以让最小元素移动到根。

与 TreeSet 一样。 既可以保存 实现了Comparable接口的类对象，也可以保存在构造器中提供比较器的 对象。


典型示例， 任务调度。任务有优先级，随机添加到队列中。 每当启动一个新的任务时，都将最高优先级 的任务 删除。


7. 映射表 Map

Java 类库 映射表（map） 数据结构 HashMap和TreeMap。
散列映射表  对键 进行散列 ，
键不能重复， 同一个键，第二次用put()时，返回 第一次添加的元素，并用第二次添加的元素 覆盖。
```
String a ="a";
Map<Integer, String> m = new HashMap<>();

m.put(1,a);
m.put(2,"c");
System.out.println(m);
//m.put(1,"b");
System.out.println(m.put(1,"b"));  //output: a

```
remove()
size()


映射表的视图。 一实现了Collection接口的 对象或者 它的子接口的视图。
三个视图： 键集、值集合  键与键/值对集。

Set<K> keySet();
Collection<K> values();
Set<Map.Entry<K,V>> entrySet()

Set<String> keys = map.keySet();
for(String key : keys){
    //
}
同时查看 键和值
```
for(Map.Entry<String, Employee> entry : staff.entrySet())
{
    String key = entry.getKey();
    Employee value = entry.getValue();

}

java.util.Map.Entry<K, V>
K getKey();
V getValue();

```

8. 弱散列映射表
WeakHashMap类 使用 弱引用保存键 weak reference 

WeakReference 对象 将引用保存到另外一个对象中， 就是散列表键。 WeakHashMap周期性检查队列。



9. LinkedHashSet 和 LinkedHashMap  用来记住插入元素项的顺序。

当条目插入到表中时，就会并入到双向链表中。

链接散列映射表 用访问顺序，而不是插入顺序对映射表条目进行迭代。 
即，每次调用 get或put 收到影响的条目将从当前的位置删除， 并放入到条目链表尾部
**条目在链表中位置会收到影响，而散列表中的桶不不受到影响。 一个条目总位于键散列码对应的桶中**
```
LinkedHashMap<K,V> (initialCapacity, loadFactor, true);
```
实现高速缓存。

10. 枚举集与映射表

EnumSet  枚举类型元素集的高效实现。 EnumSet类 没用构造器。 使用静态工厂方法 构造这个集。


11. 标识散列映射表

IdentityHashMap 散列值不是用Object.hashCode方法计算，而是由 System.identityHashCode()计算。 
这是Object.hashCode() 根据对象的内存地址来计算散列码所使用的方式。两个对象进行比较时，IdentityHashMap类使用==，而不使用equals。

不同的键对象，即便内容相同，也被视为不同对象。

```
java.util.WeakHashMap<K,V>


java.util.LinkedHashSet<E>


java.util.LinkedHashMap<K, V>


java.util.EnumMap<K extends Enum<K>, V>

java.util.IdentityHashMap<K,V>


java.lang.System 



```



集合 基本接口 Collection  Map.

标记接口 RandomAccess 这个接口没有任何方法， 可以用来检测一个特定集合是否支持高效的随机访问。
```
if(c instanceof RandomAccess){
    use random access algorithm
}else{
    use sequential access algorithm
}
```
ArrayList 和Vector 都实现了 RandomAccess 接口 

12. 视图与包装器

通过使用views 可以获得其他的实现了集合接口和映射表接口的对象。 

映射表类的keySet() 返回一个实现Set接口的类对象。 这个类的方法对原映射表进行操作。




13. 批操作 bulk operation



14. 算法







