# Java基础 final关键字

- stop Value change
- stop Method Overridding
- Stop Inheritance

1. 修饰类
不想被继承

2. 修饰方法

当一个方法不想被子类覆写(Override)时，可以用final来修饰。另外一方面，把方法用final来修饰也有一定的性能提升上的帮助，因为虚拟机知道它不会被覆写，所以可以以更简单的方式来处理。

private的方法，默认都会被编译器加上final.

3. 修饰值

值不能修改，只能被赋值一次，但是不一定非得在声明是就直接初始化。 下段代码也是合法的。
```
final int a ;
if(foo()){
    a = 3;
}else{
    a = 4;
}
```



4. 内存模型的作用 - 防止变量从构造方法中逸出
```
class Foo{
    int a ;
    Foo(int v){
        a = v;
    }
}
```
在多线程环境下，有可能出现，一个线程创建 Foo对象， 另一个则读取 该对象的a的值。 而此时可能读到还未正确初始化的a的值。


理解 Java 关键字 Finalhttp://toughcoder.net/blog/2016/11/12/understanding-java-keyword-final/?utm_source=tuicool&utm_medium=referral

[Final of Java](http://www.jianshu.com/p/f68d6ef2dcf0?utm_source=tuicool&utm_medium=referral)  undone!!
[final关键字介绍](http://www.cnblogs.com/cherryljr/p/6388373.html?utm_source=tuicool&utm_medium=referral)
[深入理解Java内存模型（六）——final](https://vinoit.me/2016/08/14/java-memeory-model-final/)
[]()

