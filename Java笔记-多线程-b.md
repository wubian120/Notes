# Java 多线程

## 创建线程方法：
Thread类 
Runnable接口

Callable<T> 与 Future<V> 接口  返回值
怎么用？ 实例呢？

## 线程有几种状态：
《Thinking in Java》21章 21.4.2 
**线程状态**
四种状态：
  - new
  
  - Runnable
  
  - Blocked
    处于阻塞状态下，调度器将忽略，直到线程重新进入就绪状态。

    进入阻塞状态的原因
      - sleep(milliseconds)  任务中止执行 给定时间。
      - wait()  释放锁
      - 任务等待某个输入/输出完成
      - 任务试图在某个对象上调用同步synchronized方法，但对象锁不可用，因为另一个对象已经获得了这个锁。
      - 
  
  - Dead

## 线程中断
  - 


## 如何控制同步
 - synchronized
  
- synchronized怎么用

 - 互斥量
    定义：    解决多线程并发冲突的问题，都是采用 **序列化访问共享资源** 的方式。即，给定时刻只允许一个任务访问共享资源。
    通常在代码前加一条锁语句来实现。 一段时间内只有一个任务可以访问这段代码。 产生了互相排斥的效果， 这种机制称为 **互斥量 mutex**。

 - 显式锁Lock 

 - ReentrantLock
 
 - volatile关键字
 volatile的字段进行写，则所有读操作都可以看到这个修改。即便使用了本地缓存。即会被 写入主存。
 volatile无法工作的情况，即如果一个域的值依赖于 它之前的值，或者 一个域的值受到其他域的值的限制。
 
 **volatile用法** 

 - 原子性
 基本类型，除了long和double之外 都是原子操作。  64位long double变量的读取和写入 当作分离的32位2次操作来执行。也就是读写中间会有上下文切换（字撕裂）。

- 原子类
**可以暂缓**



- 临界区 critical section
防止多个线程同时访问同一段代码，用synchronized修饰一段代码，表示 一段临界区，同步控制块。
```
synchronized(syncObject){
    // this code can be accessed by only one task at a time
}
```
在进入这段代码前， 必须获得syncObject对象的锁。 其他线程必须等到锁被释放以后，才能进入临界区。

**模板方法**

也可以使用 Lock对象来创建临界区

三个例子：
CriticalSection.java

ExplicitCriticalSection.java

SyncObject.java

synchronized块，必须给定一个在其上进行同步的对象。 最合理的是使用其方法正在被调用的当前对象：
```
synchronized(this)
```
在这种方式中，


- 线程本地存储ThreadLocal类
线程本地存储是一种自动化机制，为使用相同变量的每个不同的线程都创建不同的存储。  如果5个线程都要使用变量x所表示的对象，那线程本地存储就会生成5个用于x的不同的存储块。
java.lang.ThreadLocal类

例子： ThreadLocalVariableHolder.java
ThreadLocal 对象通常当作 静态域存储。 创建ThreadLocal时，只能通过set() get()访问对象内容
```
    private static ThreadLocal<Integer> value = new ThreadLocal<Integer>(){
        private Random rand = new Random(47);
        protected synchronized Integer initialValue(){ return rand.nextInt(10000);}
    };
    public static void increment() { value.set(value.get() + 1);}
    public static int get() { return value.get(); }
```




###线程池###
**ThreadPoolExecutor类**
参见：**Java多线程 线程池**



###线程间协作###
- wait()
- notify(),notifyAll()

wait() 将任务挂起，等待nofity() notifyAll() 发生时， 任务被唤醒，去检查说产生的变化。  wait()提供了一种任务间对活动同步的方式。


调用sleep() yield() 锁没有释放。  
线程执行时 遇到 wait() 线程执行被挂起，同时释放锁。
wait() 毫秒参数， 与sleep() 不同的是：
1) wait()期间 锁是释放的；
2) 可以通过notify() notifyAll() 或时间到期，从 wait()中恢复执行。

另一种，没有参数， wait()无限等下去，知道接收到 notify() notifyAll()消息。

wait() notify() notifyAll() 是基类Object的一部分，而不是Thread类的一部分。

只能在 同步控制方法或同步控制块里 调用wait() notify()  notifyAll()。


###Executors###

Executor 接口


Executors类; java.util.concurrent.Executors  继承自Object
**全部都是静态方法**
Factory and utility methods for Executor, ExecutorService, ScheduledExecutorService, ThreadFactory, and Callable classes defined in this package. 
这个类支持以下方法:
- 创建返回一个ExecutorService. 通常用于配置。
- 创建返回一个ScheduledExecutorService. 通常用于配置。
- 创建返回一个wrapped ExecutorService.That disables reconfiguration by making implementation-specific methods inaccessible.
- 创建返回一个 ThreadFactory 设定新创建的线程 一个知道的状态。
- 创建返回一个Callable out of other closure-like forms, so they can be used in execution methods requiring Callable.


###ExecutorService接口###
扩展自Executor




###ThreadPoolExecutor类###
继承自AbstractExecutorService


An ExecutorService that executes each submitted task using one of possibly several pooled threads, normally configured using Executors factory methods.

线程池解决两个问题: they usually provide improved performance when executing large numbers of 异步asynchronous tasks, 
due to reduced per-task invocation overhead（开销） 减少每个任务的调用开销, and 
they provide a means of bounding and managing the resources, including threads, consumed when executing a collection of tasks. 

Each ThreadPoolExecutor also maintains some basic statistics, such as the number of completed tasks.

To be useful across a wide range of contexts, 
this class provides many adjustable parameters and extensibility hooks. 
However, programmers are urged to use the more convenient Executors factory methods 
Executors.newCachedThreadPool() (unbounded thread pool, with automatic thread reclamation), 
Executors.newFixedThreadPool(int) (fixed size thread pool)
Executors.newSingleThreadExecutor() (single background thread), that preconfigure settings for the most common usage scenarios. Otherwise, 

配置和调教这个类遵循以下规则：

Core and maximum pool sizes

On-demand construction


Creating new threads


第四个构造函数，
```
ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue<Runnable> workQueue, ThreadFactory threadFactory, RejectedExecutionHandler handler)
```
corePoolSize  核心池的大小
maximumPoolSize  最大线程数
keepAliveTime   没有任务执行时 最多保持多久会 终止

TimeUnit unit   七种时间单位  天，小时，分钟，秒


BlockingQueue<Runnable> workQueue  阻塞队列  看文档 api


ThreadFactory threadFactory      线程工厂


RejectedExecutionHandler handler    拒绝处理任务时 策略， 
the handler to use when execution is blocked because the thread bounds and queue capacities are reached

###join###

















































































































