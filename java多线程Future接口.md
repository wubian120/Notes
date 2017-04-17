#Java多线程Future接口Callable接口
Future<V> 异步计算的结果

可以检查计算是否完成，获取计算结果，取消中断任务。
get()返回计算结果。cancel() 取消。


java.util.concurrent
所有的方法
boolean cancel(boolean mayInterruptIfRunning)  尝试取消执行的任务。
V       get()                          等待一段时间计算完成，然后取回结果
V   get(long timeout, TimeUnit unit)   在一个给定时间内等待计算完成，然后取回结果
boolean isCancelled()              是否已经取消，在正常完成前
boolean isDone()                   是否已经完成


## class FutureTask<V> ##
实现了 Runnable  Future<V> RunnableFuture<V>

public class FutureTask<V>
extends Object
implements RunnableFuture<V>
一个可以取消的异步计算。
This class provides a base implementation of Future, with methods to start and cancel a computation, query to see if the computation is complete, and retrieve the result of the computation. The result can only be retrieved when the computation has completed; the get methods will block if the computation has not yet completed. Once the computation has completed, the computation cannot be restarted or cancelled (unless the computation is invoked using runAndReset()).
A FutureTask can be used to wrap a Callable or Runnable object. Because FutureTask implements Runnable, a FutureTask can be submitted to an Executor for execution.

In addition to serving as a standalone class, this class provides protected functionality that may be useful when creating customized task classes.

### Callable接口

与Runnable接口区别，可以返回结果。以下是Java8中，Callable接口源码
```
@FunctionalInterface
public interface Callable<V> {
    /**
     * Computes a result, or throws an exception if unable to do so.
     *
     * @return computed result
     * @throws Exception if unable to compute a result
     */
    V call() throws Exception;
}
```





[Java多线程与并发应用-(8)-Callable和Future](http://blog.csdn.net/lp1137917045/article/details/45347317?utm_source=tuicool&utm_medium=referral)


[ Android（Java）之多线程结果返回——Future 、FutureTask、Callable、Runnable](http://blog.csdn.net/yangzhaomuma/article/details/51722779?utm_source=tuicool&utm_medium=referral)

[Java多线程系列--“JUC线程池”06之 Callable和Future](http://www.cnblogs.com/skywang12345/p/3544116.html?utm_source=tuicool&utm_medium=referral#a1)
