# Java基础 数组

1. 初始化
数组变量是一个引用类型的变量，Java中数组是静态的， 即初始化后，所占用的内存空间，数组长度是不变的。 必须 ** 初始化 ** 后使用。

- 静态初始化： 初始化时指定数组元素的值。
  ```
    String[] datas = new String[]{"java","c++","html"};
  ```

- 动态初始化： 初始化指定长度， 由系统为元素分配初始值；
```
  String[] datas = new String[5];
```

数组类型                  初始化值
byte,short,long             0
float,double                0.0
char                       '\u0000'
boolean                    false
引用类型（类，接口）         null

```
        String[] books = new String[]
        {
            "Thinking in java","Effective java","concurrent java",
            "Spring in Action"
        };

        String[] names = {"John", "Steve","Bob", "Ben"};

        String[] strArr = new String[5];
```

数组变量在栈区，books, names,strArr。
数组对象在堆内存。 

数组一旦初始化完成，内存分配即结束。无法改变其长度。但是可以修改其值。
























9. 参考资料

java知识 之 数组及其内存管理
http://imtianx.cn/2016/11/17/java%20%E7%9F%A5%E8%AF%86%E4%B9%8B%20%E6%95%B0%E7%BB%84%E5%8F%8A%E5%85%B6%E5%86%85%E5%AD%98%E7%AE%A1%E7%90%86/