---
title: JavaWeb之cookie和session总结
copyright: true
comments: true
abstract: 欲读此文，请输入口令！！！
message: 欲读此文，请输入口令！！！
date: 2019-09-14 17:03:26
tags:
 - JavaWeb
 - 会话技术
 - Java面试题
 - cookie
 - session
categories: JavaWeb
description: 本文这里小编根据 cookie 和 session 的学习所做的一些个人总结。
password:
sticky:
---

# 会话技术

#### 1、什么是会话技术？

&emsp;&emsp;由于http协议是无状态的协议，也就是说当用户访问Web应用时，服务器无法区分该客户端是谁。就好像你在某宝结算购物车商品时，其服务器必须根据请求你的身份，找到你所结算的商品，这时用到的就是会话技术，在Web开发中，服务器跟踪用户信息的技术称为会话技术，简单的说就是帮助服务器记住客户端状态和区分客户端。

&emsp;&emsp;从打开浏览器访问某个站点直到浏览器关闭这一过程，就是一次会话。而会话记录就是起到记录这次会话中客户端的状态和数据的作用。

#### 2、会话技术可分为两种：

&emsp;&emsp; `Cookie` 和 `Session`。

 - Cookie：将数据存储在客户端本地，减少服务端的存储的压力，安全性相对不强，客户端可以清除Cookie。

 - Session：将数据存储到服务端，安全性相对较强，但是会增加服务器压力。

# cookie：客户端会话技术

#### 1、cookie 的简单介绍

&emsp;&emsp;Cookie是小段的文本信息。通过Cookie可以标识用户身份、记录用户名及密码、跟踪重复用户。

&emsp;&emsp;当客户端第一次访问服务器时，服务端会生成一个Cookie，通过Response响应返回给客户端，客户端将Cookie保存到本地某个指定目录中。当客户端再次访问服务端时，Cookie会跟着Request请求一起到服务器，服务器会先查找Cookie中记录的信息，返回相对应的信息给用户端。

&emsp;&emsp;Cookie是以键值对（Key/Value）的形式存储在本地硬盘，用户可以手动删除Cookie信息或者服务端通过cookie.setMaxAge(int seconds)设置Cookie在客户端的持久化时间，当过了持久化时间后，浏览器将自动删除该Cookie信息（如果不设置持久化时间，浏览器关闭时则Cookie信息将销毁）。

#### 2、cookie 的使用
 - cookie 的创建
 ```java
 Cookie c = new Cookie(key,value);
 ```
&emsp;&emsp;Cookie 默认浏览器关闭，则 Cookie 对象被销毁。我们可以通过 `setMaxAge(int expiry)` 方法设置cookie的生命周期，关闭浏览器后，cookie不被销毁。该方法的设置时间单位是：秒。

 - cookie 的添加
 ```java
 response.addCookie(c);
 ```
&emsp;&emsp;如果我们想替换原有的Cookie值，那么我们只需要创建一个Cookie与原有Cookie的key相同的Cookie即可。

 -  cookie 的获取
 ```java
 Cookie[] cookies = resquest.getCookies(); //通过request对象获取一个Cookie对象数组

 getName();    //通过Cookie对象获取Cookie的key值
 getValue();   //通过Cookie对象获取Cookie的value值
 ```
 - cookie 路径问题
&emsp;&emsp;关于cookie的路径问题，小编算是入坑了，cookie的默认路径是：`/项目名` ,如果当请求路径是该cookie的同级目录下或者是其子目录的时候是可以访问到该cookie，如果不是，那么我们会访问不到该cookie。

&emsp;&emsp;对于cookie的路径问题，这里小编推荐在添加cookie时设置该cookie的路径：`c.serPath("/")`,这样设置之后就不会出现cookie获取不到的问题了。

# session 服务端会话技术

#### 1、session 的简单介绍

&emsp;&emsp;Session对象是由服务器自动创建的与用户请求相关的对象。服务器为每个用户都生成一个Session对象，用于保存该用户的信息，跟踪用户的操作状态。简单的说，Session是一种把会话数据保存到服务器端的技术。

&emsp;&emsp;当浏览器第一次发送请求时，服务器自动生成了一个Session和一个Session ID用来唯一标识这个Session，并将其通过响应发送到浏览器。当浏览器第二次发送请求，会将前一次服务器响应中的Session ID放在请求中一并发送到服务器上，服务器从请求中提取出Session ID，并和保存的所有Session ID进行对比，找到这个用户对应的Session。

&emsp;&emsp;一般情况下，服务器会在一定时间内（默认30分钟）保存这个 Session，过了时间限制，就会销毁这个Session。在销毁之前，我们可以将用户的一些数据以Key和Value的形式暂时存放在这个 Session中。

#### 2、session 的使用
 - session 的创建
 ```java
 HttpSession session = request.getSession();
 ```
 - session 中的方法
 ```java
 setAttribute(key,value);  //绑定数据
 Object getAttribute(key); //获取绑定数据 
 removeAttribute(key);     //移除绑定数据 
 invalidate();             //删除Session对象 
 ```
 - session 的生命周期：
&emsp;&emsp;session的生命周期默认是 30 分钟，我们也可以在tomcat下的web.xml文件中进行设置：

```xml
<session-config>
 <session-timeout>30</session-timeout>
</session-config>
```
我们也可以使用session对象的该方法进行设置：`session.setMaxInactiveInterval();//设置session的生命周期`

# 面试题

## 1、cookie 和 session 的区别
1. 存在的位置 
&emsp;&emsp;Cookie保存在客户端，Session保存在服务器端，服务器可以为每个用户浏览器创建一个会话对象（Session对象），
&emsp;&emsp;`注意：`一个浏览器独占一个Session对象（默认情况下）。
&emsp;&emsp;因此，在需要保存用户数据时，服务器程序可以把用户数据写到用户浏览器独占的Session中，当用户使用浏览器访问其它程序时，其它程序可以从用户的Session中取出该用户的数据，为用户服务。但是要注意Session数据不宜过多

2. 安全性
 - Cookie是以明文的方式存放在客户端，安全性相对较弱，可以使用加密算法加密
 - Session是存放在服务器的内存中的，所以安全性好

3. 网络传输量 
&emsp;&emsp;Cookie通过网络在客户端与服务器端传输。而Session保存在服务器端，不需要传输。

4. 生命周期（20分钟为例）
&emsp;&emsp;Cookie的生命周期是累计的，从创建时，就开始计时，20分钟后Cookie的生命周期结束，Cookie就失效
&emsp;&emsp;Session的生命周期是间隔的，从创建时开始计时，如在20分钟内没有访问过该Session，那么Session将被清除。
&emsp;&emsp;如果在20分钟内，比如在第19分钟时，访问过该Session，它的生命期将重新开始计算下一个20分钟。
&emsp;&emsp;另外，关机、关闭Tomcat或者reload应用会造成Session的生命周期结束，但是对Cookie没有影响

5. 访问范围
 - Session为一个用户浏览器独享
 - Cookie为多个用户浏览器共享
 - 其实Session和Cookie是有着千丝万缕的联系的，Session本身也要使用Cookie才能实现。 
&emsp;&emsp;最后一个使用原则：因为Session会占用服务器的内存，因此不能过多地使用Session，不要向Session中放过多、过大的对象。

## 2、浏览器禁用Cookie之后，Session会有影响吗？如何使用Session？

&emsp;&emsp;答：当浏览器禁用Cookie之后，Session会有影响。当Cookie禁用后，浏览器就不能在接收或保存第三方的返回cookie数据，换句话说就是cookie中没有SessionID，当发送请求的时候，请求中不能带有SessionID。

&emsp;&emsp;解决方案：
 - 方式一：表单中做一个隐藏域，我们可以根据请求在后端根据session的`getId()`的方法获取SessionID，然后放在表单的隐藏域中。
 - 方式二：重写url路径：
&emsp;&emsp;重写url路径也有两种情况：一种是用于链接地址，表单提交地址的url；一种是用于重定向的url
```java
response.encodeURL(String utl);//encodeURL方法用在链接地址、表单提交地址。
response.encodeRedirectURL(String url);//用于重定向
```

`注意：`上面的两个方法只是重写url的地址，返回的是重写后的url，然后我们使用返回的url路径该怎么重定向就怎么重定向，该怎么提交表单地址就使用重写后的地址即可。

&emsp;&emsp;重写url路径这种方式是当发送请求的时候，该url自动的会将sessionID加到请求路径的后面。

&emsp;&emsp;最后一点：禁用Cookie不会影响请求转发的页面跳转，只会影响重定向的页面跳转。
