# Java Core 多线程
1. 多线程
Thread类
    - Thread(Runnable target)
    - void start() //启动线程
    - void run()  //调用关联的Runnable的run()方法
    - void interrupt() //向线程发送中断请求
    - static boolean interrupted() //测试当前线程是否被中断。静态方法，副作用，它将当前线程的中断状态重置为false。
    - boolean isInterrupted() // 测试线程是否被中断，这一调用不改变线程的中断状态。
    - static Thread currentThread() 返回当前执行线程的Thread对象。


Runnable接口
    - void run() //必须覆盖的方法，在其中提供所要执行的代码


2. 中断线程


3. 线程状态
6种状态
- New
- Runnable
- Blocked
- Waiting
- Timed waiting (计时等待)
- Terminated (被终止)
确定当前状态 getState()

new Thread() 新建
调用start()  线程处于runnable
抢占式调度
** 阻塞状态 **
    - 当一个线程试图获取一个内部对象锁 （不是java.util.concurrent中的锁）而该锁被其他线程持有。则该线程进入阻塞状态。

** 等待状态 **
    - 当线程等待另一个线程通知调度器一个条件时，它自己进入等待状态。 调用Object.wait()  Thread.join() 或者等待java.util.concurrent 中的Lock或Condition时。会出现等待。

** 计时等待 ** 
    - Thread.sleep() Object.wait, Thread.join(), Lock.tryLock(), Condition.await() 计时版。

** 线程终止 **
    - run() 正常退出
    - 因为一个没有捕获的异常终止了run() 而意外死亡


** 调度器 **
4. 线程属性

- 线程优先级

    每个线程都有自己的优先级，子线程继承父线程的优先级。 setPriority() 提高或降低线程优先级。
    线程调度器选择新线程时高度依赖操作系统。

- 守护线程 daemon

    t.setDaemon(true)； 将线程转换为守护线程，为其他线程提供服务。 必须在线程启动之前调用。

    ** 守护线程永远不应该去访问资源，如文件或者数据库。**

- 未捕获异常处理器
    
    线程run() 不能抛出任何被检测的异常，why？ 但是不被检测的异常会导致线程终止。
    异常被传递到一个用于未捕获异常的处理器。
    该处理器必须是一个 实现了 Thread.UncaughtExceptionHandler接口的 类。  该接口只有一个方法：
    void uncaughtException(Thread t, Throwable e);

    通过setUncaughtExceptionHandler() 为任何线程安装处理器。 static Thread.setDefaultUncaughtExceptionHanlder() 为所有线程安装一个默认处理器。

    如果不安装默认处理器，默认处理器为空。 此时的处理器就是该线程的ThreadGroup对象。

5. 同步

两个线程同时修改一个对象，

- 条件对象

    ** 条件对象用来管理获得锁但是却不能做有用工作的线程 **  一个锁对象可以有一个或者多个相关的条件对象。
    ** Lock 对象 Condition 对象 **


- synchronized 关键字
    - 锁 用来保护代码片段，任何时刻只能有一个线程执行被保护的代码。
    - 锁 可以用来管理试图进入被包括代码段的线程。
    - 锁 可以拥有一个或者多个相关的条件对象。
    - 条件对象 管理那些已经进入被保护的代码段但不能运行的线程。

** 每个对象都有一个内部锁 **

如果 synchronized 关键字声明 方法， 对象的锁将保护整个方法。

内部锁 ** 只有 ** 一个相关条件，  wait方法添加一个线程到等待集中， notifyAll/notify()方法 解除等待线程的阻塞状态。 


- 线程局部变量

为了避免共享变量，使用ThreadLocal辅助类为各个线程提供各自的实例。

6. 阻塞队列

生产者线程，向队列中插入元素，  消费者线程则取出它们。


7. Callable与Future 
    从JDK 1.5开始提供。解决Thread,Runnable 无法返回值的问题。
    - Callable<V> 接口 java.util.concurrent.Callable<V>
        参数类型表示返回值类型。 例如：Callable<Integer> 表示一个最终返回一个Integer对象的异步计算。
        可以抛出异常
        Callable只能在ExecutorService的线程池中跑？？？？

    - Future接口  java.util.concurrent.Future<V>
        对于具体Runnable或者Callable执行结果进行取消，查询是否完成，获取结果。
        通过get()获取结果，会阻塞直到任务返回结果。

```
public interface Future<V> {
    boolean cancel(boolean mayInterruptIfRunning);
    boolean isCancelled();
    boolean isDone();
    V       get() throws InterruptedException, ExecutionException;
    V       get(long timeout, TimeUnit unit)
             throws InterruptedException, ExecutionException, TimeoutException;
}

```

cancel方法用来取消任务，如果取消任务成功则返回true，如果取消任务失败则返回false。
参数mayInterruptIfRunning表示是否允许取消正在执行却没有执行完毕的任务，如果设置true，则表示可以取消正在执行过程中的任务。
如果任务已经完成，则无论mayInterruptIfRunning为true还是false，此方法肯定返回false，即如果取消已经完成的任务会返回false；
如果任务正在执行，若mayInterruptIfRunning设置为true，则返回true，若mayInterruptIfRunning设置为false，则返回false；
如果任务还没有执行，则无论mayInterruptIfRunning为true还是false，肯定返回true。

isCancelled方法表示任务是否被取消成功，如果在任务正常完成前被取消成功，则返回 true。

isDone方法表示任务是否已经完成，若任务完成，则返回true；

get()方法用来获取执行结果，这个方法会产生阻塞，会一直等到任务执行完毕才返回；

get(long timeout, TimeUnit unit)用来获取执行结果，如果在指定时间内，还没获取到结果，就直接返回null。

 - 判断任务是否完成；
 - 能够中断任务；
 - 能够获取任务执行结果。



    - FutureTask类 实现了RunnableFuture接口 可将Callable转换成Future和Runnable。
      java.util.concurrent.FutureTask<V>

```
public interface RunnableFuture<V> extends Runnable, Future<V> {  
    void run();  
}  

```
FutureTask 俩个构造方法：
```
public FutureTask(Callable<V> callable) 
{  
    if (callable == null)  
        throw new NullPointerException();  
    sync = new Sync(callable);  
}  
public FutureTask(Runnable runnable, V result) 
{  
    sync = new Sync(Executors.callable(runnable, result));  
}  

```
如上提供了两个构造函数，一个以Callable为参数，另外一个以Runnable为参数。
这些类之间的关联对于任务建模的办法非常灵活，允许你基于FutureTask的Runnable特性（因为它实现了Runnable接口），把任务写成Callable，然后封装进一个由执行者调度并在必要时可以取消的FutureTask。


FutureTask类是Future 的一个实现，并实现了Runnable，所以可通过Excutor(线程池) 来执行,也可传递给Thread对象执行。
如果在主线程中需要执行比较耗时的操作时，但又不想阻塞主线程时，可以把这些作业交给Future对象在后台完成，
当主线程将来需要时，就可以通过Future对象获得后台作业的计算结果或者执行状态。 

Executor框架利用FutureTask来完成异步任务，并可以用来进行任何潜在的耗时的计算。

一般FutureTask多用于耗时的计算，主线程可以在完成自己的任务后，再去获取结果。
FutureTask类既可以使用new Thread(Runnable r)放到一个新线程中跑，也可以使用ExecutorService.submit(Runnable r)放到线程池中跑，而且两种方式都可以获取返回结果，但实质是一样的，即如果要有返回结果那么构造函数一定要注入一个Callable对象。

8. Executors 
 
** java.util.concurrent.Executors **

 - 线程池 thread pool 包含大量空闲线程，将Runnable对象交给线程池，就会有一个线程调用run()。当run()结束，线程不会死亡，而是准备在池中为下一个请求服务。
 
 线程池还可以减少并发线程数目。

 
 - Executors类 许多静态工厂方法用来构建线程池。

- ExecutorService
- Future<V>
- 

- 多线程 join() 方法

- 预定执行
** ScheduledExecutorService **  接口 

- Fork-join 框架

** RecursiveTask<Integer> **

例子： 统计一个数组中有多少个元素 满足某个特定属性。 可以将数组分为二部分，分别统计，再将结果相加。

双端序列 deque 的递归方式， 覆盖compute方法来生成并调用子任务，然后合并结果。
每个工作线程，都有一个双端队列。 一个线程将子任务 压入 其双端队列队头，（只有一个线程可以访问队头，所以不需要加锁。） 一个线程空闲时，从另一个双端队列队尾‘密取’ ？（work stealing） 一个任务。 
```

class Counter extends RecursiveTask<Integer>
{
    public static final int THRESHOLD = 1000;
    private double[]     values;
    private int          from;
    private int          to;
    private Filter       filter;

    public Counter(double[] values, int from, int to, Filter filter){
        this.values = values;
        this.filter = filter;
        this.from   = from;
        this.to     = to;
    }

    @Override
    protected Integer compute() {
        if( to - from < THRESHOLD)
        {
            int count = 0;
            for(int i=from;i<to;i++){
                if(filter.accept(values[i]))
                    count++;
            }
            return count;
        }
        else{

            int mid = (from +to) /2;
            Counter first = new Counter(values,from, mid,filter);
            Counter second = new Counter(values,mid, to, filter);
            invokeAll(first,second);
            return first.join() + second.join();
        }
    }
}
```
// invokeAll(first, second)； 接收到很多任务阻塞， 直到所有任务都已经完成。 join()方法生成结果，并返回总和。

- 问题：
  - fork() 作用
  - fork-join 其他的应用场景及示例

9. 同步器
java.util.concurrent包， 管理相互合作的线程集的类， 为线程之间的 ** 共用集结点模式 （common rendezvous patterns）** 提供的“预置功能” (canned functionality)

- CyclicBarrier

- CountDownLatch

- Exchanger
  两个线程在同一个数据缓冲区的两个实例上工作
  一个线程向缓冲区填入数据，另一个线程消耗这些数据。当它们完成后，相互交换缓冲区。

- Semaphore

- SynchronousQueue类
  生产者与消费者线程配对的机制，即当一个线程调用SynchronousQuue.put()， 会阻塞直到另一个线程调用take() 为止。
  数据仅沿着一个方向传递，从生产者到消费者。
  SynchronousQueue类 


重要概念：
 - 信号量    管理 ** 许可证 permits ** 
   线程通过acquire请求许可，通过调用release释放许可。 
   信号量1968年由 Edsger Dijkstra发明， 作为 synchronization primitive, 解决线程同步问题，通过信号量实现有界序列。

 - CountDownLatch  让一个线程集等待知道技数变为0。 一次性，一旦计数为0,不能重用。
  举例，一个线程需要一些初始数据，工作器线程被启动并在门外等候，另一个线程准备数据。当数据准备好的时候，调用countDown， 所有工作器线程就可以继续运行。


 - 障栏 CyclicBarrier
  CyclicBarrier类实现了一个集结点 rendezvous 称为 barrier。
  例子： 大量线程运行一次计算的不同部分的情况。 当所有部分都计算完成时，需要将结果组合在一起。
  当一个线程计算完毕，让它运行到 barrier处，一旦所有线程都达到这个 barrier， barrier撤销， 线程继续运行。
  
  - Barrier被称为循环的 cyclic，因为可以  所有等待线程 被释放后被重用，因此不同于 CountDownLatch。

  - Phaser类 允许改变不同阶段中参与线程的个数。












## 参考资料

Java 多线程编程 http://blog.csdn.net/fengqilove520/article/details/53304036?utm_source=tuicool&utm_medium=referral

Fork and Join: Java也可以轻松地编写并发程序
http://ifeve.com/fork-and-join-java/

