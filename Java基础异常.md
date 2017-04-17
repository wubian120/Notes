##Java基础 异常


##异常基本
- 异常以方法为中心，
- 所有的异常对象都派生自Throwable类的实例
- Error类
- Exception类


### Error类
表示内部错误，（JVM出错或内存不足）一旦出现程序会自动结束。


### Exception类
- IOException

IO错误导致的异常
- RuntimeException

运行时异常，数组下标越界，

异常根据是否需要检查 分为unchecked   checked 

java.lang.RuntimeException 和 java.lang.Error 类及其子类是 unchecked exception，其余的就是 checked exception 了。

- try-catch-finally  捕获异常并处理
- throw              遇到错误抛出一个异常
- throws             声明一个方法可能抛出的异常



 通常，Java的异常(包括Exception和Error)分为可查的异常（checked exceptions）和不可查的异常（unchecked exceptions）。
      可查异常（编译器要求必须处置的异常）：正确的程序在运行中，很容易出现的、情理可容的异常状况。可查异常虽然是异常状况，但在一定程度上它的发生是可以预计的，而且一旦发生这种异常状况，就必须采取某种方式进行处理。

      除了RuntimeException及其子类以外，其他的Exception类及其子类都属于可查异常。这种异常的特点是Java编译器会检查它，也就是说，当程序中可能出现这类异常，要么用try-catch语句捕获它，要么用throws子句声明抛出它，否则编译不会通过。

     不可查异常(编译器不要求强制处置的异常):包括运行时异常（RuntimeException与其子类）和错误（Error）。

     Exception 这种异常分两大类运行时异常和非运行时异常(编译异常)。程序中应当尽可能去处理这些异常。

       运行时异常：都是RuntimeException类及其子类异常，如NullPointerException(空指针异常)、IndexOutOfBoundsException(下标越界异常)等，这些异常是不检查异常，程序中可以选择捕获处理，也可以不处理。这些异常一般是由程序逻辑错误引起的，程序应该从逻辑角度尽可能避免这类异常的发生。

      运行时异常的特点是Java编译器不会检查它，也就是说，当程序中可能出现这类异常，即使没有用try-catch语句捕获它，也没有用throws子句声明抛出它，也会编译通过。
       非运行时异常 （编译异常）：是RuntimeException以外的异常，类型上都属于Exception类及其子类。从程序语法角度讲是必须进行处理的异常，如果不处理，程序就不能编译通过。如IOException、SQLException等以及用户自定义的Exception异常，一般情况下不自定义检查异常。

###异常处理

异常处理机制为：抛出异常，捕捉异常。
        
        抛出异常：当一个方法出现错误引发异常时，方法创建异常对象并交付运行时系统，异常对象中包含了异常类型和异常出现时的程序状态等异常信息。运行时系统负责寻找处置异常的代码并执行。

        捕获异常：在方法抛出异常之后，运行时系统将转为寻找合适的异常处理器（exception handler）。潜在的异常处理器是异常发生时依次存留在调用栈中的方法的集合。当异常处理器所能处理的异常类型与方法抛出的异常类型相符时，即为合适 的异常处理器。运行时系统从发生异常的方法开始，依次回查调用栈中的方法，直至找到含有合适异常处理器的方法并执行。当运行时系统遍历调用栈而

**未找到合适** 的异常处理器，则运行时系统终止。同时，意味着Java程序的终止。

对于运行时异常、错误或可查异常，Java技术所要求的异常处理方式有所不同。

由于运行时异常的不可查性，为了更合理、更容易地实现应用程序，Java规定，运行时异常将由Java运行时系统自动抛出，允许应用程序忽略运行时异常。

对于方法运行中可能出现的Error，当运行方法不欲捕捉时，Java允许该方法不做任何抛出声明。因为，大多数Error异常属于永远不能被允许发生的状况，也属于合理的应用程序不该捕捉的异常。

对于所有的可查异常，Java规定：一个方法必须捕捉，或者声明抛出方法之外。也就是说，当一个方法选择不捕捉可查异常时，它必须声明将抛出异常。

能够捕捉异常的方法，需要提供相符类型的异常处理器。所捕捉的异常，可能是由于自身语句所引发并抛出的异常，也可能是由某个调用的方法或者Java运行时 系统等抛出的异常。也就是说，一个方法所能捕捉的异常，一定是Java代码在某处所抛出的异常。简单地说，异常总是先被抛出，后被捕捉的。

         任何Java代码都可以抛出异常，如：自己编写的代码、来自Java开发环境包中代码，或者Java运行时系统。无论是谁，都可以通过Java的throw语句抛出异常。

        从方法中抛出的任何异常都必须使用throws子句。

        捕捉异常通过try-catch语句或者try-catch-finally语句实现。

         总体来说，Java规定：对于可查异常必须捕捉、或者声明抛出。允许忽略不可查的RuntimeException和Error。



Throws抛出异常的规则：

    1) 如果是不可查异常（unchecked exception），即Error、RuntimeException或它们的子类，那么可以不使用throws关键字来声明要抛出的异常，编译仍能顺利通过，但在运行时会被系统抛出。

    2）必须声明方法可抛出的任何可查异常（checked exception）。即如果一个方法可能出现受可查异常，要么用try-catch语句捕获，要么用throws子句声明将它抛出，否则会导致编译错误

3)仅当抛出了异常，该方法的调用者才必须处理或者重新抛出该异常。当方法的调用者无力处理该异常的时候，**应该继续抛出**，而不是囫囵吞枣。


4）调用方法必须遵循任何可查异常的处理和声明规则。若覆盖一个方法，则不能声明与覆盖方法不同的异常。声明的任何异常必须是被覆盖方法所声明异常的同类或子类。

```
void method1() throws IOException{}  //合法    
   
//编译错误，必须捕获或声明抛出IOException    
void method2(){    
  method1();    
}    
   
//合法，声明抛出IOException    
void method3()throws IOException {    
  method1();    
}    
   
//合法，声明抛出Exception，IOException是Exception的子类    
void method4()throws Exception {    
  method1();    
}    
   
//合法，捕获IOException    
void method5(){    
 try{    
    method1();    
 }catch(IOException e){…}    
}    
   
//编译错误，必须捕获或声明抛出Exception    
void method6(){    
  try{    
    method1();    
  }catch(IOException e){throw new Exception();}    
}    
   
//合法，声明抛出Exception    
void method7()throws Exception{    
 try{    
  method1();    
 }catch(IOException e){throw new Exception();}    
}   

```

 判断一个方法可能会出现异常的依据如下：
     1）方法中有throw语句。例如，以上method7()方法的catch代码块有throw语句。
     2）调用了其他方法，其他方法用throws子句声明抛出某种异常。例如，method3()方法调用了method1()方法，method1()方法声明抛出IOException，因此，在method3()方法中可能会出现IOException。
2. 使用throw抛出异常
　  throw总是出现在函数体中，用来抛出一个Throwable类型的异常。程序会在throw语句后立即终止，它后面的语句执行不到，然后在包含它的所有try块中（可能在上层调用函数中）从里向外寻找含有与其匹配的catch子句的try块。
　　我们知道，异常是异常类的实例对象，我们可以创建异常类的实例对象通过throw语句抛出。该语句的语法格式为：
    throw new exceptionname;
    例如抛出一个IOException类的异常对象：
    throw new IOException;
    要注意的是，throw 抛出的只能够是可抛出类Throwable 或者其子类的实例对象。下面的操作是错误的：
    throw new String("exception");
    这是因为String 不是Throwable 类的子类。

     如果抛出了检查异常，则还应该在方法头部声明方法可能抛出的异常类型。该方法的调用者也必须检查处理抛出的异常。

       如果所有方法都层层上抛获取的异常，最终JVM会进行处理，处理也很简单，就是打印异常消息和堆栈信息。如果抛出的是Error或RuntimeException，则该方法的调用者可选择处理该异常。
```
package Test;  
import java.lang.Exception;  
public class TestException {  
    static int quotient(int x, int y) throws MyException { // 定义方法抛出异常  
        if (y < 0) { // 判断参数是否小于0  
            throw new MyException("除数不能是负数"); // 异常信息  
        }  
        return x/y; // 返回值  
    }  
    public static void main(String args[]) { // 主方法  
        int  a =3;  
        int  b =0;   
        try { // try语句包含可能发生异常的语句  
            int result = quotient(a, b); // 调用方法quotient()  
        } catch (MyException e) { // 处理自定义异常  
            System.out.println(e.getMessage()); // 输出异常信息  
        } catch (ArithmeticException e) { // 处理ArithmeticException异常  
            System.out.println("除数不能为0"); // 输出提示信息  
        } catch (Exception e) { // 处理其他异常  
            System.out.println("程序发生了其他的异常"); // 输出提示信息  
        }  
    }  
  
}  
class MyException extends Exception { // 创建自定义异常类  
    String message; // 定义String类型变量  
    public MyException(String ErrorMessagr) { // 父类方法  
        message = ErrorMessagr;  
    }  
  
    public String getMessage() { // 覆盖getMessage()方法  
        return message;  
    }  
}  



```

4.3 异常链

      1) 如果调用quotient(3,-1)，将发生MyException异常，程序调转到catch (MyException e)代码块中执行；

      2) 如果调用quotient(5,0)，将会因“除数为0”错误引发ArithmeticException异常，属于运行时异常类，由Java运行时系统自动抛出。quotient（）方法没有捕捉ArithmeticException异常，Java运行时系统将沿方法调用栈查到main方法，将抛出的异常上传至quotient（）方法的调用者：

         int result = quotient(a, b); // 调用方法quotient()
        由于该语句在try监控区域内，因此传回的“除数为0”的ArithmeticException异常由Java运行时系统抛出，并匹配catch子句：

       catch (ArithmeticException e) { // 处理ArithmeticException异常
System.out.println("除数不能为0"); // 输出提示信息
} 

        处理结果是输出“除数不能为0”。Java这种向上传递异常信息的处理机制，形成异常链。

       Java方法抛出的可查异常将依据调用栈、沿着方法调用的层次结构一直传递到具备处理能力的调用方法，最高层次到main方法为止。如果异常传递到main方法，而main不具备处理能力，也没有通过throws声明抛出该异常，将可能出现编译错误。

      3)如还有其他异常发生，将使用catch (Exception e)捕捉异常。由于Exception是所有异常类的父类，如果将catch (Exception e)代码块放在其他两个代码块的前面，后面的代码块将永远得不到执行，就没有什么意义了，所以catch语句的顺序不可掉换。

4.4 Throwable类中的常用方法
注意：catch关键字后面括号中的Exception类型的参数e。Exception就是try代码块传递给catch代码块的变量类型，e就是变量名。catch代码块中语句"e.getMessage();"用于输出错误性质。通常异常处理常用3个函数来获取异常的有关信息:

     getCause()：返回抛出异常的原因。如果 cause 不存在或未知，则返回 null。

　 getMeage()：返回异常的消息信息。
　 printStackTrace()：对象的堆栈跟踪输出至错误输出流，作为字段 System.err 的值。

     有时为了简单会忽略掉catch语句后的代码，这样try-catch语句就成了一种摆设，一旦程序在运行过程中出现了异常，就会忽略处理异常，而错误发生的原因很难查找。


###使用异常的最佳实践
1. 记得释放资源
```
public void dataAccessCode(){
    Connection conn = null;
    try{
        conn = getConnection();
        ..some code that throws SQLException
    }catch(SQLException ex){
        ex.printStacktrace();
    } finally{
        DBUtil.closeConnection(conn);
    }
} 
class DBUtil{
    public static void closeConnection(Connection conn){
        try{
            conn.close();
        } catch(SQLException ex){
            logger.error("Cannot close connection");
            thrownewRuntimeException(ex);
        }
    }
}
```
DB Util关闭连接工具类，最重要的部分在于finally，无论异常发不发生都会执行关闭连接操作，如果关闭发生异常会抛出一个RuntimeException。
 
2. 不要使用异常作控制流程之用
```
public void useExceptionsForFlowControl() {
    try{
        while(true) {
            increaseCount();
        }
    } catch(MaximumCountReachedException ex) {
    }
    //Continue execution
}
 
public void increaseCount()
    throws MaximumCountReachedException {
    if(count >= 5000)
        throw new MaximumCountReachedException();
}
```
生成栈回溯的代价很昂贵，栈回溯的价值是在于调试，而不是流程控制中。
 
3. 不要catch最高层次的exception
```
try{

}catch(Exception ex){
}
```
上面这种写法不建议，应该对具体异常分类处理。
 
4.尽量减小try代码块

5. 提早抛出异常


6. 延迟捕获

菜鸟和高手都可能犯的一个错是在程序有能力处理异常之前就捕获它。Java编译器通过要求检查出的异常必须被捕获或抛出而间接助长了这种行为。自然而然的做法就是立即将代码用try块包装起来，并使用catch捕获异常，以免编译器报错。
问题在于，捕获之后该拿异常怎么办？最不该做的就是什么都不做。空的catch块等于把整个异常丢进黑洞，能够说明何时何处为何出错的所有信息都会永远丢失。把异常写到日志中还稍微好点，至少还有记录可查。但我们总不能指望用户去阅读或者理解日志文件和异常信息。让readPreferences()显示错误信息对话框也不合适，因为虽然JCheckbook目前是桌面应用程序，但我们还计划将它变成基于HTML的Web应用。那样的话，显示错误对话框显然不是个选择。同时，不管HTML还是C/S版本，配置信息都是在服务器上读取的，而错误信息需要显示给Web浏览器或者客户端程序。 readPreferences()应当在设计时将这些未来需求也考虑在内。适当分离用户界面代码和程序逻辑可以提高我们代码的可重用性。

在有条件处理异常之前过早捕获它，通常会导致更严重的错误和其他异常。例如，如果上文的readPreferences()方法在调用FileInputStream构造方法时立即捕获和记录可能抛出的FileNotFoundException，代码会变成下面这样：




###参考资料
[全面理解java异常机制](http://www.cnblogs.com/yangming1996/p/6429922.html?utm_source=tuicool&utm_medium=referral)

[Java 异常处理](https://zhuanlan.zhihu.com/p/24043941)
[深入理解java异常处理机制](http://blog.csdn.net/hguisu/article/details/6155636)

[Java异常处理的10个最佳实践](http://www.importnew.com/20139.html)

[浅谈java异常](http://blog.csdn.net/qq_35101189/article/details/55509687?utm_source=tuicool&utm_medium=referral)

[有效处理java异常的三个原则](http://www.cnblogs.com/lijia0511/p/5390226.html)
