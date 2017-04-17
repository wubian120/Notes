# Java多线程 阻塞队列

1. 阻塞队列概念 BlockingQueue

常用于生产者和消费者场景， 生产者是往队列中添加元素的线程，消费者是从队列中取元素的线程。

常见阻塞场景：
(1) 当队列空时，消费端所有线程都会自动阻塞（挂起），直到有数据放入队列。

![](file://F:/brady/BlockingQueue1.jpg)

(2) 当队列满，则生产端所有队列阻塞， 直到队列有空的位置，线程被自动唤醒。

支持以上


2. Java中的阻塞队列

JDK7 提供7个阻塞队列：
  - ArrayBlockingQueue    : 数组结构的有界阻塞队列
  - LinkedBlockingQueue   : 链表结构的有界阻塞队列
  - PriorityBlockingQueue : 支持优先级排序的无界阻塞队列
  - DelayQueue            : 使用优先级队列实现的无界阻塞队列
  - SynchronousQueue      : 不存储元素的阻塞队列
  - LinkedTransferQueue   : 由链表组成的无界阻塞队列
  - LinkedBlockingDeque   : 链表组成的双向阻塞队列



3. BlockingQueue核心方法
放入数据
    - offer(object)

    - offer(E o, long timeout, TimeUnit unit)

    - put(object)

获取数据
    - poll(time)

    -  






















