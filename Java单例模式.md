#Java 单例模式

** 单例模式，又称单件模式或者单子模式，指的是一个类只有一个实例，并且提供一个全局访问点。**


1. 实现思路：

在单例的类中设置一个 private 静态变量instance，instance 类型为当前类，用来持有单例唯一的实例。
将（无参数）构造器设置为 private，避免外部使用 new 构造多个实例。
提供一个 public 的静态方法，如 getInstance，用来返回该类的唯一实例 instance。

- 懒汉式
- 恶汉式
- 双重校验锁
- 枚举           * 不常用？
- 静态内部类



设计模式--单例模式几种写法及比较
http://mp.weixin.qq.com/s/R7qTYwLwEtVc8o4q6NG8nQ













