## SpringMVCDe 相关介绍

SpringMVC 也叫 Spring web mvc。是 Spring 框架的一部分，是在 Spring3.0 后发布的。

1. SpringMVC的优点

   1. 基于 **MVC** **架构**
基于 MVC 架构，功能分工明确。解耦合，

   2. **容易理解，上手快；使用简单。**
就可以开发一个注解的 SpringMVC 项目，SpringMVC 也是轻量级的，jar 很小。不依赖的特定的接口和类。

   3. **作 为** **Spring** **框 架 一 部 分 ， 能 够 使 用** **Spring** **的** **IoC** **和** **Aop** **。 方 便 整 合**
        **Strtus,MyBatis,Hiberate,JPA** **等其他框架。**

   4. SpringMVC 强化注解的使用，在控制器，**Service**，**Dao** **都可以使用注解。方便灵活。**

   5. 使用@Controller 创建处理器对象,@Service 创建业务对象，      @Autowired 或者@Resource
    在控制器类中注入 Service, Service 类中注入 Dao。
2. SpringMVC的大致工作原理
   1. 用户通过界面发送请求(index.jsp)
   2. DispatcherServlet(servlet)进行转发
   3. 分配给Controller对象(@Controller注解创建的对象)
## 第一个SpringMVC项目
   - 项目的需求分析：用户在页面发送一个请求，请求交给springmvc的控制器对象，并显示请求处理的结果在结果页面显示一个欢迎语句
   - 开发流程
   - 实现步骤
     1. 新建maven项目，加入spring-webmvc依赖，这间接把spring依赖加入了，加入servlet和jsp依赖
     2. 在web.xml文件中注册springmvc的核心对象DispathcherServlet(这个对象是衡量你是否用了springmvc的标准之一)
        1. 设置DispathcherServlet对象在服务器启动的时候就创建加载
        2. 指定DispathcherServlet对象读取springmvc配置文件的位置
     3. DispathcherServlet叫做中央调度器，是一个servlet，他的父类继承HttpServlet
     4. DispathcherServlet页叫做前端控制器(front controller)
     5. DispathcherServlet负责接收用户提交的请求，
            调用其他的控制器对象，并把请求处理的结果显示给用户
     6. 创建控制器类
         1. 在类的上面加入@Controller注解，创建对象，并放入到springmvc容器中
         2. 在类中的方法上面加入@RequestMapping注解。
     7. 创建一个show.jsp作为最后结果的显示页面
     8. 创建springmvc的配置文件，其类型和spring的配置文件类型相同
        1. 声明组件扫描器，指定 **@Controller** 注解的包名

### 加入servelt,jsp,Springmvc依赖
```xml-dtd
<!--    servlet依赖-->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
            <scope>provided</scope>
        </dependency>
        <!--    springmvc依赖-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.5.RELEASE</version>
        </dependency>
```
### 在web.xml文件中注册DispathcherServlet对象
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
	<!-- 声明和注册springmvc的核心对象DispatcherServlet
     需要在Tomcat服务器启动后，就要创建DispathcherServlet实例
     因为DispatcherServlet在他的创建过程中，会同时创建springmvc容器，
     读取springmvc的配置文件，会把这个配置文件的对象都创建好，当用户发起请求时就可以直接使用对象了

     servlet的初始化会执行init()方法，DispatcherServlet在intit()中{
        //创建容器，读取配置文件
        WebApplicationContext ctx=new ClassPathXmlApplicationContext("springmvc.xml");
        //把容器对象放入到ServletContext中
        getServletContext().setAttribute(key,ctx);
     }-->
		<!--不能正确加载/WEB-INF/DispatcherServlet-servlet.xml
        springmvc在创建容器的时候，读取的配置文件默认是/WEB-INF/<servlet-Name>-servlet.xml
        自定义springmvc读取的配置文件的位置-->
     <servlet>
         <servlet-name>DispatcherServlet</servlet-name>
         <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--  在tomcat启动后，创建Servlet对象，表示tomcat创建对象的顺序，值越小，越优先创建-->
         <load-on-startup>1</load-on-startup>
         <init-param>
       	<!-- contextConfigLocation:springmvc的配置文件的位置的属性-->
             <param-name>contextConfigLocation </param-name>
		<!-- 指定自定义文件的位置-->
             <param-value>classpath:springmvc.xml</param-value>
         </init-param>
     </servlet>
    <servlet-mapping>
        <servlet-name>DispatcherServlet</servlet-name>
<!--        使用框架的时候，url-pattern可以使用两种值
            1.使用扩展名的方式，语法 *.xxxx, xxxx是自定义的扩展名，常用的方式 *.do, *.action, *.mvc等等
                http://localhost:8080/01-spring/some.do
                http://localhost:8080/01-spring/other.do
            2.使用"/"的方式
-->
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>
</web-app>
```
### 创建控制器类
```java
/**
 * @Controller:创建处理器对象，对象放在springmvc容器
 * 位置：类的上面
 * 和spring中的@Service，@Component都是用来创建对象的
 * 只不过这个对象可以用来处理对应的请求
 */
//用value属性来指定对象被创建时的名称
@Controller(value="MyController")
public class MyController {
    /**
     * 处理用户提交的请求，springmvc中是使用方法来处理的。
     * 方法时自定义的，可以有多种返回值，多种参数，方法名称自定义
     */
    
    /**
     *准备使用doSome方法处理some.do请求。
     * @RequestMapping:请求映射，作用是吧一个请求地址和一个方法绑定在一起
     *                  一个请求指定一个方法处理
     *     属性：1.value 是一个String，表示请求的uri地址(some.do).
     *            value的值必须是唯一的，不能重复。使用时，推荐地址以"/"开头
     *          如果有多个请求可以用括号括起来，逗号进行分隔
     *           @RequestMapping(value={"/some.do","/first.do"}) // 表示处理some.do这个请求
     *     位置: 1.在方法的上面，
     *           2.在类的上面
     * 说明:使用RequestMapping修饰的方法叫做处理器方法或者控制器方法。
     * 使用@RequestMapping修饰的方法可以处理请求的，类似servlet中的doGet，doPost方法
     * 返回值：ModelAndView
     * Model：数据，请求处理完成后，要显示给用户的数据
     * View： 视图，比如jsp等等
     */
    @RequestMapping(value="/some.do") // 表示处理some.do这个请求
    public ModelAndView doSome(){
        //处理some.do请求，相当于service调用处理完成了。
        ModelAndView mv=new ModelAndView();
        //添加数据,框架在请求的最后把数据放到request作用域相当于是
		//request.setAttribute("msg","欢迎使用springMVC开发web项目");
        mv.addObject("msg","欢迎使用springMVC开发web项目");
        mv.addObject("fun","这SpringMVC可真的是有趣");
        //指定视图，指定视图的完整路径
        //框架对视图执行的forward操作，request.getRequestDispather("/show.jsp").forward()
        mv.setViewName("/show.jsp");
		//返回mv
        return mv;
    }
}
```
### 创建show.jsp文件用于显示最后的结果
```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h3>show.jsp</h3>
    <h3>msg数据:${msg}</h3>
    <h3>fun数据:${fun}</h3>
</body>
</html>
```

### 在springmvc的配置文件中注册扫描器
```xml-dtd
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
<!--    声明组件扫描器-->
    <context:component-scan base-package="controller"/>
</beans>
```
### 项目的结构图
![image.png](https://i.loli.net/2021/03/30/k25RgCKU3DZFpAt.png)

### 项目的运行结果
![image.png](https://i.loli.net/2021/03/30/WGSQ6F2dz17uvLk.png)
![image.png](https://i.loli.net/2021/03/30/arlUmXjeCvWKnDM.png)

### 修改show.jsp文件的位置
   - 为了防止用户直接通过地址栏来访问show.jsp文件，我们在WEB—INF下创建了view包，将show.jsp文件存放在了这里，因为WEB-INF文件夹对外是不开发的，所以用户直接输入地址是访问不到的
   - ![image.png](https://i.loli.net/2021/03/31/OxkgIynCqfYSR84.png)
### 设置视图文件的路径
1. 在springmvc的配置文件中声明视图解析器
```xml
   <!--    声明springmvc框架中的视图解析器，帮助开发人员设置视图文件的路径-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
<!--        文件的前缀-->
        <property name="prefix" value="/WEB-INF/view/"/>
<!--        文件的后缀-->
        <property name="suffix" value=".jsp"/>
    </bean>
```
1. 声明了视图解析器后如何填写资源路径
```java
    @RequestMapping(value="/some.do") // 表示处理some.do这个请求
    public ModelAndView doSome(){
        //处理some.do请求，相当于service调用处理完成了。
        ModelAndView mv=new ModelAndView();

        //添加数据,框架在请求的最后把数据放到request作用域相当于是
       //request.setAttribute("msg","欢迎使用springMVC开发web项目");
        mv.addObject("msg","欢迎使用springMVC开发web项目");
        mv.addObject("fun","这SpringMVC可真的是有趣");

        //指定视图，指定视图的完整路径
        //框架对视图执行的forward操作，request.getRequestDispather("/show.jsp").forward()
        // mv.setViewName("/show.jsp");
        // mv.setViewName("/WEB-INF/view/show.jsp");
        // 当配置了视图解析器后，可以使用逻辑名称(文件名的方式)，指定视图
        // 框架会使用视图解析器的前缀 + 逻辑名称 +后缀组成完整路径，这里就是字符串连接操作
        mv.setViewName("show");
        // 返回mv
        return mv;
    }
```
### 项目的总结
   当用户点击超链接后，浏览器的执行顺序分析
   - 首先浏览器将请求发送给Tomcat服务器
   - Tomcat服务器会去加载web.xml文件，通过servlet的url-pattern属性发现了  **<url-pattern>*.do</url-pattern>**的请求是交给中央控制器DispathcherServlet的，
   - 而我们在Tomcat启动的时候就创建了DispathcherServlet的对象，而这个对象是一个servlet，它在初始化的时候会加载springmvc的配置文件
   -   <context:component-scan base-package="controller"/>通过扫描器的包找到添加了注解的类
   -   发现类里面有一个@Componnet注解的类，这表示这个类是专门用来处理请求的，在这个类里发现了@RequestMapping(value="/some.do") 这表示处理some.do这个请求，
   -   在这个方法里将内容都保存到了request域当中，通过请求转发的方式到show.jsp完成页面的展示工作
   -   在项目中的**路径问题**:
       -  例如: ```<form action="user/receive.do" method="post">```的绝对路径是/项目名/user/receive.do
       -  例如:```<form action="/user/receive.do" method="post">```的路径user会被当做项目名

### 在类的上面使用RequestMapping注解
   1. @RequestMapping注解
      1. value属性: 所有请求地址的公共部分,叫做模块名称
      2. 位置: 放在类的上面
   2. @RequestMapping注解
      1. value属性: 表示处理的方法的相对路径，要求路径要和访问的路径保持一致
```java
@RequestMapping("/test")
@Controller(value="MyController")
public class MyController {
 //表示的路径是:  /项目名/test/first.do
 //            /项目名/test/some.do 因为都有相同的地址部分/test 所以在类上添加了注解
 @RequestMapping(value={"/some.do","/first.do"},method ={RequestMethod.GET}) 
```

### 对方法指定请求的方式
   1. 使用@RequestMapping的注解
      1. 属性:method 表示请求的方式，值是RequestMethod类枚举值
      2. get方法的请求表示 RequestMethod.GET
      3. 指定了get方式就只能用get方式进行访问，如果请求方式不匹配，网页405错误
      4. 如果没有写method属性默认表示get和post方式访问都可以
```java
@RequestMapping(value={"/some.do","/first.do"},method ={RequestMethod.GET})
```
### 处理器方法的参数
1. HttpServletRequest
   1. 如果需要使用HttpServletRequest的方法就需要将其定义在方法的形参上
   2. 通过使用```request.getParamerter("参数名")```的方式获取值
2. HttpServletResponse
3. HttpSession
4. 请求中所携带的参数
### 1.逐个接收参数
#### 定义index.jsp界面传递name和age属性的值
```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>SpringMVC项目</title>
</head>
<body>
    <p>提交参数给Controller</p>
    <form action="user/receive.do" method="post">
        姓名: <input type="text" name="name"/> <br>
        年龄: <input type="text" name="age"><br>
        <input type="submit" value="提交">
    </form>
</body>
</html>
```
#### 定义处理器方法参数
- 要求处理器方法和请求中参数名必须一致，同名的请求参数赋值给同名的形参
- 框架接收请求参数的大致流程
```java
//1.使用request对象接收请求参数
          String strName=request.getParameter("name");
          String strAge=request.getParameter("age");
//2.springmvc框架通过DispatcherServlet调用MyController对象的doOther方法
         //调用方法时，按名称对应，把接收的参数赋值给形参,和参数的位置无关
          doOther(strName,Integer.valueOf(strAge));
          //框架会提供类型转换的功能，能把String转为init，long，float，double等类型
```
- 实现代码
```java
  @RequestMapping(value="/receive.do") 
                                        // 参数使用Integet是为了防止用户传入值为空
    public ModelAndView doOther(String name,Integer age){
        ModelAndView mv=new ModelAndView();
        mv.addObject("MyName",name);
        mv.addObject("MyAge",age);
        mv.setViewName("show");
        return mv;
    }
```
#### **注意事项**
1. 当用户传入参数时，如果age参数没有值，或者不能够转化成Integer类型会出现400错误，但不会抛出异常，而是将异常信息写入了框架的日志(**NumberFormatException**)。400错误是客户端错误，表示提交参数过程中，发生了问题
2. 当用户提交的数据是以post方式发送的时候，如果出现中文会乱码

### 解决以post方式传递中文乱码问题
1. 可以在方法的参数列表加入HttpServletResponse参数，调用```javaresponse.setCharacterEncoding("utf-8");```，但是每个方法都需要添加，复用性差(不推荐)
2. 使用功能过滤器解决乱码问题，可以自定义，也可以使用框架中提供的过滤器**CharacterEncodingFilter**
#### CharacterEncodingFilter对象属性
```java
    private String encoding;
    private boolean forceRequestEncoding;
    private boolean forceResponseEncoding;
    //  默认为false，我们需要将它们修改为真
    public CharacterEncodingFilter() {
        this.forceRequestEncoding = false;
        this.forceResponseEncoding = false;
    }
```

#### 在web.xml文件中进行注册
```xml
<!--    注册声明过滤器,为了解决post方式乱码问题-->
    <filter>
        <filter-name>characterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
<!--        设置项目中使用的字符编码-->
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
<!--        强制请求对象使用encoding编码的值-->
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
<!--        强制响应对象使用encoding编码的值-->
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>characterEncodingFilter</filter-name>
<!--
        /*: 表示强制所有请求都先通过过滤器处理
-->
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```
### 当请求中的参数和控制器方法的形参不同名时
- 用户界面(发送的参数是sname和sage)
 ```java <p>当请求中参数名和控制器方法形参名不同时</p>
    <form action="user/requestParam.do" method="post">
        姓名: <input type="text" name="sname"/> <br>
        年龄: <input type="text" name="sage"><br>
        <input type="submit" value="提交">
    </form>
 ```
- 控制器方法的实现
```java
 /**
 * 请求中参数名和处理器方法的形参名不一样
 * @RequestParam: 解决请求中参数名和方法名形参不一样的问题
 *              属性：value: 请求中参数的名称
 *                    required: 是一个boolean，默认是true
 *                    true:表示请求中必须包含此参数，如果没有参数的话会出现400错误
 *              位置：在处理器方法的形参定义的前面
*/
    @RequestMapping(value="/requestParam.do")
public ModelAndView doReceive
(@RequestParam(value="sname",required = false) String name,
 @RequestParam(value="sage",required = false) Integer age){
        ModelAndView mv=new ModelAndView();
        mv.addObject("MyName",name);
        mv.addObject("MyAge",age);
        mv.setViewName("show");
        return mv;
    }
```
### 使用java对象作为控制器方法的形参
- 用户界面
```html
<p>使用java对象来接收请求参数</p>
    <form action="user/receiveObject.do" method="post">
        姓名: <input type="text" name="name"/> <br>
        年龄: <input type="text" name="age"><br>
        <input type="submit" value="提交">
    </form>
```
- 控制器方法
  - **当控制器中有多个对象时，且两个对象的属性名都和形参名相同的话，那么框架会给它们对应的属性都附上值**
```java
/**
     * 处理器方法形参是java对象，这个对象的属性名和请求中参数名一样的
     * 框架会创建形参的java对象，给属性赋值。请求中的参数是name，框架会调用setName()
     * @return
     */
    @RequestMapping(value="/receiveObject.do")
    public ModelAndView doReceiveObject(Student myStudent, Teacher teacher){
        ModelAndView mv=new ModelAndView();
        mv.addObject("MyName",myStudent.getName());
        mv.addObject("MyAge",myStudent.getAge());
        mv.addObject("MyStduent",myStudent);
        System.out.println("teacher age="+teacher.getAge()); //teacher age=123
        System.out.println("teacher name="+teacher.getName());//teacher name=林帅
        mv.setViewName("show");
        return mv;
    }
```
### 控制器方法的返回值类型的含义
- ModelAndView
  - Model代表数据处理，View代表视图处理，当一个方法既要处理数据也要进行视图操作(转发)
- String
  - 只进行视图操作,表示视图，可以逻辑名称，也可以是完整视图路径
- void: 不能表示数据，也不能表示视图。
  - 在处理ajax项目的时候，可以通过HttpServletResponse来输出数据，响应
ajax。ajax请求服务端返回的就是数据，和视图无关。
- Object: 例如String,Integet,Map,List,Student等等都是对象，对象有属性，属性就是数据。所以返回Object表示数据，和视图无关。可以使用对象表示的数据，响应ajax请求
  - 现在做ajax,主要使用的是json的数据格式:实现步骤
     1. 加入处理json的工具库的依赖，springmvc默认使用的是jackson
     2. 在springmvc配置文件中加入<<mvc:annotation-driven>>注解驱动，这相当于是完成了``` json=om.writeValueAsString(student)```
     3. 在处理器方法的上面加入@ResponseBody注解，相当于是``` resonse.setContentType("application/json;charset=utf-8");````PrintWriter pw=response.getWriter();pw.print(json);```
- springmvc处理器方法返回Object，可以转为json输出到浏览器，响应ajax的内部原理
  - <mvc:annotation-driven>注解驱动
    注解驱动实现的动功能是完成java对象到json，xml，test，二进制等数据格式的转换，HttpMessageConveter接口:消息转换器
    功能:定义了java转为json，xml等数据格式的方法，这个接口有很多的实现类，这些实现类分别完成了java对象到json，xml，二进制等数据的转换
    

#### 当返回值是String类型时
- 控制器方法
```java
/**
     * 处理器方法返回String--表示逻辑视图名称，需要配置视图解析器,
     * 如果不写视图解析器就返回视图的完整路径/WEB-INF/view/show.jsp
     */
    @RequestMapping(value="/returnString.do")
    public String doReturnView(HttpServletRequest request, String name, Integer age){
        System.out.println("doReturnView,name="+name+" age="+age);
        //可以自己手动进行数据处理
        request.setAttribute("MyName",name);
        request.setAttribute("MyAge",age);
        //show : 逻辑视图名称，项目中配置了视图解析器,进行forward转发。
        return "show";
        //不写视图解析器的话
        //return "/WEB-INF/view/show.jsp";
    }
```
### 当返回值是void类型的时候
用方法处理ajax请求
- 准备工作
  1. 导入jquery文件
  2. 导入到jackson依赖
#### 用户界面
```jsp
<%--
  Created by IntelliJ IDEA.
  User: Lenovo
  Date: 2021/3/30
  Time: 13:06
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>SpringMVC项目</title>
    <script src="js/jquery-3.4.1.js"></script>
    <script type="text/javascript">
        $(function(){
            $("#mybutton").click(function (){
                $.ajax({
                    url:"user/returnVoid-ajax.do",
                    data:{
                        name:"zhangsan",
                        age:20
                    },
                    type:"post",
                    //jquery中ajax的dataType属性用于指定服务器返回的数据类型，如果不指定，
                    // jQuery 将自动根据HTTP包MIME信息来智能判断，如果datatype选项不填写的话，
                    // 会将返回的数据当成字符串处理。

                    //contentType表示服务器发送的数据的请求的类型
                    //例如: contentType="applicaton/json"
                    dataType:"json",
                    success:function (response){
                        //从服务器返回的是json格式的字符串
                        // jquery会把字符串转为json队形，赋值给response形参
                        alert(response.name)
                    }
                })
            })
        })
    </script>
</head>
<body>
    <p>处理器方法返回String表示视图名称</p>
    <form action="user/returnString.do" method="post">
        姓名: <input type="text" name="name"/> <br>
        年龄: <input type="text" name="age"><br>
        <input type="submit" value="提交"><br>
        <input type="button" value="发送ajax请求" id="mybutton">
    </form>
</body>
</html>
```
#### 控制器方法
```java
//使用ajax的方式发送请求
    //手工实现ajax，json数据:代码有重复的 1，java对象转为json 2.通过HttpServletResponse转为json对象
    @RequestMapping(value="/returnVoid-ajax.do")
    public void doReturnView(String name, Integer age,HttpServletResponse response) throws IOException {
       //使用Student对象将传入的结果封装
        Student student=new Student();
        student.setName(name);
        student.setAge(age);
        String json="";
        ObjectMapper om=new ObjectMapper();
            //将结果对象转为json格式的数据
            json=om.writeValueAsString(student);
            response.setContentType("application/json;charset=utf-8");
            //输出数据，响应ajax的请求
            PrintWriter pw=response.getWriter();
            pw.print(json);
            pw.close();
    }
```








