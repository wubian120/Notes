# Java 代理

1. 代理模式

代理类 ，委托类 ，实现相同的接口。

通过代理类 控制对 委托类对象的访问；

当两个类需要通信时， 引入第三方代理类， 将两个类 解耦。  只需要了解 代理类即可。
代理真正调用的还是 委托类的方法。
按照代理的创建时期，代理类可以分为两种：
静态：由程序员创建代理类或特定工具自动生成源代码再对其编译。在程序运行前代理类的.class文件就已经存在了。
动态：在程序运行时运用反射机制动态创建而成


** java中的代理 java.lang.reflect.Proxy类**

通过下面Proxy静态方法创建代理对象，共三个参数，第一个类加载器，第二个通过接口指定生成那个对象的代理对象，通过接口指定，（隐含的意思如果想产生一个对象的代理对象，那么这个对象必须有接口。）第三个参数InvocationHandler用来指明产生这个代理对象用来做什么。
```
static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h)
```
具体实例：
```
/**
 * Created by Brady-Server on 2016/12/18.
 */
public interface Person {
    
    String sing(String name);
    
    String dance(String name);

}
//LiuDeHua.java
/**
 * Created by Brady-Server on 2016/12/18.
 */
public class LiuDeHua implements Person {

    @Override
    public String sing(String name) {
        System.out.println("Liu de hua sing " + name);

        return "finided singing";
    }

    @Override
    public String dance(String name) {
        System.out.println("Liu de hua sing " + name);
        return "finished dancing.";
    }
}
//LiudehuaAgent.java
public class LiudehuaAgent {
    private LiuDeHua liudehua = new LiuDeHua();

    public Person getProxy(){
        return (Person) Proxy.newProxyInstance(LiudehuaAgent.class.getClassLoader(), liudehua.getClass().getInterfaces(),
                new InvocationHandler() {
                    /**
                     * @param proxy   代理对象自己传递进来
                     * @param method 代理对象当前调用的方法传递进来
                     * @param args 方法参数
                     * @return
                     * @throws Throwable
                     */
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                if(method.getName().equals("sing")){
                    System.out.println("I'm his agent, pay 10000 to let him to sing");
                    return method.invoke(liudehua, args);
                }
                if(method.getName().equals("dance")){
                    System.out.println("I'm his agent, pay 10000 to let him to dance");
                    return method.invoke(liudehua, args);
                }

                return null;
            }
        });
    }
}

// 测试代码
public class AgentProxyTest {

    public static void main(String[] args){

        LiudehuaAgent agent = new LiudehuaAgent();
        Person p = agent.getProxy();

        String singValue = p.sing("忘情水");
        String danceValue = p.dance("江南style");

        System.out.println(singValue);
        System.out.println(danceValue);

    }
    
}

```



2. 动态代理 
- JDK实现动态代理
```

/**
 * Created by Brady-Server on 2016/11/21.
 */
public class UserManagerDynamicProxy implements InvocationHandler {

    private Object tarObj;

    public Object newProxyInstance(Object obj){

        this.tarObj = obj;
        /**
         * 第一个参数指定产生代理对象的类加载器，需要将其指定为和目标对象同一个类加载器
         * 第二个参数要实现和目标对象一样的接口，所以只需要拿到目标对象的实现接口类加载器，目标对象接口，
         * 第三个参数表明这些被拦截的方法在被拦截时需要执行哪个InvocationHandler的invoke方法；
         * 返回代理对象
         */
        return Proxy.newProxyInstance(tarObj.getClass().getClassLoader(),
                tarObj.getClass().getInterfaces(),this);
    }

    /**
     * 关联的实现类的方法被调用时将被执行。
     * @param proxy 代理
     * @param method 原对象被调用的方法
     * @param args   方法的参数
     * @return    原方法的返回值
     * @throws Throwable
     */
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable{
        Object ret = null;
        try{
            System.out.println("Start. invoke..");
            /**
            * 调用目标方法
            * */
            ret = method.invoke(tarObj,args);

        }catch (Exception e){
            e.printStackTrace();
            System.out.println(" error...");

            throw e;
        }

        return ret;

    }

}

// 测试类

public class TestClient {
    public static void main(String[] args){
//      UserManager userManager = new UserManagerProxy(new UserManagerImpl());
        UserManager userManager = (UserManager)new UserManagerDynamicProxy().newProxyInstance(new UserManagerImpl());
        userManager.addUser("11111","nick");

//        start add User...
//       ...UserManagerImpl.addUser()...
//        succeed...
    }
}


```
** 动态代理的优点：**
动态代理与静态代理相比较，最大的好处是接口中声明的所有方法都被转移到调用处理器一个集中的方法中处理
（** InvocationHandler.invoke **）。这样，在接口方法数量比较多的时候，我们可以进行灵活处理，而不需要像静态代理那样每一个方法进行中转。
而且动态代理的应用使我们的类职责更加单一，复用性更强

- cglib实现动态代理

JDK 动态代理局限性：必须实现一个或多个接口

如果想代理没有实现接口的类可以使用CGLIB包。
CGLIB是一个强大的高性能的代码生成包。它被许多AOP的框架（例如Spring AOP）使用，为他们提供方法的interception（拦截）。Hibernate也使用CGLIB来代理单端single-ended(多对一和一对一)关联。EasyMock通过使用模仿（moke）对象来测试java代码的包。它们都通过使用CGLIB来为那些没有接口的类创建模仿（moke）对象。
　　CGLIB包的底层是通过使用一个小而快的字节码处理框架ASM，来转换字节码并生成新的类。不鼓励直接使用ASM，因为它要求你必须对JVM内部结构包括class文件的格式和指令集都很熟悉。

CGLIB 主要实现原理是通过 对制定的目标类生成一个子类，并覆盖其中方法实现增强，因为采用的是 继承 因此不能对 final类 进行代理。

CGLIB的核心类：
    net.sf.cglib.proxy.Enhancer – 主要的增强类
    net.sf.cglib.proxy.MethodInterceptor – 主要的方法拦截类，它是Callback接口的子接口，需要用户实现
    net.sf.cglib.proxy.MethodProxy – JDK的java.lang.reflect.Method类的代理类，可以方便的实现对源对象方法的调用,如使用：
    Object o = methodProxy.invokeSuper(proxy, args);//虽然第一个参数是被代理对象，也不会出现死循环的问题。

net.sf.cglib.proxy.MethodInterceptor接口是最通用的回调（callback）类型，它经常被基于代理的AOP用来实现拦截（intercept）方法的调用。这个接口只定义了一个方法
public Object intercept(Object object, java.lang.reflect.Method method,
Object[] args, MethodProxy proxy) throws Throwable;

第一个参数是代理对像，第二和第三个参数分别是拦截的方法和方法的参数。原来的方法可能通过使用java.lang.reflect.Method对象的一般反射调用，或者使用 net.sf.cglib.proxy.MethodProxy对象调用。net.sf.cglib.proxy.MethodProxy通常被首选使用，因为它更快。


```
public class CglibProxy implements MethodInterceptor {

    private Object obj;

    public Object createProxy(Object target){
        this.obj = target;
        Enhancer enhancer = new Enhancer();
        enhancer.setSuperclass(this.obj.getClass());
        enhancer.setCallback(this);
        enhancer.setClassLoader(target.getClass().getClassLoader());

        return  enhancer.create();
    }

    /**
     * 在代理实例上处理方法并返回结果
     * @param proxy 代理类
     * @param method 被代理的方法
     * @param params 该方法的参数数组
     * @param methodProxy
     * @return
     * @throws Throwable
     */
    public Object intercept(Object proxy, Method method, Object[] params, MethodProxy methodProxy) throws Throwable {

        Object result = null;

        doBefore();
        result = methodProxy.invokeSuper(proxy,params);
        doAfter();

        return result;
    }
    private void doBefore(){
        System.out.println("Before method invoke.");
    }
    private void doAfter(){
        System.out.println("After method invoke.");
    }
}

public class HelloWorld {
    public void sayHelloWorld(){
        System.out.println("Hello World...");
    }
}
//测试类
public class HelloWorldTest {
    public static void main(String[] args){

        HelloWorld hw = new HelloWorld();
        CglibProxy cglibProxy = new CglibProxy();

        HelloWorld helloWorld = (HelloWorld)cglibProxy.createProxy(hw);
        helloWorld.sayHelloWorld();
    }

}


```





## 参考文档
  代理模式详解 http://blog.csdn.net/u013815218/article/details/52562536
- Java基础加强总结(三)——代理(Proxy) http://www.cnblogs.com/xdp-gacl/p/3971367.html
- Java动态代理的两种实现方法 http://blog.csdn.net/heyutao007/article/details/49738887
- Java代理模式详解 https://yq.aliyun.com/articles/48913
- 彻底理解JAVA动态代理http://www.cnblogs.com/flyoung2008/archive/2013/08/11/3251148.html
- Java动态代理的两种实现方法http://blog.csdn.net/heyutao007/article/details/49738887
十分钟理解Java之动态代理 http://www.jianshu.com/p/cbd58642fc08



