## Shiro 身份认证 ##

**身份验证**，即在应用中谁能证明他就是他本人。一般提供如他们的身份ID一些标识信息来表明他就是他本人，如提供身份证，用户名/密码来证明。
在shiro中，用户需要提供principals （身份）和credentials（证明）给shiro，从而应用能验证用户身份：

**principals**：身份，即主体的标识属性，可以是任何东西，如用户名、邮箱等，唯一即可。一个主体可以有多个principals，但只有一个Primary principals，一般是用户名/密码/手机号。


**credentials**：证明/凭证，即只有主体知道的安全值，如密码/数字证书等。
最常见的principals和credentials组合就是用户名/密码了。接下来先进行一个基本的身份认证。
 
另外两个相关的概念是之前提到的Subject及Realm，分别是主体及验证主体的数据源。

示例：

1.新建maven项目

2.在resources目录下添加 shiro.ini文件

3.写测试类：  

``` 
public static void main(String[] args)throws Exception {

    Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro.ini");
    SecurityManager securityManager = factory.getInstance();
    SecurityUtils.setSecurityManager(securityManager);
    Subject subject = SecurityUtils.getSubject();
    UsernamePasswordToken token = new UsernamePasswordToken("zhang","123");

    try{
        subject.login(token);
        System.out.println("login success.");
    }catch (AuthenticationException e){
        e.printStackTrace();
    }
    subject.logout();
}

```

2.1、首先通过new IniSecurityManagerFactory并指定一个ini配置文件来创建一个SecurityManager工厂；

2.2、接着获取SecurityManager并绑定到SecurityUtils，这是一个全局设置，设置一次即可；

2.3、通过SecurityUtils得到Subject，其会自动绑定到当前线程；如果在web环境在请求结束时需要解除绑定；然后获取身份验证的Token，如用户名/密码；

2.4、调用subject.login方法进行登录，其会自动委托给SecurityManager.login方法进行登录；

2.5、如果身份验证失败请捕获AuthenticationException或其子类，常见的如： DisabledAccountException（禁用的帐号）、LockedAccountException（锁定的帐号）、UnknownAccountException（错误的帐号）、ExcessiveAttemptsException（登录失败次数过多）、IncorrectCredentialsException （错误的凭证）、ExpiredCredentialsException（过期的凭证）等，具体请查看其继承关系；对于页面的错误消息展示，最好使用如“用户名/密码错误”而不是“用户名错误”/“密码错误”，防止一些恶意用户非法扫描帐号库；

2.6、最后可以调用subject.logout退出，其会自动委托给SecurityManager.logout方法退出。

** 从如上代码可总结出身份验证的步骤：**

1、收集用户身份/凭证，即如用户名/密码；

2、调用Subject.login进行登录，如果失败将得到相应的AuthenticationException异常，根据异常提示用户错误信息；否则登录成功；

3、最后调用Subject.logout进行退出操作。
 
如上测试的几个问题：

1、用户名/密码硬编码在ini配置文件，以后需要改成如数据库存储，且密码需要加密存储；

2、用户身份Token可能不仅仅是用户名/密码，也可能还有其他的，如登录时允许用户名/邮箱/手机号同时登录。 

![] (file://e:/20161214223926.png) 
