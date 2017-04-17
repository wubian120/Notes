# 多线程

- 线程优先级 ：** 继承性， 规则性，随机性 **
    - 优先级具有继承性，如，线程A启动线程B， 则A与B优先级一样。
    - 优先级规则性，  CPU倾向把资源分给优先级高的线程。
    - 优先级随机性， 优先级不等于执行顺序。

- Java两种线程，用户线程和 守护线程。

** Thread类 自身不执行任何操作，它只是驱动赋于它的任务。

1. 基本线程机制

- Thread类
** Thread类有哪些方法和属性**
    -  static void sleep(long millis);
    -  void run()
    -  void start() // 将引发调用run
    -  Thread(Runnable target)
    
    - void interrupt()

    - static boolean interrupted() 测试当前线程



- Runnable 接口
    - void run() //必须覆盖此方法
    
    - sleep()
    - yield()

- daemon线程






- join()  interrupt
    1.1
     join()  一个线程在另一个线程之上调用join()方法， 效果是等待 另一个线程结束之后才恢复。
也可以调用join()时带上一个超时参数，即在这段时间到期时还没结束的话，join()总能返回。
     join()方法可以被中断，在调用线程上调用interrupt()， 需要用到try-catch子句。


- 线程组

 意义、用法
 （好像已经被废弃）

2. 共享受限资源
序列化访问共享资源，解决线程访问资源的冲突问题。
锁语句产生了互斥效应， 这种机制称为互斥量。mutex
** synchronized **

** java.util.concurrent.locks **
- 显式使用Lock对象


- 原子性与易变性

原子操作是不能被线程调度中断的操作。

** volitale 关键字 **

** 临界区 **

** 线程本地存储 **

3. 终结任务


在 run()方法中 设置一个 循环，然后 根据条件终止循环从而停止 线程。 from <Java Cookbook>

** 未捕获异常处理器 **




