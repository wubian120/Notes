# Java多线程 线程池

**关注点**：
> ThreadPoolExecutor类，创建线程池的核心类。Executors静态工厂方法，通过这个类来创建线程池。 
 
> ExecutorService接口 An Executor that provides methods to manage termination and methods that can produce a Future for tracking progress of one or more asynchronous tasks.
> Executor接口  An object that executes submitted Runnable tasks.
> Executors类 为 Executor, ExecutorService, ScheduledExecutorService, ThreadFactory, and Callable提供工厂和工具方法， 
> 阻塞队列

## ThreadPoolExecutor类

java.util.concurrent.**TheadPoolExecutor** 继承自 java.util.cocurrent.AbstractExecutorService
implements 接口 Executor   ExecutorService


线程池解决两个问题:解决大量异步工作中，减少每个线程的创建开销，提供了一种执行一组任务集合的管理资源的方法，资源包括
线程，消耗。

构造函数，
```
**ThreadPoolExecutor**(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue<Runnable> workQueue, ThreadFactory threadFactory, RejectedExecutionHandler handler)
```
corePoolSize  核心池的大小
maximumPoolSize  最大线程数
keepAliveTime   没有任务执行时 最多保持多久会 终止
TimeUnit unit   七种时间单位  天，小时，分钟，秒
BlockingQueue<Runnable> workQueue  阻塞队列 
ThreadFactory threadFactory      线程工厂
RejectedExecutionHandler handler    拒绝处理任务时 策略， 


- 线程池大小
    - 核心线程池大小。 
    - 最大线程数。

在创建了线程池后，默认情况下，线程池中并没有任何线程，等到有任务来才创建线程去执行任务,
除非调用了prestartAllCoreThreads()或者prestartCoreThread()方法
当创建的线程数等于 corePoolSize 时，会加入设置的阻塞队列。
当队列满时，会创建线程执行任务直到线程池中的数量等于maximumPoolSize。


 - 阻塞队列
 
ArrayBlockingQueue ：   由数组结构组成的有界阻塞队列。
LinkedBlockingQueue ：  由链表结构组成的有界阻塞队列。
PriorityBlockingQueue ：支持优先级排序的无界阻塞队列。
DelayQueue：            使用优先级队列实现的无界阻塞队列。
SynchronousQueue：      不存储元素的阻塞队列。
LinkedTransferQueue：   由链表结构组成的无界阻塞队列。
LinkedBlockingDeque：   由链表结构组成的双向阻塞队列。

- 拒绝策略 -

ThreadPoolExecutor.AbortPolicy:         丢弃任务并抛出RejectedExecutionException异常。 (默认)
ThreadPoolExecutor.DiscardPolicy：      也是丢弃任务，但是不抛出异常。
ThreadPoolExecutor.DiscardOldestPolicy：丢弃队列最前面的任务，然后重新尝试执行任务（重复此过程）
ThreadPoolExecutor.CallerRunsPolicy：   由调用线程处理该任务

**当有请求到来时：**

若当前实际线程数量 少于 corePoolSize，                   即使有空闲线程，也会创建一个新的工作线程；
若当前实际线程数量处于corePoolSize和maximumPoolSize之间，并且阻塞队列没满，则任务将被放入阻塞队列中等待执行；
若当前实际线程数量 小于 maximumPoolSize，                但阻塞队列已满，则直接创建新线程处理任务；
若当前实际线程数量已经达到maximumPoolSize，              并且阻塞队列已满，则使用饱和策略。

### Executors

Executors弊端：

1）newFixedThreadPool 和 newSingleThreadExecutor:
主要问题是堆积的请求处理队列可能会耗费非常大的内存，甚至 OOM。
2）newCachedThreadPool 和 newScheduledThreadPool:
主要问题是线程数最大数是 Integer.MAX_VALUE，可能会创建数量非常多的线程，甚至 OOM。



###Executor接口
```
public interface Executor {

    void execute(Runnable command);
}
```


###ExecutorService接口##
```
public interface ExecutorService extends Executor {

    void shutdown();
    List<Runnable> shutdownNow();
    boolean isShutdown();
    boolean isTerminated();
    boolean awaitTermination(long timeout, TimeUnit unit) throws InterruptedException;

    <T> Future<T> submit(Callable<T> task);

    <T> Future<T> submit(Runnable task, T result);

    Future<?> submit(Runnable task);

    invokeAll(Collection<? extends Callable<T>> tasks)

```

An Executor that provides methods to manage termination and methods that can produce a Future for tracking progress of one or more asynchronous tasks.

ExecutorService 可以被停止，会导致拒绝新任务task。 
两种shutdown方法。 shutdown(),允许之前提交的任务继续执行完毕，但不再接受新任务。  
shutdownNow(); 所有任务 立即停止，返回结果。


**任务类型**

- CPU密集型任务配置尽可能少的线程数量：cpu+1
- IO密集型任务则由于需要等待IO操作，线程并不是一直在执行任务，则配置尽可能多的线程，如2*Ncpu。
- 混合型的任务，如果可以拆分，则将其拆分成一个CPU密集型任务和一个IO密集型任务，只要这两个任务执行的时间相差不是太大，那么分解后执行的吞吐率要高于串行执行的吞吐率，如果这两个任务执行时间相差太大，则没必要进行分解。我们可以通过Runtime.getRuntime().availableProcessors()方法获得当前设备的CPU个数。


```
public void execute(Runnable command) {
        if (command == null)
            throw new NullPointerException();
        /*
         *三步
         * 1. If fewer than corePoolSize threads are running, try to
         * start a new thread with the given command as its first
         * task.  The call to addWorker atomically checks runState and
         * workerCount, and so prevents false alarms that would add
         * threads when it shouldn't, by returning false.
         *
         * 2. If a task can be successfully queued, then we still need
         * to double-check whether we should have added a thread
         * (because existing ones died since last checking) or that
         * the pool shut down since entry into this method. So we
         * recheck state and if necessary roll back the enqueuing if
         * stopped, or start a new thread if there are none.
         *
         * 3. If we cannot queue task, then we try to add a new
         * thread.  If it fails, we know we are shut down or saturated
         * and so reject the task.



```

任务从提交到执行的步骤：

```
public void execute(Runnable command){
    //...
    //三步: 
    1.如果少于corePoolSize在运行，尝试启动一个新线程，并将command给他。
    call addWorker
    addIfUnderCorePoolSize(command)
=
         * 1. If fewer than corePoolSize threads are running, try to
         * start a new thread with the given command as its first
         * task.  The call to addWorker atomically checks runState and
         * workerCount, and so prevents false alarms that would add
         * threads when it shouldn't, by returning false.
         *
         * 2. If a task can be successfully queued, then we still need
         * to double-check whether we should have added a thread
         * (because existing ones died since last checking) or that
         * the pool shut down since entry into this method. So we
         * recheck state and if necessary roll back the enqueuing if
         * stopped, or start a new thread if there are none.
         *
         * 3. If we cannot queue task, then we try to add a new
         * thread.  If it fails, we know we are shut down or saturated
         * and so reject the task.


}
```

**ThreadPoolExecutor** 提交两种任务 Callable Runnable
1. Callable


2. Runnable
只执行没有返回结果，可以抛出异常。
通过submit函数提交，返回Future对象。
通过get获取执行结果。

**Java8新引入Executors.newWorkStealingPool()**


newWorkStealingPool方法有两种函数签名：
```
public static ExecutorService newWorkStealingPool(int parallelism)
public static ExecutorService newWorkStealingPool()
```
这两个方法用于创建ForkJoin框架中用到的ForkJoinPool线程池，第一个函数中的参数用于指定并行数，第二个函数没有参数，它默认使用当前机器可用的CPU个数作为并行数。


###实例



###参考资料：


[从阿里Java开发手册学习线程池的正确创建方法](http://www.cnblogs.com/javanoob/p/threadpool.html?utm_source=tuicool&utm_medium=referral)
[线程池的原理以及java的线程池框架](http://www.jsondream.com/2016/12/27/Thread-factory.html?utm_source=tuicool&utm_medium=referral)
[11 java 线程池 使用实例](http://www.cnblogs.com/wihainan/p/4765862.html)
[深入理解Java之线程池](http://www.cnblogs.com/absfree/p/5357118.html)
[Java并发包学习二-Executors介绍](http://qifuguang.me/2015/08/12/[Java%E5%B9%B6%E5%8F%91%E5%8C%85%E5%AD%A6%E4%B9%A0%E4%BA%8C]Executors%E4%BB%8B%E7%BB%8D/)
