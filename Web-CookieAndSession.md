# Web Cookie & Session

### Cookie

1. Cookie 定义
Cookie是请求头域和响应头域的字段。简单地说，就是伴随请求和响应的一组键值对的文本，小文本。

js 和 服务端都可以设置cookie。 

第一次请求的时候，服务端设置cookie，作为response返回，以后每一次request都会带上cookie。 这样服务端就知道 客户端 曾经来过。

 ![](./pics/Cookie-Work.png)

 ** cookie的内容主要包括：名字、值、过期时间、路径和域。**
 路径和域一起构成cookie的作用范围。
 所不设置cookie的过期时间那么就称这个cookie是会话级别的cookie，关闭浏览器cookie就消失。会话级别的cookie一般不存储在硬盘上而存储在内存里面。
 若设置了过期时间浏览器就会把cookie存储到硬盘上，关闭浏览器再打开，cookie会一直存在知道超过过期时间。
 存储在硬盘上的cookie可以被不同的浏览器所共享.


![](./pics/cooki-baidu.png)

1、可以实现自动登录的功能：
          当用户注册网站的时候我们可以把用来唯一标识用户id的cookie保存到客户端，当用户再次打开我那个站的时候会把这个id发送到服务器，检查用户是否选择自动登录，然后既可以为用户自动执行登陆操作。
 
2、cookie的使用方法 ：我们需要使用HttpServletResponse的addCookie方法把cookie插入到HTTP请求报头。我们需要使用HttpServletRequest的getCookie方法获得cookie对象的数组。

3. spring中设置cookie
```
    @ResponseBody
    @RequestMapping(value = "/cookie")
    public void addCookie(HttpServletRequest request, HttpServletResponse response){

        Cookie cookie = new Cookie("Test", "Hello cookie" + System.currentTimeMillis());
        cookie.setMaxAge(300); //unit is second  expire time
        response.addCookie(cookie);
    }

    @ResponseBody
    @RequestMapping(value = {"/showCookie"})
    public void showCookie(HttpServletRequest request, HttpServletResponse response){
        Cookie[] cookies = request.getCookies();
        if(cookies != null){
            for(Cookie c : cookies){
                System.out.println(c.getName() +" - " +c.getValue());
            }
        }else {
            System.out.println("Cookies is null");
        }
    }

    @ResponseBody
    @RequestMapping("/getCookie")
    public void getCookieBySpring(@CookieValue(value="test")String cookie){
        System.out.println(cookie);
        return;
    }

```

4. Cookie 与XSS 跨站点脚本攻击

Cookie可以包含一些敏感信息，比如常见的记住密码或者访问记录等。
全名：Cross Site Script，中文名：跨站脚本攻击。顾名思义，是指“HTML注入”纂改了网页，插入恶意的脚本，从而在用户用浏览网页的时候，控制用户浏览器的一种攻击。一般攻击的套路如图所示：

![](./pics/cookie-xss.png)

### Session 

1. Session定义
Session代表着服务器和客户端一次会话的过程。直到session失效（服务端关闭），或者客户端关闭时结束。

Session 是存储在服务端的，并针对每个客户端（客户），通过SessionID来区别不同用户的。Session是以Cookie技术或URL重写实现。默认以Cookie技术实现，服务端会给这次会话创造一个JSESSIONID的Cookie值。

2. 保存session id的方式 

a、可以采用cookie，这样就可以在交互的过程中自动的按照规则把这个表示发送给服务器。一般这个cookie的名字类似于SEEESIONID的，但是cookie可以被人为禁止。

b、所以也经常使用一种使用一种叫做URL重写的技术，就是把sessionid直接附加在URL路径的后面。

c、还有一种技术叫做表单隐藏字段。就是服务器会自动修改表单添加一个隐藏的字段，在表单提交的时候就会把这个session id传递会服务器。

这技术，也可以使用在Web安全上，可以有效地控制CSRF跨站请求伪造。

3. Session 过程
![](./pics/session-process.png) 

服务器首先检查这个客户端的请求里是否已经包含了一个session标识（session id）
如果含有sessionid则说明以前为此客户端创建过session服务器就会把这个按照session Id把session给检索出来（如果检索不到就会重新建立一个session），
如果客户端请求不包含session id，则为此客户端创建一个session并且生成一个与此对应的session id，
session id是一个不会重复的字符串，该id会在本次响应客户端的时候传送给客户端。

### 参考资料
JavaEE 要懂的小事：二、图解 Cookie（小甜饼）
http://www.bysocket.com/?p=362

** 未看 **
[Web安全之实战] 跨站脚本攻击XSS
http://www.bysocket.com/?p=123

http://www.bysocket.com/?p=384

Java Web中cookie和session详解
http://dxz.iteye.com/blog/2193399?utm_source=tuicool&utm_medium=referral

通过Spring Session实现新一代的Session管理
http://www.infoq.com/cn/articles/Next-Generation-Session-Management-with-Spring-Session?utm_source=tuicool&utm_medium=referral

JavaWeb学习总结(十二)——Session http://www.cnblogs.com/xdp-gacl/p/3855702.html


http://www.cnblogs.com/xdp-gacl/p/3859416.html

