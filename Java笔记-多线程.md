# Java笔记 多线程

## 基本线程机制
  - Thread类
  
  - Runnable接口 

  - Executors ExecutorService

  - Callable<V>

  - sleep()
  
  - 优先级
    只使用三种优先级：MAX_PRIORITY    NORM_PRIORITY  MIN_PRIORITY

  - yield() 
  
  - daemon线程

  - join()
  
  - 线程组与捕获异常

## 共享受限资源

  - 互斥量
    定义
    解决多线程并发冲突的问题，都是采用 **序列化访问共享资源** 的方式。
  
  - synchronized关键字

  - Lock

  - ReentrantLock
  http://hyxw5890.iteye.com/blog/1578597

  http://blog.csdn.net/startupmount/article/details/37080277?utm_source=tuicool&utm_medium=referral

  - volatile关键字

  - 原子类

  - 临界区 critical section

  - ThreadLocal类线程本地存储

  - 

## 终结任务
  - 



## 协作



### CountDownLatch 

### CyclicBarrier

### DelayQueue

### PriorityBlockingQueue

### Semaphore

### Exchanger

## 性能调优

  - 免锁容器

  - 乐观锁
  
  乐观加锁

  - ReadWriteLock

## 活动对象

  - Future<V>




