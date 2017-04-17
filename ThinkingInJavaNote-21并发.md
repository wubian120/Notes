
## Thinking in Java Notes 21 并发concurrent




21.4.3 中断
在线程执行到一半 终止

这段介绍 Thread.interrupt()。  还有Executor， Future<?>调用cancel()中断某个特定任务。
例子：concurrency/Interrupting.java

cancel() 是一种中断由Executor启动的单个线程的方式。


可以中断对sleep()调用，但对于I/O操作，或者 试图获取synchronized锁 是无法中断的。
这意味着 I/O操作 可能会锁住 多线程程序。
略显笨拙的办法是  通过关闭 发生阻塞的底层资源。来释放锁。

被阻塞的NIO 通道会自动的相应中断：
NIOInterruption.java

线程被阻塞的情况：
 - sleep() 
 - wait()
 - I/O
 - synchronized 块 获取锁，但是锁在其他对象上

其中， I/O  synchronized方法 都是不可中断的。
I/O 通过关闭 底层资源  

ReentrantLock 使得在阻塞的任务 具备可以被中断的能力。

