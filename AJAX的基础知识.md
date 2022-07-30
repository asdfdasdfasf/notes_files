

# AJAX的基础知识

1. 全局刷新和局部刷新
   1. 全局刷新：整个浏览器被新的数据覆盖。 在网络中传输大量的数据。 浏览器需要加载，渲染页面。
   2. 局部刷新：在浏览器器的内部，发起请求，获取数据，改变页面中的部分内容。
      其余的页面无需加载和渲染。 网络中数据传输量少， 给用户的感受好。
2. AJAX的作用
   1.  ajax是用来做局部刷新的。局部刷新使用的核心对象是 异步对象（XMLHttpRequest）
       这个异步对象是存在浏览器内存中的 ，使用javascript语法创建和使用XMLHttpRequest对象。
3. ajax：Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）
   1. Asynchronous:异步的意思
   2. JavaScript:javascript脚本，在浏览器中执行
   3. XML:一种数据格式
4. ajax是一种做局部刷新的新方法（2003左右），不是一种语言。 ajax包含的技术主要有javascript,
    dom,css, xml等等。 核心是javascript 和 xml 。
    javascript：负责创建异步对象， 发送请求， 更新页面的dom对象。 ajax请求需要服务器端的数据。
    xml： 网络中的传输的数据格式。 使用json替换了xml 。

### ajax中使用XMLHttpRequest对象

1. 创建异步对象:

   ```javascript
   1. var xmlHttp=new XMLHttpRequest();
   ```

2.给异步对象绑定事件onreadystatechange:当异步对象发起请求，获取了数据就会触发这个事件，这个事件需要指定一个函数，在函数中处理状态的变化。

```javascript
btn.onclick=fun1()
function fun1(){
    alert("按钮按下");
}
//在fun1这个函数中监测处理状态的变化
xmlHttp.onreadystatechange=function(){
    if(xmlHttp.readyState==4&&xmlHttp.status==200){//当状态值为200且状态变化值为4时说明服务器正常返回数据
       var data=xmlHttp.responseText;//获取浏览器发回的文本数据
        //然后进行相关的更新页面的操作
    }
}
```

3.异步对象的属性readyState和status属性值

```javascript
0：创建异步对象时， new XMLHttpRequest();
1: 初始异步请求对象， xmlHttp.open()
2：发送请求， xmlHttp.send()
3: 从服务器端获取了数据，此时3， 注意3是异步对象内部使用， 获取了原始的数据。
4：异步对象把接收的数据处理完成后。 此时开发人员在4的时候处理数据。
	    在4的时候，开发人员做什么 ？  更新当前页面。
        
status属性表示网络请求的状态：
	200:表示服务器正常返回数据
    404:未找到相关资源
    500:服务器端出现了相关的错误
```

4.初始异步对象

```javascript
 异步的方法open().
	  xmlHttp.open(请求方式get|post, "服务器端的访问地址", 同步|异步请求（默认是true，异步请求）)
	  例如：
	  xmlHttp.open("get", "loginServlet?name=zs&pwd=123",true);//路径不需要加/
```

5.使用异步对象发送数据

```
xmlHttp.send()
```

6.回调概念：

​	当请求的属性readState值发生变化的时候，此时就会触发onreadystatechange事件，调用相关的函数

### JSON使用

1.ajax发起请求，服务器返回一个json格式的字符串{ name:"河北", jiancheng:"冀","shenghui":"石家庄"}，用过key:value的形式存在。

2.json数组JSONArray,基本格式:

```json
[{ name:"河北", jiancheng:"冀","shenghui":"石家庄"} , { name:"山西", jiancheng:"晋","shenghui":"太原"} ]
```

3. 为什么要使用json ：

   1. json格式数据在多种语言中，比较容易处理。 使用java， javascript读写json格式的数据比较容易。
   2. json格式数据他占用的空间下，在网络中传输快， 用户的体验好。

4. 在servlet中如何封装json数据

   ```java
   //将获取到的数据都封装到p这个对象中
   while(rs.next()){
                   p=new Province();
                   p.setId(Integer.valueOf(id));
                   p.setName(rs.getString("name"));
                   p.setJiancheng(rs.getString("jiancheng"));
                   p.setShenghui(rs.getString("shenghui"));
                   ObjectMapper om=new ObjectMapper();
                   json=om.writeValueAsString(p);//将对象p转化成json格式的字符串
               }
   		   response.setContentType("application/json;charset=utf-8");//设置浏览器接受数据的方式，这行代码表明接收的是json数据
               PrintWriter pw= response.getWriter();
               pw.print(json);
   ```

5.在ajax中如何解析servlet发送过来的json数据

​	

```javascript
if(xmlhttp.readyState==4&&xmlhttp.status==200){
            var data=xmlhttp.responseText;//获取servlet发送过来的json字符串
            var jsondata=eval("("+data+")");//通过eval函数解析成json数据
           //{ name:"河北", jiancheng:"冀","shenghui":"石家庄"}
            document.getElementById("name").value=jsondata.name;//通过key(name)，将value(河北)更新到相关的标签上
            document.getElementById("jiancheng").value=jsondata.jiancheng;
            document.getElementById("shenghui").value=jsondata.shenghui;
          }
```

6.谈论xmlHttp.open()方法第三个参数，当为true时表示异步

​	1.当为true时，如果ajax向服务器发送请求，在正常情况下需要服务器返回资源后，以下的代码才会继续执行，如果设置为true的话，两者可同时进行，你发送你的请求，我继续往下执行，类似于并发。

​	2.当设置为false时，就是同步，需要等待服务器将相关的资源进行返回后，才能够继续的往下执行相关的代码。

7.在java中使用json需要用到的jar。

![image.png](https://i.loli.net/2020/11/25/n9ZfpLSzGJ2uYC3.png)