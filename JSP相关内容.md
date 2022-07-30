# JSP相关内容

#### 1.为什么学习JSP

学习JSP是javaServer pages 是为了做动态网页。在一个servlet中频繁的输出HTML代码，使得程序的耦合度提高了

违背了OCP(开发-封闭原则)开发原则。学习JSP在一定程度上也是为了降低程序的耦合度，JSP是javaEE规范之一

1. JSP文件的存放位置？

   1. jsp可以放到WEB-INF目录外，目前我们是这样做的
   2. 在实际开发中，有很多项目是将JSP放到WEB-INF目录中，为了保护JSP
   3. WEB-INF目录的数据是相对安全的

2. **jsp文件后缀？**

   1. 默认是.jsp
   2. 但是jsp文件的后缀也可以修改的，通过修改Tomcat的CATALINA_HOME/conf/web.xml文件![修改jsp后缀.png](https://i.loli.net/2020/11/18/wstK9EcqnMIJGWO.png)
   3. 默认是.jsp和.jspx文件后缀

3. **js和jsp的区别?**

   1. js:JavaScript，运行在浏览器中，和服务器没有关系
   2. jsp:JavaServer Pages,运行在服务器中，JSP底层就是java程序，运行在JVM中。

4. **JSP的执行原理？**

   1. 浏览器上访问的路径虽然是以.jsp文件结尾，访问的是某个jsp文件，实际上底层访问的是jsp对应的java程序

   2. Tomcat服务器负责将.jsp文件翻译成.java源文件，并且将java源文件编译生成.class字节码文件

   3. 访问jsp，其实底层还是执行了.class文件中的程序

   4. Tomcat服务器内置了JSP翻译引擎，撰文负责翻译JSP文件，编译java源文件

   5. index.jsp会被翻译生成index_jsp.java,编译生成了index_jsp.class

   6. index_jsp这个类继承了HttpJspBase，而HttpJspBase继承了HttpServlet

   7. 所以jsp就是Servlet，只不过职责不同 ，jsp的强项是页面展示

   8. 编译生成的.java和.class文件的存放位置

      ![jsp文件存放路径.png](https://i.loli.net/2020/11/18/NLoWvCcZ9iu3bVB.png)

5. 当你在JSP文件中编写了一段HTML代码的时候，在其对应的java文件中会输出以下内容

   ![网页信息.png](https://i.loli.net/2020/11/18/9uWtzPTHhDm1Yys.png)

   6.在JSP文件中编写的所有HTML，CSS，JavaScript，对于JSP来说，只是普通的字符串，被翻译到out.write("翻译到此处")；

   7.jsp文件修改之后，不需要重新部署，也不需要重启Tomcat

   8.jsp第一次访问为啥慢？

   ​	1.启动JSP翻译引擎

   ​	2.需要一个翻译的过程

   ​	3.需要Servlet对象的创建过程

   ​	4.init方法的调用

   ​	5.service方法的调用。。。。。。

   9.为啥第n+次访问JSP的时候非常的快

   	- 不需要重新翻译
   	- 不需要重新编译
   	- 不需要创建Servlet对象直接调用Servlet方法
   	- JSP也是单实例多线程环境运行的一个Servlet对象

   10.jsp文件在什么时候会被重新的翻译

   		- jsp文件被修改之后会被重新翻译
   		- 怎么确定jsp文件修改了？Tomcat服务器会记录jsp文件最后修改时间

   ## JSP小脚本

   ```java
    1.JSP的专业注释，这种方式不会被翻译到java源文件中 <%-- --%>
           2.在JSP文件中所编写的hmtl，css，javascript都会被自动翻译到Servlet的service方法中的out.write("翻译内容");
           3.关于JSP的小脚本scriptlet:
           <%
               java语句;
               java语句;
               java语句;
               java语句;
           %>
           4.小脚本中的java语句被翻译到Servlet的service方法中，所以脚本中必须编写java语句
           5.所谓的JSP规范就是SUN制定好的一些翻译规则，按照翻译规则进行翻译，生成对应的java源程序。不同的web服务器，翻译的结果是完全相同的，因为这些服务器在翻译的时候都遵守JSP规范
           6.小脚本的数量随意，可以多个
           7.小脚本中编写的java程序出现在service方法中，service方法中有执行顺序，所以小脚本中的代码也有执行顺序
   ```

   #### jsp小脚本中编写代码的注意点

   ```java
   1.能够在<% %>中编写private String name;嘛?
   	答案是NO，service方法中定义的变量属于局部变量，这种定义方式应该是属于实例变量
   2.在小脚本中能定义方法嘛？
       答案是NO
   3.在小脚本中能编写静态代码块嘛？
       答案是NO,静态代码块是在类加载时间执行,应该写在方法外面
   ```

   ## JSP声明语法

   ```java
   <%--
       1.JSP的声明语法格式:
       <%!
           实例变量;
           静态变量;
           方法;
           静态代码块;
           构造方法;
       %>
       注意:声明块中的java程序会被JSP翻译引擎翻译到service方法之外，声明块中不能直接编写java语句，除非是变量的声明
   --%>
   ```

   ## JSP的九大内置对象

   ```java
    <%--
       关于JSP的九大内置对象
           1.什么是内置对象？
             可以直接在JSP文件中拿来使用的引用。
           2.九大内置对象都有哪些？
               内置对象名称                   完整类名
               -------------------------------------
               pageContext                   javax.serlvet.jsp.PageContext           页面范围在当前页面效
               request                       javax.servlet.http.HttpServletRequest   请求范围
               session                       javax.servlet.http.HttpSession          会话范围
               application                   javax.servlet.ServletContext            应用范围
   
               out                           javax.servlet.jsp.JspWriter
               response                      javax.servlet.http.HttpServeltResponse
   
               config                        javax.servlet.ServletConfig
   
               exception                     java.lang.Throwable
   
               page                          java.lang.Object [page=this;]
           3.以上内置对象只能在service方法中直接使用，在其他方法中无法直接使用，可以间接使用
       --%>
   ```

   #### 在网页中动态输出姓名的三种方式

   ```java
   <%
   	String username = "jack";
   	out.print("登录成功,欢迎"+username+"回来");
   %>
   <br>
   
   登录成功，欢迎
   <% out.print(username); %>
   回来
   
   <br>
   
   登录成功，欢迎
   <%=username%>
   回来
   
   <%--
   	<% out.print(); %>
   	等同于
   	<%= %>
   --%>
   表达式语法具有向网页输出动态信息的功能。
   ```

   ## 关于JSP中的指令

   ```java
   <%--
   	关于JSP的指令：
   		1、指令的作用，是指导JSP的翻译引擎如何翻译JSP代码。
   		2、JSP中共三个指令：
   			* page			页面指令
   			* include		包含指令
   			* taglib		标签库指令【以后讲】
   		3、指令的使用语法格式：
   			<%@指令名  属性名=属性值  属性名=属性值.....%>
   		3、关于JSP的page指令,page指令中常用的属性：
   			* contentType		设置JSP的响应内容类型，同时在响应的内容类型后面也可以指定响应的字符编码方式
   			* pageEncoding		设置JSP响应时的字符编码方式
   			
   			* import			组织导入
   			
   			* session			设置当前JSP页面中是否可以直接使用session内置对象
   			
   			* errorPage			错误页面
   			* isErrorPage		是否是错误页面
   			
   			* isELIgnored		是否忽略EL表达式【后期讲】
   --%>
   ```

   #### 	关于page指令中的import，session，contentType，pageEncoding相关解释

   ```java
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
       以上代码中的设置相当于在service方法中编写response.setContentType("text/html;charset=utf-8");
   
   pageEncoding 属性的作用是用来设置响应内容的编码方式:
   <%@ page contentType="text/html;" pageEncoding="utf-8" %>
    当响应给网页的类型都是text/html时，以上两种方式没有区别，但如果想响应其他的类型只能在contentType设置
       
       
       
   import 属性是用于导入包的路径
      <%Date d=new Date();%> 这行代码相当于在service方法中new了一个日期对象，但是会报错，因为找不到对应的类的路径
      <%@ page import="java.util.Date" %> 
       
   session属性是用于设置当前页面是否能使用session对象，如果不写默认是true，
    <%@ page  session="true" %> 在底层是如果有session对象会直接返回当前session对象，如果没有的话就会创建session对象
       <%@ page  session="false" %> 在底层根本就没有session对象
       <%
           //使用request的getSession()的原则：
           //若想Session域中存放数据，则使用getSession(true)，即getSession()
           //若想Session域中取数据，则使用getSession(false)
           HttpSession session= request.getSession(false);
       %>
   ```
   
   #### 关于page指令中 errorPage和isErrorPage属性解释

```java
<%@page contentType="text/html; charset=UTF-8" errorPage="/error.jsp"%> 
<%-- 
	关于page指令的errorPage属性：
		当前JSP页面处错之后，要跳转的页面路径，需要使用该属性指定。路径不用写项目名 
--%>

    
<%@page contentType="text/html; charset=UTF-8" isErrorPage="true"%>
<%--
	关于page指令中的isErrorPage属性：
		- isErrorPage = "false" 表示内置对象exception无法使用【缺省情况下是false】
		- isErrorPage = "true"  表示内置对象exception可以使用
 --%>
<%-- 使用内置对象exception打印异常堆栈追踪信息,方便管理员进行调试错误 --%>
<%-- exception引用指向了抛出的异常 --%>
<%
```

#### 关于include指令中的file属性

```java
<%-- 
			关于include指令：
				1、a.jsp可以将b.jsp包含进来，当然被包含的资源不一定是jsp，也可能是其它的网络资源
				2、include作用：
					在网页中有一些主体框架，例如：网页头、网页脚，这些都是固定不变的，
					我们可以将网页头、网页脚等固定不变的单独编写到某个JSP文件中，
					在需要页面使用include指令包含进来。
					优点：
						代码量少了
						便于维护【修改一个文件就可以作用于所有的页面】
				3、在一个jsp中可以使用多个include指令
				4、include实现原理：
					4.1 编译期包含
					4.2 a.jsp包含b.jsp，底层共生成一个java源文件，一个class字节码文件,翻译期包含/编译期包含/静态联编
				
				5、静态联编的时候，多个jsp中可以共享同一个局部变量。
				因为最终翻译之后service方法只有一个。
		 --%>
		<%@include file="/index2.jsp" %><%=i%>//file 属性就是将其他的jsp包括到同一个jsp当中 
```

#### 关于jsp当中的动作

```java
<%-- 
	关于JSP中的动作：
		语法格式：<jsp:动作名  属性名=属性值   属性名=属性值....></jsp:动作名>
--%>

%-- 转发是一次请求 --%>
<%--
<jsp:forward page="/index2.jsp"></jsp:forward>
 --%>
 
<%-- 以上JSP的动作可以编写为对应的java程序进行实现 --%>
<%--
<%
	request.getRequestDispatcher("/index2.jsp").forward(request,response);
%>
 --%>

<%-- 编写java程序完成重定向 --%>
<%
	response.sendRedirect(request.getContextPath() + "/index2.jsp");
%>

<%-- 
	JSP主要是完成页面的展示，最好在JSP文件中少的编写java源代码：
		以后会通过EL表达式 + JSTL标签库来代替JSP中的java源代码
		当然，使用某些动作也能代替java源代码
 --%>

```

##### include动作

```java
<%--
			关于JSP中的include动作：
				1、a.jsp包含b.jsp，底层会分别生成两个java源文件，两个class字节码文件
				2、编译阶段并没有包含，编译阶段是两个独立的class字节码文件，生成两个Servlet，两个独立的service方法
				3、使用include动作属于运行阶段包含， 实际上是在运行阶段a中的service方法调用了b中的service方法，达到了包含效果
				4、a.jsp包含b.jsp，若两个jsp文件中有重名的变量，只能使用动态包含。其余都可以使用静态包含。
				5、include动作完成的动态包含，被称为动态联编。
		--%>
		
		
	<jsp:include page="/b.jsp"></jsp:include>
```



## EL表达式

1.什么是EL表达式？

- EL，Expression Language，表达式语言，是一种在jsp页面中获取数据的简单方式，EL表达式的基本语法形式很简单:在JSP页面的任何静态部分均可通过${expression}的形式获取到指定表达式的值。不能用于输出数据
- EL表达式只能从四大域中获取内容
  -  pageContext                   javax.serlvet.jsp.PageContext               页面范围在当前页面效
     request                           javax.servlet.http.HttpServletRequest   请求范围
     session                           javax.servlet.http.HttpSession               会话范围
    application                      javax.servlet.ServletContext                 应用范围
- EL的四大内置对象？
  - ![image.png](https://i.loli.net/2020/12/06/jxH4dNqlDfmB3iZ.png)

```jsp
<%
String name="kyrie"
%>
name=${name}  //这样无法取出数据，因为name不在四大域中

 <%
    // 以下代码的作用域是从小到大，查找数据的顺序是从小到大依次寻找，若找到就返回不再往下继续找
      pageContext.setAttribute("name","pageContext");
      request.setAttribute("name","request");
      session.setAttribute("name","session");
      application.setAttribute("name","application");
  %>
  name=${name}  //这样会先从pageContext,request,session,application的顺序依次往下进行搜寻
  name=${requestScope.name} //从request域中查找数据
  name=${name}<br>
 
```

#### EL访问对象的属性

```jsp
  <%
//   创建stu对象，里面有name和age两个成员变量
      student stu =new student("lisi",123);
      pageContext.setAttribute("student",stu);
    %>
<%-- 访问对象的属性的两种方式--%>
    name=${student.age} <br>
    age=${student.name} <br>
    name=${student['name']} <br>
<%--访问null对象的属性时，不会出现空指针异常，只是不会显示而已--%>
    name=${student3.name} <br>
```

#### EL访问数组对象

```jsp
   <%
      String[] names={"123","345","34324"};
      student[] array=new student[3];
      array[0]=new student("lisi",14);
      array[1]=new student("kyrie",28);
      array[2]=new student("James",34);
      pageContext.setAttribute("arraynames",names);
      request.setAttribute("arraylist",array);
    %>
  name1=${arraynames[1]} <br>
<%--若访问的数组下标越界了，EL不会抛出越界异常--%>
  name5=${arraynames[19]} <br>
<%--访问数组中的指定成员的属性--%>
  student[1].name=${requestScope.arraylist[1].name}
```

#### EL访问list集合元素

```jsp
    <%
      List<String> list=new ArrayList<>();
      list.add("first");
      list.add("second");
      list.add("third");
      pageContext.setAttribute("list",list);
    %>
   list1=${list[0]}
<%--同样的这里也不会报下标越界异常--%>
    list100=${list[100]}
```

#### EL访问Map集合元素

```jsp
<%
    Map<String,Object> map =new HashMap<>();
    map.put("name","lisi");
    map.put("age",15);
    pageContext.setAttribute("Map",map);
  %>
<%--通过.的方式直接访问map中的key得到value--%>
  name=${Map.name} //lisi
  age=${Map.age}  //15
```

#### EL中 empty运算符

![image.png](https://i.loli.net/2020/12/07/jdt9JxvhleNU1KG.png)

#### EL中的11个内置对象

1. 除了pageContext之外，其他的10个内置对象都是Map类型

```html
 <form action="${pageContext.request.contextPath}/hello"> //pageContext用于动态的获取项目的根路径
   用户名:<input type="text" name="name"><br>
    密码:<input type="password" name="password">
    <input type="submit" value="提交">
  </form>
```

2.param对象的使用：

```jsp
<%--    获取客户端发来的用户名--%>
    name=${param.name}<br>
<%--    获取客户端发来的密码--%>
    password=${param.password}<br>
相当于是request.getParameter("password");

```

3.paramValues对象的使用:

```jsp
 兴趣: <input type="checkbox" name="interest" value="basketball">篮球
            <input type="checkbox" name="interest" value="soccerball">足球
            <input type="checkbox" name="interest" value="volleyball">排球
<%-- 用于获取客户端发来的数据，比如兴趣复选框，就会有多个值，是个String数组--%>
    interest=${paramValues.interest} //返回的是一个String数组
	interest[0]=${paramValues.interest[0]} //返回的是basketball
```

4.initParam对象的用法

```jsp
//在web.xml文件中配置初始化参数
<context-param>
        <param-name>place</param-name>
        <param-value>都江堰</param-value>
</context-param>
//通过initParam获取value的值
place=${initParam.place}
```

5.如何自定义EL函数

```xml
//自定义函数
//该类及其函数，需要在一个扩展名为.tld的XML文件中进行注册，
//tld，tag library definition,标签库定义
// tld文件需要添加一定的约束，约束在以下文件拷贝
// 文件路径 D:\TomcatDowload\apache-tomcat-9.0.30-windows-x64\apache-tomcat-9.0.30\webapps\examples\WEB-INF\jsp2
// 文件名：jsp2-example-taglib.tld
1.首先先自己定义一个类，类里面定义一个方法
public class ELfunction {
    public static String lowerToUper(String source){
        return source.toUpperCase();
    }
}
2.注册函数:在web项目下的/WEB_INF目录下，新建一个扩展名为.tld的XML文件

3.添加约束
    <!--    定义标签库的信息-->
    <tlib-version>1.0</tlib-version> <!--指定当前函数库版本号-->
    <short-name>myfunction</short-name> <!--指定函数库的名称，一般一个函数库一个名称，这个名称在jsp文件中要使用-->
    <uri>http://mycompany.com</uri>  <!--指定该函数库所对应的url，即一个tld文件一个url，后面jsp文件会使用-->
<!--    注册函数-->

4.注册函数
<function>   
    <name>MylowerToUper</name>   <!--    自定义函数的名字-->
    <function-class>com.ELfunction</function-class>   <!--    该函数所属类的完整路径-->
    <function-signature>java.lang.String lowerToUper(java.lang.String)</function-signature> <!--    函数返回值类型，函数名，函数参数类型 类型都是些完整类路径-->
</function>


5.使用函数
<%@taglib uri="http://mycompany.com" prefix="myfunction" %>
    
6.调用函数
 String = ${myfunction:MylowerToUper("adSDfdasdfADSAS")}
```

#### JSTL调用封装好的函数

1.在web/WEB-INF/lib下添加相关的JSTL架包

![image.png](https://i.loli.net/2020/12/08/5KIXcWelVbHyZg7.png)

2.在standard.jar下面的META-INF下面的fn.tld文件，里面封装了16个函数

![image.png](https://i.loli.net/2020/12/08/3FnbgwMVLZTHcx7.png)

3.如何使用自定义的函数

```jsp
<%@taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>

flag=${fn:contains("abcdefdsa","abc")}  //这个函数的作用是判断abc是否在abcdefdsa中，是则返回true，否则返回false
```



4.如何自定义不带标签体的标签

```java
//需求:获取客户端的ip地址
//1.定义一个类继承SimpleTagSupport这个类，这个类的父类是接口SimpleTag,调用标签的时候会执行doTag()方法，所以要重写doTag()方法
//2.思路是先得到request对象,其次通过request.getRemoteAddr()
//3.得到request对象需要PageContext，PageContext是JspContext的子类。
//4.在调用了这个方法后需要将ip显示到浏览器上就需要输出流
public class tags extends SimpleTagSupport  {
    @Override
    // 定义标签处理器，获取客户端ip
    public void doTag() throws JspException, IOException {
          //获取PageContext对象
        PageContext pc= (PageContext) this.getJspContext(); //返回的是JspContext需要向下转型为PageContext
        ServletRequest request= pc.getRequest();
        String ip=request.getRemoteAddr();
        JspWriter jw=pc.getOut();
        jw.print(ip);
    }
}



//然后是在WEB—INF下面创建一个tld文件，对标签进行注册，类似于函数
 <tlib-version>1.0</tlib-version> 
    <short-name>tags</short-name>
    <uri>http://mycompany.com/tags</uri>
    <tag>
        <name>getIp</name>
        <tag-class>com.tags</tag-class>
        <body-content>empty</body-content> //目前设置标签体为空
    </tag>

            
// 如何使用自定义标签
   <%@taglib prefix="getIp" uri="http://mycompany.com/tags" %>
   <getIp:getIp/>
```



5.定义带有标签体的标签

```java
//需求:将传入的字符串从小写转换为大写

//1.定义一个继承SimpleTagSupport的类，然后重写doTag方法，
//2.首先要获取到标签体的内容
	JspFragment jf=this.getJspBody();
//3.然后需要将标签的内容通过输出流输出
	jf.invoke(输出流) 
//4.获取能转化成字符串的输出流
	StringWrite sw=new StringWriter();
	将sw放入上一步的输出流，此时的输出流中的内容就是jf
//5.将内容转化为字符串，通过调用字符串的toUpperCase方法来达到变成大写的目的
	 String test=sw.toString();
//6.将转换后的内容通过输出流输出到浏览器上
  this.getJspContext().getOut().print(upperString);
public class tag extends SimpleTagSupport {
    @Override
    public void doTag() throws JspException, IOException {
        //创建一个具有缓存功能的输出流
        StringWriter sw=new StringWriter();
        //获取jsp片段，也就是标签体的内容
        JspFragment jf=this.getJspBody();
        //将获取到的jsp内容，放在字符串输出流中
        jf.invoke(sw);
        //调用输出流的toString方法就能转换成字符串
       String test=sw.toString();
       //通过字符串的toUpperCase方法将字符串转换为大写
       String upperString=test.toUpperCase();
       this.getJspContext().getOut().print(upperString);
    }
}



在WEN-INF下面创建一个tld文件
     <tlib-version>1.0</tlib-version>
    <short-name>ut</short-name>
    <uri>http://mycompany.com</uri>
    <tag>
        <name>lowerToUpper</name>
        <tag-class>com.Tags.tag</tag-class>
        <body-content>scriptless</body-content>
    </tag>
            
           
            
            
//关于<body-content>标签参数的相关解释
//1.empty:表示自定义的标签没有标签体
//2.scriptless:表示自定义的标签有标签体，但标签体文本中不能包含java小脚本，即不能包含java代码块与java表达式，但可以包含EL表达式，且会对EL表达式进行计算
//3.JSP:在JSP2.0之前，定义标签处理器类需要实现Tag接口，或继承自TagSupport类。那时候该属性值有用，表示会将标签体的文本内容原样显示在浏览器。该属性值对于继承自SimpleTagSupport类的标签处理器是不能使用的。使用会报错。
//4.tagdependent:表示会将标签体的文本内容原样显示在浏览器。若其中包含EL表达式，也会将其作为普通字符串，而不对其进行计算。    

//调用方式            
<%@taglib prefix="ut" uri="http://mycompany.com" %>            
 <%
   String name="kyrieIrving";
   pageContext.setAttribute("name",name);
 %>
  <ut:lowerToUpper>nihao</ut:lowerToUpper> <br>
  <ut:lowerToUpper>${name}</ut:lowerToUpper>          
```

6.定义带有属性值的标签

```java
需求：在标签中传入一个布尔值，如果为真则输出标签体内容，否则不输出
1.定义标签处理器
2.标签处理器中声明具有set方法的属性，其属性名即为标签的属性名
3.注册标签处理器   
public class tag extends SimpleTagSupport {
    private boolean flag;
    public void setFlag(boolean flag) {
        this.flag = flag;
    }
    @Override
    public void doTag() throws JspException, IOException {
        JspFragment jf=this.getJspBody();
        if(flag){ 
            jf.invoke(this.getJspContext().getOut());
        }
    }
}


    <tlib-version>1.0</tlib-version>
    <short-name>myshortname</short-name>
    <uri>http://mycompany.com</uri>
    <tag>
        <name>function</name>
        <tag-class>com.Tags.tag</tag-class>
        <body-content>scriptless</body-content>
        <attribute>
<!--指定属性名，要保证与标签处理器的属性名相同-->
            <name>flag</name>
<!--指定该属性标签是否是必须的，为true则是必须的，为false则表示可有可无-->
            <required>true</required>
<!--指定该属性值是否可以来自运行时表达式的值，为true表示属性值可以来自表示，false表示该值只能是常量，这里的表达式
指的是EL表达式与JSP中的表达式 Runtime Expression Value的简写-->
            <rtexprvalue>true</rtexprvalue>
        </attribute>
    </tag>
            
            
如何调用
  <%
    boolean isTrue=true;
    pageContext.setAttribute("isTrue",isTrue);
  %>
  <function:function flag="${isTrue}">男生</function:function>
  <function:function flag="<%=isTrue%>">女生</function:function>            
```

7.自定义标签ForEach进行遍历

```java
需求:将list集合中的元素遍历出现，显示在浏览器上
 <%
    List<String> list=new ArrayList<>();
    list.add("James");
    list.add("KyrieIrving");
    list.add("Curry");
    pageContext.setAttribute("list",list);
  %>
  <fun:forEach items="${list}" now="name">  item表示当前集合，now表示当前被遍历的元素
    ${name} <br>
  </fun:forEach>
  
  
 //定义标签处理器
  public class Tag extends SimpleTagSupport {
    private List items;
    private String now;

    public void setItems(List items) {
        this.items = items;
    }

    public void setNow(String now) {
        this.now = now;
    }

    @Override
    public void doTag() throws JspException, IOException {
        for(Object om:items){
            this.getJspContext().setAttribute(now,om);
            //如果没有上面将name添加到jspcontext域中，将无法遍历到元素，now属性就是用来将当前对象添加到
            //jspcontext域中，这样才能通过EL表达式访问到
            this.getJspBody().invoke(null); // 输出流为null的时候默认是jspwriter
        }
    }
}

```

如何使用自己打包好的标签

首先参考oneNote上如何打包，然后将原来的tld文件添加到WEB-INF下面，forEach就是自己写好的jar，list.tld文件是原来的项目的

![image.png](https://i.loli.net/2020/12/12/28AICjPNLzWFDZa.png)



使用JSTL标准库中c:set标签

```jsp
1.将jstl.jar和standard.jar引入到WEB-INF下面的lib里面
2.引入对应的标签库
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
3.使用set标签
	 1.c:set 将变量存放到指定域属性空间;为Bean的属性赋值；设置Map的key和value等。改标签在实际开发中并不常用 <br>
<%--value:变量的值--%>
<%--var:变量名--%>
<%--scope:将变量存放的域属性空间，取值为page，request，session，application。默认为page范围--%>
  <c:set value="hello" var="flag" scope="page" />  注意这个是单标签
  相当于: pageContext.setAttribute("flag","hello")
    flag=${pageScope.flag}

  2.c:set 为Bean的属性赋值
  <%
    teacher t=new teacher();
    session.setAttribute("teacher",t);
  %>
<%--  value:变量的值--%>
<%--  property:对象的属性的名称--%>
<%--  target:指定那个域的那个对象--%>
  <c:set value="15" property="age" target="${sessionScope.teacher}"></c:set>
  <c:set value="苍老师" property="name" target="${sessionScope.teacher}"></c:set>
  teacher= ${sessionScope.teacher}

<%--   c:set 为Map赋值--%>
  <%
    Map<String,Object> map =new HashMap<>();
    pageContext.setAttribute("map",map);
  %>
  value:属性值
  property:指定对象的指定属性，这里指的是map的key
  target:指定在哪个域寻找map
    <c:set value="lisi" property="name" target="${pageScope.map}"></c:set>
  <c:set value="15" property="age" target="${pageScope.map}"></c:set>
  name=${map.name} <br> //通过map.属性的方式来访问value  name=lisi
  age=${map.age} <br> //age=15
```

c:catch标签和c:remove标签

```jsp
 <br>------------c:remove标签----------<br>
  <c:set value="lisi" var="name" scope="page"></c:set>
  <c:set value="lisi" var="name" scope="session"></c:set>
  <c:set value="lisi" var="name" scope="request"></c:set>
  <c:set value="lisi" var="name" scope="application"></c:set>
<%--  删除application域中的属性--%>
  <c:remove var="name" scope="application"></c:remove>
<%--  如果不指定域，则会删除所有域中的name属性--%>
  <c:remove var="name"></c:remove>
  page_name=${pageScope.name} <br>
  session_name=${sessionScope.name} <br>
  request_name=${requestScope.name} <br>
  application_name=${applicationScope.name} <br>


  <br>------------c:catch标签----------<br>
	如果catch标签体中发生了异常，异常信息会存放在exception中
  <c:catch var="exception">
   <%
    int a = 3/0;
   %>
  </c:catch>
  exception=${exception.message};
```

c:out标签的用法

```jsp
  <br>------------c:out标签----------<br>
<%--  不写target属性默认是page域--%>
  <c:set var="City" value="成都"></c:set>
<%--  存放在page域，却从session域中找，找不到就用默认值default，使用default的条件是未定义，而不是为空--%>
 city=<c:out value="${sessionScope.City}" default="中国"></c:out>

  <c:set var="title" value="<h1>HelloWorld</h1>"></c:set>
<%--  通过out输出的value是原样输出，不会解析成html标签，因为默认是忽略html的，要想解析成html的话加上escapeXml属性--%>
  c:out= <c:out value="${title}" escapeXml="false"></c:out> <br>
  EL表达式:${title}
```

c:if标签和c:choose标签

```jsp
<br>------------c:if标签----------<br>
  <c:set var="username" value="kyrie" scope="session"></c:set>
<%--  if标签test中判断，如果为真的话执行标签体，否则不执行--%>
  <c:if test="${username=='kyrie'}">欢迎用户登录</c:if>

  <br>------------c:choose标签----------<br>
  <c:set var="pagenumber" value="1"></c:set>
  <c:set var="totalpage" value="5"></c:set>
<%--  choose标签中只能包含when和otherwise标签,类似于switch case 匹配上就不再往下匹配执行--%>
  <c:choose>
      <c:when test="${pagenumber==1}">
          首页 上一页 <a href="#">下一页</a> <a href="#">尾页</a> 当前页${pagenumber}/${totalpage}
      </c:when>
      <c:when test="${pagenumber==totalpage}">
          <a href="#"> 首页</a> <a href="#">上一页</a> 下一页 尾页 当前页${pagenumber}/${totalpage}
      </c:when>
      <c:otherwise>
          <a href="#"> 首页</a> <a href="#">上一页</a> <a href="#">下一页 </a><a href="#">尾页</a> 当前页${pagenumber}/${totalpage}
      </c:otherwise>
  </c:choose>
```

c:forEach标签

```jsp
<br>------------c:forEach标签----------<br>
        <%
            List<String> L=new ArrayList();
            L.add("张三");
            L.add("李四");
            L.add("王五");
            L.add("刘备");
            L.add("关于");
            L.add("张飞");
//            pageContext.setAttribute("list",L); 可以直接将list添加到pageContext当中，也可以通过set标签
        %>
      <c:set var="list" value="<%=L%>"></c:set>
<%--    遍历list，从下标1开始，5结束，步长为2--%>
      <c:forEach var="name" items="${list}" begin="1" end="5" step="2">
          ${name}
      </c:forEach>

      <c:forEach begin="1" end="5" var="num"> 遍历输出1到5
          ${num}
      </c:forEach>

    <%
        List<teacher> list=new ArrayList();
        list.add(new teacher(23,"马老师"));
        list.add(new teacher(24,"李老师"));
        list.add(new teacher(25,"王老师"));
        list.add(new teacher(26,"张老师"));
        list.add(new teacher(27,"林老师"));
        list.add(new teacher(28,"秦老师"));
    %>
将list集合存入到pageContext中，不写scope默认是存放在pageContext域中
  <c:set var="teachers" value="<%=list%>"></c:set>
  <table border="1px">
      <tr>
          <th>编号</th>
          <th>姓名</th>
          <th>年龄</th>
      </tr>
      <c:forEach var="teacher" items="${teachers}" varStatus="now"> varStatus表示的是当前对象的状态
          <tr class="${now.count%2==0? 'first':'second'}">
              <td>${now.count}</td> ccount属性表示当前对象的编号，从1开始
              <td>${teacher.name}</td>
              <td>${teacher.age}</td>
          </tr>
      </c:forEach>
  </table>
```

使用格式化标签库里的标签

fmt:formatDate 和fmt:parseDate

```jsp
引入对应的标签库
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<br>-------------fmt:formatDate-------<br>
  <%
    Date d=new Date();
    pageContext.setAttribute("Date",d);
  %>
<%--  value:需要格式化的日期对象--%>
<%--  pattern:指定格式化的格式--%>
  now=<fmt:formatDate value="${Date}" pattern="yyyy-MM-dd:HH-mm-ss"></fmt:formatDate> <br>
<%--  当添加了var属性后，表示将格式化好的日期存放在四大域中，不指名默认是page域，并且不会在浏览器上输出--%>
now=<fmt:formatDate value="${Date}" pattern="yyyy-MM-dd:HH-mm-ss" var="birthday"></fmt:formatDate><br>
  <input type="text" value="${birthday}">

 <br>-------------fmt:parseDate-------<br>
<%--将字符串转化成日期对象，输出在浏览器上，如果加上var和formatDate标签类似，也不会在浏览器上输出，而是添加到指定的域中，实际用到的很少，了解--%>
  <fmt:parseDate value="1949-10-01" pattern="yyyy-MM-dd"></fmt:parseDate>
```

