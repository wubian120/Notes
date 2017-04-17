# Java基础 static 关键字


如果类被声明为static， 只有一种情况，静态内部类。  外部类被声明为static无法通过编译。 





1. 

静态内部类 没有指向外部类的引用，且不能访问外部类非静态成员。
```
public class StaticTest {

    private static int y = 0 ;
    private static String str;

    public StaticTest(){
        System.out.println(" StaticTest constructor..");
    }
    //外部类静态代码块
    static {
        System.out.println(" StaticTest static code block.");
    }
    //外部类静态方法
    public static void T(){
        System.out.println("StaticTest static function ...");
    }
    //静态内部类
    static class InnerX{  
        private static int x = 0;
        static {
            System.out.println(" Inner class static block.." +  y);
        }

        public InnerX(){

            System.out.println("Inner class constructor.. ");

            T();
            
        }
    }

    public static void main(String[] args){
        InnerX innerX = new InnerX();

    }
}
运行结果：
 StaticTest static code block.
 Inner class static block..0
Inner class constructor.. 
StaticTest static function ...
```





## 参考资料

Java中的static关键字解析 http://www.cnblogs.com/dolphin0520/p/3799052.html

[Java静态类](http://www.cnblogs.com/Alex--Yang/p/3386863.html)


