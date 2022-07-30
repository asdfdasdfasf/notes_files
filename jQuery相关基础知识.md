# jQuery 相关基础知识

1. jQuery 的相关总结

   1. 什么是 jQuery：一个 javascript 的库，里面有很多的函数。
   2. 作用：可以操作 dom 对象，事件处理，动画，ajax
   3. jQuery 的优点：1.免费，开源 2.比较小巧，压缩版本只有 87k，3.兼容大多数的浏览器 4.文档齐全，方便进行查询
   4. 对象分类：
      1. dom 对象：使用 js 语法创建的对象是 dom 对象，也是 js 对象，dom 对象可以调用 dom 的方法和属性
      2. jQuery 对象：使用 jQuery 语法创建的对象能调用 jQuery 函数库中的函数或者属性，jquery 对象是数组，数组中的每个成员都是 dom 对象
   5. jquery 中的选择器：
      1. 选择器就是一个字符串，用于定位 dom 对象，就是选择页面中的 dom 对象的条件
      2. id 选择器：$("#id 的值")
      3. class 选择器: $(".class 的值")
      4. 标签选择器：$("标签")
      5. 所有选择器:$("\*")
      6. 组合选择器：$("id 选择器，class 选择器，标签选择器")

2. dom 对象和 jQuery 对象啊

   1. var obj=document.getElementByid("txt1"); obj 是 dom 对象，也叫做 js 对象。obj.value
   2. **var jobj=$("#txt1"),jobj 就是使用 jQuery 语法表示的对象，也就是 jquery 对象，它是一个数组。而现在数组中就一个数**

3. dom 对象和 jquery 对象的互换
   1. dom 对象转化为 jquery 对象：$(dom 对象)
   2. jquery 对象转化为 dom 对象：从数组中获取第一个对象，第一个对象就是 dom 对象，使用[0]或者 get(0)
```html
<script type="text/javascript">
        $(document).ready(function(){
           var $number=$("#first").val();//通过jquery对象将input标签的值进行读取
           alert($number);
           //将jquery对象转换为dom对象，jquery对象就是一个数组，只不过这个id是唯一的那么数组中就一个值
           alert("dom对象"+$("#first")[0].value);
        })
```

4. 为什么要进行 jquery 对象和 dom 对象的相互转换？

   1. 当是 dom 对象时，可以使用 dom 对象的属性或者方法，如果想要使用 jquery 提供的函数必须是 jquery 对象才可以。

5. 表单选择器：

   1. 使用<input>标签的 type 属性，定位 dom 对象的方式；

      1. 语法：$(":type 属性值")

      2. 例如：$(":text")，选择的就是所有的单行文本框，$(":button")，选择的就是所有的按钮

         ```javascript
         <!DOCTYPE html>
         <html lang="en">
         <head>
             <meta charset="UTF-8">
             <meta name="viewport" content="width=device-width, initial-scale=1.0">
             <title>通过选择器获取form表单属性</title>
             <script type="text/javascript" src="./js/jquery-3.4.1.js"></script>
             <script type="text/javascript">
                 function fun1(){
                    var obj=$(":text");
                    alert(obj.val());
                 }
                 function fun2(){
                     var obj=$(":radio");
                     alert(obj.val());
                     for(var i=0;i<obj.length;i++){
                         //通过dom对象获取radio的value值
                         //alert(obj[i].value);
      
                         //通过jquery对象获取radio的value值
                         alert($(obj[i]).val());
                     }
                 }
                 function fun3(){
                     var obj=$(":checkbox");
                     for(var i=0;i<obj.length;i++){
                         //通过dom对象获取checkbox的value值
                         //alert(obj[i].value);
      
                         //通过jquery对象获取checkbox的value值
                         alert($(obj[i]).val());
                     }
                 }
             </script>
         </head>
         <body>
             <form action="">
                 <input type="text" value="Hello text"><br>
                 <input type="radio" value="man" name="sex">男<br>
                 <input type="radio" value="woman" name="sex">女<br>
                 <input type="checkbox" value="basketball">篮球
                 <input type="checkbox" value="soccerball">足球
                 <input type="checkbox" value="volleyball">排球<br>
                 <input type="button" value="获取text的value" onclick="fun1()">
                 <input type="button" value="获取radio的value" onclick="fun2()">
                 <input type="button" value="获取checkbox的value" onclick="fun3()">
             </form>
         </body>
         </html>
         ```

6. 过滤器：在定位了 dom 对象后，根据一些条件进行 dom 对象的筛选

   1. 过滤器也是一个字符串，用于筛选 dom 对象的。

   2. 过滤器不能单独使用，必须和选择器一起使用

   3. $("选择器:first") : 第一个 dom 对象

   4. $("选择器:last"): 数组中的最后一个 dom 对象

   5. $("选择器:eq(数组的下标)") ：获取指定下标的 dom 对象

   6. $("选择器:lt(下标)") ： 获取小于下标的所有 dom 对象

   7. $("选择器:gt(下标)") ： 获取大于下标的所有 dom 对象

      ```javascript
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <meta name="viewport" content="width=device-width, initial-scale=1.0">
          <title>过滤选择器</title>
          <style type="text/css">
              div{
                  background:grey;
                  width: 100px;
                  height: 100px;
                  text-align: center;
              }
          </style>
          <script type="text/javascript" src="./js/jquery-3.4.1.js"></script>
          <script type="text/javascript">
              $(function(){
              	//给id为btn的按钮绑定click事件
                  $("#btn").click(function(){
                      //设置第一个div的背景颜色
                      $("div:first").css("background","yellow");
                  })
                  $("#btn1").click(function(){
                      //设置最后一个div的背景颜色
                      $("div:last").css("background","blue");
                  })
                  $("#btn2").click(function(){
                      //设置第三个div的背景颜色
                      $("div:eq(3)").css("background","green");
                  })
                  $("#btn3").click(function(){
                      //设置下标小于3的所有div的颜色(下标为0,1,2)
                      $("div:lt(3)").css("background","lightgreen");
                  })
                  $("#btn4").click(function(){
                      $("div:gt(2)").css("background","red");
                  })
              })
          </script>
      </head>
      <body>
          <div id="one">div-0</div>
          <div id="two">div-1</div>
          <div id="three">div-2</div>
          <div id="four">div-3</div>
          <div id="fifth">div-4</div>
          <input type="button" value="获取第一个div的内容" id="btn">
          <input type="button" value="获取最后一个div的内容" id="btn1">
          <input type="button" value="获取第3个div的内容" id="btn2">
          <input type="button" value="获取下标为3之前的div的内容" id="btn3">
          <input type="button" value="获取下标为2以后的div" id="btn4">
      </body>
      </html>
      ```

   ## 表单属性过滤器：根据表中 dom 对象的状态情况，定位 dom 对象

   1. 启动状态：enabled

   2. 不可选中状态：disabled

   3. 选择状态：checked，例如 radio，checkbox

   4. 被选中：selected 在下拉列表中被选中的一项

   5. 选择所有可用的文本框：$(":text:enabled")

   6. 选择不可用的文本框：$(":text:disabled")

   7. 复选框选中的元素：$(":checkbox:checked")

   8. 选择指定下拉列表的被选中元素：选择器>option:selected

      ```javascript
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <meta name="viewport" content="width=device-width, initial-scale=1.0">
          <title>form表单的属性选择器</title>
          <script src="./js/jquery-3.4.1.js"></script>
          <script>
              //如果想提前给按钮绑定事件，必须等所有的dom对象全部加载完毕，所有绑定事件必须写在
              //$(function(){})，或者$(document).ready(function(){})里面
              $(document).ready(function(){
                  $("#btn1").click(function(){
                      //获取type类型为text的所有能够修改的input标签
                      var temp=$(":text:enabled");
                      //遍历输出他们的value值
                      for(var i=0;temp.length;i++){
                          alert(temp[i].value);
                      }
                  })
                  $("#btn2").click(function(){
                      var array=$(":text:disabled");
                      for(var i=0;i<array.length;i++){
                          //获取到的每一个都是dom对象，将dom对象转化为jquery对象，
                          //调用val函数输出value属性的值
                          alert($(array[i]).val());
                      }
                  })
                  $("#btn3").click(function(){
                      var array=$(":checkbox:checked");
                      for(var i=0;i<array.length;i++){
                          alert(array[i].value);
                      }
                  })
                  $("#btn4").click(function(){
                      var number=$("select>option:selected");
                      alert(number.val());
                  })
      
              })
          </script>
      </head>
      <body>
          <input type="text" value="hello world" disabled><br>
          <input type="text" value="你好呀世界"><br>
          <input type="text" value="凯里欧文永远的神"><br>
          <input type="text" value="帅帅永远的神" disabled><br>
          <input type="checkbox" value="篮球" checked>篮球
          <input type="checkbox" value="足球">足球
          <input type="checkbox" value="台球">台球
          <select id="choose">
              <option value="Java">java</option>
              <option value="C++">C++</option>
              <option value="Python" selected>Python</option>
              <option value="PHP">PHP</option>
          </select><br>
          <input type="button" value="获取enable的文本框" id="btn1"><br>
          <input type="button" value="获取disabled的文本框" id="btn2"><br>
          <input type="button" value="获取被选中的复选框的值" id="btn3"><br>
          <input type="button" value="获取下拉列表中被选中的一项" id="btn4"><br>
      </body>
      </html>
      ```

---

# 函数

#### 第一组函数

1. val:操作数组中 DOM 对象的 value 属性 1. $(选择器).val():无参数调用形式，读取数组中第一个 DOM 对象的 value 属性值 2. $(选择器).valu(值):有参数形式调用；对数组中所有 DOM 对象的 value 属性值进行统一赋值
2. text：操作数组中所有 DOM 对象的【文字显示内容属性】 1. $(选择器).text():无参数调用，读取数组中所有 DOM 对象的文字显示内容，将得到内容拼接成一个字符串返回 2. $(选择器).text(值):有参数方式,对数组中所有 DOM 对象的文字显示内容进行统一赋值
3. attr:对 val，text 之外的其他属性操作 1. $(选择器).attr("属性值") 获取 DOM 数组第一个对象的属性值 2. $(选择器).attr("属性值","值") 对数组中所有 DOM 对象的属性设置为新值
```javascript
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Document</title>
       <script src="./js/jquery-3.4.1.js"></script>
       <script>
           $(function(){
               $("#first").click(function(){
                   alert($(":text").val());
               })
               $("#second").click(function(){
                   //设置所有的文本标签的值为王者荣耀
                   $(":text").val("王者荣耀");
               })
               $("#third").click(function(){
                   alert($("div").text());
               })
               $("#four").click(function(){
                   $("div").text("加油打工人!");
               })
               $("#five").click(function(){
                  var temp=$("img").attr("src");
                  alert(temp);
               })
               $("#six").click(function(){
                   $("img").attr("src","./img/ex4.jpg");
               })
           })
       </script>
   </head>
   <body>
       <input type="text" value="刘备"><br>
       <input type="text" value="关羽"><br>
       <input type="text" value="张飞"><br>
       <div>第一个div</div>
       <div>第二个div</div>
       <div>第三个div</div>
       <img src="./img/ex1.jpg"><br>
       <input type="button" value="获取input的value值" id="first"><br>
       <input type="button" value="设置所有的text的value值" id="second"><br>
       <input type="button" value="获取所有文本框的值" id="third"><br>
       <input type="button" value="修改所有文本框的值" id="four"><br>
       <input type="button" value="获取图片的路径" id="five"><br>
       <input type="button" value="设置图片的src属性值" id="six"><br>
   </body>
   </html>
```

## 第二组函数
* hide:$(选择器).hide() :将数组中所有 DOM 对象隐藏起来
* show:$(选择器).show():将数组中所有 DOM 对象在浏览器中显示起来
* remove:$(选择器).remove() : 将数组中所有 DOM 对象及其子对象一并删除
* empty:$(选择器).empty()：将数组中所有 DOM 对象的子对象删除
* append:为数组中所有 DOM 对象添加子对象 ```$(选择器).append("<div>我动态添加的 div</div>")```
* html:设置或返回被选元素的内容（innerHTML）。
  * $(选择器).html()：无参数调用方法，获取 DOM 数组第一个匹元素的内容。
  * $(选择器).html(值)：有参数调用，用于设置 DOM 数组中所有元素的内容。
* **each**:each 是对数组，json 和 dom 数组等的遍历,对每个元素调用一次函数。
  * 语法 1：$.each( 要遍历的对象, function(index,element) { 处理程序 } )
  * 语法 2：jQuery 对象.each( function( index, element ) { 处理程序 } )
  * index表示数组的下标
  * element数组的对象
### 第一组函数的样例
```html
<script type="text/javascript" src="./js/jquery-3.4.1.js"></script>
    <style type="text/css">
        div{
            background:blue;
        }
    </style>
    <script type="text/javascript">
    $(function(){
       $("#btn1").click(function(){
       $("div").hide();
      })
    $("#btn2").click(function(){
        $("div").show();
    })
    $("#btn3").click(function(){
        $("select").remove();
    })
    $("#btn4").click(function(){
        $("select").empty();
    })
    $("#btn5").click(function(){
        $("div").append("<h1>Hello world</h1>");
        $("div").append("<table border='1px'><tr><td>第一列</td><td>第二列</td></tr></table>");
    })
    $("#btn6").click(function(){
        //如果内容中有标签，不会解析标签样式，会原样输出
       alert($("span").html());
    })
    $("#btn7").click(function(){
        $("span").html("new data<h1>Hello</h1>");
    })
})

    </script>
</head>
<body>
    <select name="" id="">
        <option value="喜洋洋">喜洋洋</option>
        <option value="美洋洋">美洋洋</option>
        <option value="懒洋洋">懒洋洋</option>
        <option value="沸羊羊">沸羊羊</option>
    </select>
    <br><br>
    <select name="" id="">
        <option value="欧洲">欧洲</option>
        <option value="美洲">美洲</option>
        <option value="大洋洲">大洋洲</option>
    </select>
    <span>kyrie<b>最佳控卫</b></span>
    <span>我也是一个span标签呢</span>
    <br><br>
    <div>第一个div标签</div>
    <input type="button" value="隐藏所有的div标签" id="btn1"><br>
    <input type="button" value="显示所有的div标签" id="btn2"><br>
    <input type="button" value="清除所有下拉列表中的option子标签" id="btn3"><br>
    <input type="button" value="删除所有下拉列表" id="btn4"><br> 
    <input type="button" value="添加元素" id="btn5"><br>
    <input type="button" value="获取第一个dom的文本值" id="btn6"><br>
    <input type="button" value="修改所有span标签的文本值" id="btn7"><br>
```
### 数组的各种遍历方式
```html
<title>遍历各种数组</title>
    <script type="text/javascript" src="./js/jquery-3.4.1.js"></script>
    <script type="text/javascript">
        $(function(){
            $("#btn1").click(function(){
                var arr=[1,2,3,4];
                $.each(arr,function(index,item){
                    alert("index="+index+" value="+item);
                })
            })
            $("#btn2").click(function(){
                var jsonarr={"name":"kyrie","age":15,"position":"PG"};
                $.each(jsonarr,function(key,value){
                    alert("key="+key+"  value="+value);
                })
            })
            $("#btn3").click(function(){
                var domArray=$(":text");
                $.each(domArray,function(index,item){
                    alert("第"+index+"个input标签的值是"+item.value);
                })
            })
            $("#btn4").click(function(){
                $(":text").each(function(index,item){
                    alert("i="+index+" value="+item.value);
                })
            })
        })
    </script>
</head>
<body>
    <input type="text" value="first"><br>
    <input type="text" value="second"><br>
    <input type="text" value="third"><br>
    <input type="button" value="遍历非dom数组" id="btn1"><br>
    <input type="button" value="遍历json数组" id="btn2"><br>
    <input type="button" value="遍历dom数组" id="btn3"><br>
    <input type="button" value="遍历jquery数组" id="btn4"> 
</body>
```

### jquery中的事件
语法:$(选择器).监听事件名称(处理函数)

说明:监听事件名称是js事件中去掉on后的内容，js中的onclick的监听事件名称就是click

举例:
1. 为页面中所有的 button 绑定 onclick,并关联处理函数 fun1
```$("button").click(fun1)```
2. 为页面中所有的 tr 标签绑定 onmouseover，并关联处理函数 fun2
```$("tr").mouseover(fun2)```

### on()绑定事件
on()方法就是在选中元素上添加事件处理程序
语法:```$(选择器).on(event,,data,function)```
1. event：事件一个或者多个，多个之间空格分开
2. data：可选。规定传递到函数的额外数据，json 格式
3. function: 可选。规定当事件发生时运行的函数。
```html
<title>on事件来绑定对象</title>
    <script type="text/javascript" src="./js/jquery-3.4.1.js"></script>
    <script type="text/javascript">
        $("document").ready(function(){
            $("#hello").click(function(){
                $("#first").append("<input type='button' value='我是新添加的按钮' id='newButton'>");
                $("#newButton").on("click",function(){
                    alert("new button");
                })
            })
        })
    </script>
</head>
<body>
    <div id="first">
        我是一个div，我需要新加入一个按钮
    </div>
    <input type="button" value="给div添加一个按钮" id="hello">
</body>
```
![image.png](https://i.loli.net/2021/04/15/MxRue5yz9IJYpHB.png)

## Ajax和Jquery
jQuery 提供多个与 AJAX 有关的方法。通过 jQuery AJAX 方法，您能够使用 HTTP Get 和 HTTP Post 从远程服务器上请求文本、HTML、XML 或 JSON 同时能够把接收的数据更新到 DOM 对象。
### $.ajax()核心方法
$.ajax() 是 jQuery 中 AJAX 请求的核心方法，所有的其他方法都是在内部使用此方法。

语法：```$.ajax( { name:value, name:value, ... } )```
1. 说明：参数是 json 的数据，包含请求方式，数据，回调方法等
2. async ： 布尔值，表示请求是否异步处理。默认是 true
3. contentType ：发送数据到服务器时所使用的内容类型。默认是：```"application/x-www-form-urlencoded"```。
4. data：规定要发送到服务器的数据，可以是：string， 数组，多数是 json
5. dataType：期望从服务器响应的数据类型。jQuery 从 xml, json, text,, html这些中测试最可能
   的类型
   * "xml" - 一个 XML 文档
   * "html" - HTML 作为纯文本
   * "text" - 纯文本字符串
   * "json" - 以 JSON 运行响应，并以对象返回
6. error(xhr,status,error)：如果请求失败要运行的函数, 其中 xhr, status,  error 是自定义的形参名
7. success(result,status,xhr)：当请求成功时运行的函数，其中 result, status, xhr 是自定义的形参
  名
  type：规定请求的类型（GET 或 POST 等），默认是 GET， get，post 不用区分大小写
  url：规定发送请求的 URL。
  8.以上是常用的参数。 error() , success()中的 xhr 是 XMLHttpRequest 对象。

---

#### 遇到的问题

```javascript
 <script>
         $(function(){
            function fun2(){
                var list=$(":radio");
                for(let i=0;i<list.length;i++){
                    alert(list[i].value)
                }
            }
         })
在$()里面的函数是在外部访问不到的，相当于局部变量，使用$()的原因是页面从上网下进行加载，script标签中获取不到下面的dom元素，所以必须写在$()里面，函数的话则不需要
</script>
```

