# Java 集合

Java集合都在java.util中，
集合部分分为 接口与类两部分。
主要的接口：
Iterator<E>
Iterable<E>
ListIterable<E>
Collection<E>

List
Map
Set

类：
ArrayList



### Iterator和ListIterator

**ListIterator** 
```
public interface ListIterator<E> extends Iterator<E>
```
An iterator for lists that allows the programmer to traverse the list in either direction, modify the list during iteration, and obtain the iterator's current position in the list. A ListIterator has no current element; its cursor position always lies between the element that would be returned by a call to previous() and the element that would be returned by a call to next(). An iterator for a list of length n has n+1 possible cursor positions, as illustrated by the carets (^) below:
                      Element(0)   Element(1)   Element(2)   ... Element(n-1)
 cursor positions:  ^            ^            ^            ^                  ^
 
Note that the remove() and set(Object) methods are not defined in terms of the cursor position; they are defined to operate on the last element returned by a call to next() or previous().
This interface is a member of the Java Collections Framework.
 - void add(E e)
 - boolean hasNext()
 - boolean hasPrevious()
 - E next()
 - int nextIndex()
 - E previous()
 - int previousIndex()
 - void remove()
 - void set(E e) 
 //Replaces the last element returned by next() or previous() with the specified element (optional operation). This call can be made only if neither remove() nor add(E) have been called after the last call to next or previous.

 Parameters:
e - the element with which to replace the last element returned by next or previous
可能抛出的异常:
**UnsupportedOperationException** - if the set operation is not supported by this list iterator
**ClassCastException** - if the class of the specified element prevents it from being added to this list
**IllegalArgumentException** - if some aspect of the specified element prevents it from being added to this list
**IllegalStateException** - if neither next nor previous have been called, or remove or add have been called after the last call to next or previous

**Iterator接口**
 
Iterator<E> 接口， 无序集合，只能向后遍历。

 - boolean hasNext(), 
 - E next(), 
 - void remove();
 - default void forEachRemaining(Consumer<? super E> action)   1.8

LinkedIterator 接口, 有序集合实现， 可以双向遍历。
  add();
  previous();
  hasPrevious();































[关于Java Collections的几个常见问题](http://www.importnew.com/23715.html)

