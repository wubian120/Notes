#Java解惑 equals与==

## == 
 - 比较的是内存地址。
 - 基本类型比较
 

   == 比较的是 栈上的内容。


## equals() 

- 默认 比较引用。

自动变量 是什么 ？
基本类型的 自动变量 只能用== 来比较， 因为既不是类的对象 也不是引用。 equals是方法。

equals是 Object类的方法。 默认是比较对象的内存地址。

### String 的 equals 比较的是什么 ？






```

equals

public boolean equals(Object obj)
Indicates whether some other object is "equal to" this one.
The equals method implements an equivalence relation on non-null object references:

It is reflexive: for any non-null reference value x, x.equals(x) should return true.
It is symmetric: for any non-null reference values x and y, x.equals(y) should return true if and only if y.equals(x) returns true.
It is transitive: for any non-null reference values x, y, and z, if x.equals(y) returns true and y.equals(z) returns true, then x.equals(z) should return true.
It is consistent: for any non-null reference values x and y, multiple invocations of x.equals(y) consistently return true or consistently return false, provided no information used in equals comparisons on the objects is modified.
For any non-null reference value x, x.equals(null) should return false.
The equals method for class Object implements the most discriminating possible equivalence relation on objects; that is, for any non-null reference values x and y, this method returns true if and only if x and y refer to the same object (x == y has the value true).

Note that it is generally necessary to override the hashCode method whenever this method is overridden, so as to maintain the general contract for the hashCode method, which states that equal objects must have equal hash codes.

Parameters:
obj - the reference object with which to compare.
Returns:
true if this object is the same as the obj argument; false otherwise.
See Also:
hashCode(), Hashtable


```


对象池

局部变量表   在 JVM 栈部分。

对象创建于对象池中，而对象池则位于方法区中的常量池。 ???

《深入Java虚拟机 2003》 8.1 class文件 把它所有的引用符号保存在一个地方， 常量池。 每一个class文件有一个常量池，每个被JVM装载的类或者接口都有 一份内部版本的 常量池， 被称作运行时常量池。 




实例：
```
        String s1 = new String("Hello") ;
        String s2 = new String("Hello") ;

        System.out.println("s1 == s2: " + (s1 == s2));
        System.out.println(s1.equals(s2));
        // output:  false; true


        String s1 = "Hello" ;
        String s2 = "Hello" ;

        System.out.println(s1 == s2);
        System.out.println(s1.equals(s2));
        // output: true; true;


        Integer i1 = 40;
        Integer i2 = 40;

        Integer i3 = 400;
        Integer i4 = 400;

        System.out.println(i1 == i2);   //true
        System.out.println(i3 == i4);   //false 


        double d1 = 1.1;
        double d2 = 1.1;

        Double d3 = 1.1;
        Double d4 = 1.1;
        System.out.println(d1 == d2); // true
        System.out.println(d3 == d4); // false

```


```
// java.lang.Integer.java


    private static class IntegerCache {
        static final int low = -128;
        static final int high;
        static final Integer cache[];

        static {
            // high value may be configured by property
            int h = 127;
            String integerCacheHighPropValue =
                sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
            if (integerCacheHighPropValue != null) {
                try {
                    int i = parseInt(integerCacheHighPropValue);
                    i = Math.max(i, 127);
                    // Maximum array size is Integer.MAX_VALUE
                    h = Math.min(i, Integer.MAX_VALUE - (-low) -1);
                } catch( NumberFormatException nfe) {
                    // If the property cannot be parsed into an int, ignore it.
                }
            }
            high = h;

            cache = new Integer[(high - low) + 1];
            int j = low;
            for(int k = 0; k < cache.length; k++)
                cache[k] = new Integer(j++);

            // range [-128, 127] must be interned (JLS7 5.1.7)
            assert IntegerCache.high >= 127;
        }

        private IntegerCache() {}
    }


```




### 参考资料
Java常量池理解与总结 http://www.jianshu.com/p/c7f47de2ee80



