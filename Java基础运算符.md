# Java 运算符







位运算

& 与
| 或
^ 异或   两个操作数的位中，相同则结果为0，不同则结果为1。
~ 非

`>>>`  二进制右移， 用0填充高位。 `>>` 用符号位填充高位。

x << n;  x 可以为byte, short, char, int, long,  n只能是int。
二进制基础上 对数字进行平移运算。  主要有三种： << 左移，  >> 带符号右移，用符号位填充高位， >>> 无符号右移，将用0填高位。

1. 左移n  相当于 乘以2的n次方。
2. 右移n  相当于 除以2的n次方。
3. 无符号右移， 所有数字右移，低位舍弃，高位空位补零。 对于正数，    对于负数，

```
        int v = 9;  //4 * 8 bits  0000 1001

        System.out.print((v<<1)+ " "); //output: 18 0001 0010
        System.out.print((v<<2)+ " "); //        36 0010 0100

        System.out.println();
        v = 9;
        System.out.println("9 binary :" + Integer.toBinaryString(v));
        v = (v>>2);

        System.out.print(v+ " ");                          // 2
        System.out.print(Integer.toBinaryString(v)+ " "); // 10

        System.out.println();

        v = -9;
        System.out.println("-9 binary :" + Integer.toBinaryString(v)); //output: -9 1111 1111 1111 1111 1111 1111 1111 0111
        v = (v>>2);
        System.out.println(v+ " ");
        System.out.println(Integer.toBinaryString(v)+ " ");//output: -3 1111 1111 1111 1111 1111 1111 1111 1101

```
计算机对有符号数（包括浮点数）的表示有三种方法：
原码、反码和补码， 补码=反码+1。

在 二进制里，是用 0 和 1 来表示正负的，最高位为符号位，最高位为 1 代表负数，最高位为 0 代表正数。

    以Java中8位的byte为例，最大值为：0111 1111，最小值为1000 0001。

    那么根据十进制的数字，我们如何转换为二进制呢？对于正数我们直接转换即可，对于负数则有一个过程。

    以负数-9为例：

    1.先将-9的绝对值转换成二进制，即为0000 1001；

    2.然后求该二进制的反码，即为 1111 0110；

    3.最后将反码加1，即为：1111 0111

所以Java中Integer.toBinaryString(-9)结果为11111111111111111111111111110111. Integer是32位(bit)的.


###涉及知识点：

java各类型占字节, java 采用 unicode；
字节 byte，  位 bit;

1 boolean = 1 bit;
1 byte    = 8 bit;
1 char    = 2 byte;
1 short   = 2 byte;
1 int     = 4 byte;
1 long    = 8 byte;

1 float   = 4 byte;           IEEE754
1 double  = 8 byte;           IEEE754  



 计算机对有符号数（包括浮点数）的表示有三种方法：原码、反码和补码， 补码=反码+1。在 二进制里，是用 0 和 1 来表示正负的，最高位为符号位，最高位为 1 代表负数，最高位为 0 代表正数。

    以Java中8位的byte为例，最大值为：0111 1111，最小值为1000 0001。

    那么根据十进制的数字，我们如何转换为二进制呢？对于正数我们直接转换即可，对于负数则有一个过程。

    以负数-5为例：

    1.先将-5的绝对值转换成二进制，即为0000 0101；

    2.然后求该二进制的反码，即为 1111 1010；

    3.最后将反码加1，即为：1111 1011

所以Java中Integer.toBinaryString(-5)结果为11111111111111111111111111111011. Integer是32位(bit)的.

补码：



###参考资料：
http://blog.csdn.net/hopezhangbo/article/details/7348740?utm_source=tuicool&utm_medium=referral

Java基本数据类型总结
http://www.cnblogs.com/doit8791/archive/2012/05/25/2517448.html



关于Java中基本类型的长度相关基础知识
http://www.cnblogs.com/simoncook/p/5770973.html
