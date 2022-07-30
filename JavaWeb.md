# JavaWeb相关知识总结

## Cookie和Session

### 1、Cookie

1. **Cookie是服务器通知客户端保存键值对的一种技术，客户端有个Cookie之后，每次请求都会发送给服务器**
2. **每个Cookie的大小不能超过4KB**
3. 如何创建Cookie以及流程图

![image-20211110085148795](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301114004.png)

4. 客户端查看响应头看是否有相同的key的cookie，如果有则用value去替换，没有则添加一个cookie

```java
//创建Cookie对象
Cookie cookie=new Cookie("name", "James");
//通知客户端保存cookie
resp.addCookie(cookie);
```

5. 如何获取所有的cookie
6. ![image-20211110093545484](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301114006.png)

7. 修改cookie的值

```java
//方案-： 创建一个要修改该的同名的cookie对象
//构造器中赋予新的cookie值
Cookie cookie=new Cookie("name", "linshuai");
// 方案二：调用指定cookie的setvalue方法
cookie.setValue("你好");
//调用response.addCookie(cookie)方法添加
resp.addCookie(cookie);
```

8. Cookie的生命控制
   1. 值得是cookie什么时候被销毁，通过setMaxAge方法来指定
   2. 正数，表示在指定的秒数后过期 
   3. 负数，表示浏览器一关，Cookie 就会被删除（默认值是-1） 
   4. 零，表示马上删除 Cookie

**Cookie** **有效路径** **Path** **的设置** 

Cookie 的 path 属性可以有效的过滤哪些 Cookie 可以发送给服务器。哪些不发。 path 属性是通过请求的地址来进行有效的过滤。 

```java
CookieA 	path=/工程路径 

CookieB  	path=/工程路径/abc 
请求地址如下： 
http://ip:port/工程路径/a.html 
CookieA 发送 
CookieB 不发送 

http://ip:port/工程路径/abc/a.html 
CookieA 发送 
CookieB 发送
    
Cookie cookie = new Cookie("path1", "path1");
// getContextPath() ===>>>> 得到工程路径 
cookie.setPath( req.getContextPath() + "/abc" );
// ===>>>> /工程路径/abc resp.addCookie(cookie); 
resp.getWriter().write("创建了一个带有 Path 路径的 Cookie");
```

用户免输入用户名登录练习流程示意图

![image-20211110122816898](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301114007.png)

### 2、Session

1. Session就是一个接口，Session就是会话，它用来维护一个客户端和服务器之间关联的一种技术
2. 每个客户端都有自己的一个Session会话
3. Session会话中，我们经常用来保存用户登录之后的信息