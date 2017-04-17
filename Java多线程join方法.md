# Java多线程join方法

1. join()属于Thread类的方法

    - void join() //等待终止指定线程

    - void join(long millis)  //等待终止指定线程死亡或者经过指定毫秒数。


2. 实例

```
public class JoinTest2 {
    
    public static void main(String[] args){
        try{
            ThreadA t1 = new ThreadA("t1");
            t1.start();
            t1.join();   // 这里 意味着，主线程在此等待 t1线程执行完毕再唤醒继续执行。
            System.out.printf(" %s finish\n", Thread.currentThread().getName());

        }catch (InterruptedException e){
            e.printStackTrace();
        }
    }

    static class ThreadA extends Thread{
        public ThreadA(String name){
            super(name);
        }
        
        @Override
        public void run() {
            System.out.printf("%s start\n",this.getName());

            for(int i=0;i<10;i++){
                ;
                System.out.printf(" %s finished..\n", this.getName());
            }
        }
    }
}

```
把指定的线程加入到当前线程，原本两个线程可以并发执行，join之后变成了两个线程顺序执行。比如在线程B中调用了线程A的Join()方法，直到线程A执行完毕后，才会继续执行线程B。


 A.join:在API中的解释是：堵塞当前线程B，直到A执行完毕并死掉，再执行=>A先执行完，再执行B;

 A.yield:A让出位置，给B执行，B执行结束后A再执行，跟join的意思正好相反










