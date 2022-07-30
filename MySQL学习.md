# MySQL学习总结

### 数据库的基本知识（MySQL必知必会）

##### 一到七章注意要点：

1. net stop mysql 停止MySQL服务  net start mysql 启动mysql服务
2. mysql -h localhost -p 3306 -u root -p
3. show tables from 数据库  再其他库查看指定库中的数据库
4. select database();  查看当前在哪个数据库
5. 数据库是由DBMS创建和管理的容器
6. 使用show columns from table_name 等价于(desc table_name) 的方式可以查看表的每一列的类型，是否为空，默认值，以及其他配置
7. show status 用于显示广泛的服务器状态信息
8. show create database 数据库名 和show create table 表名 用于显示创建表的语句
9. show grants 用于显示授予用户的安全权限
10. show errors和show warnings 用于显示服务器错误和警告信息
11. where 后面不能使用别名去查询，因为此时别名还没有生成，where的执行顺序在select之前
12. 在使用distinct关键字的时候，==distinct单独使用必须前置所有列名前面，和其他函数一起没有位置限制==， 例如： ```SELECT COUNT( DISTINCT player_id ) FROM task```    distinct应用于全部列而不是它的直接后置列，必须是所有列的值相同才会被过滤掉，否则不会被过滤
13. limit关键字：当limit后面只有一个数字的时候表示数据的行数，两个数字的时候表示 行号，返回条数，==行号是从0开始的，也就是说取第二条开始的3条数据表示为 limit 1,3==
14. 在关系型数据库中如果不进行数据排序，默认查找出来的数据都是没有意义的，数据查询出来一般都是数据插入时存放的数据，如果后续数据进行了删除或者修改再次查询出来的顺序可能会发生变化
15. 使用order by关键字进行一列或者多个列（若第一个条件相同，则按第二个条件排序，依次类推）的排序（可通过选择列进行排序，也可以通过非选择列进行排序），默认排序是ASC升序（A-Z），DESC降序（Z-A）
16. order by必须出现在from之后，limit必须出现在order by之后,order by 支持别名排序，函数排序，和表达式排序
17. 使用between和and来查找一个范围的数据，判断数据是否为空用is NULL  is not NULL，使用and 或者是 or用来连接多个查询条件，==当多个and和多个or连用的时候，and的优先级高于or，所以运算的时候要加上括号==
18. in操作符表示一个范围，表示匹配这个范围内的值，匹配的上才显示出来，反之则为NOT IN
19. in和or的作用基本相同，但是in里面可以是select语句，效率上来讲in的效率比or更高，可读性也更好
20. DDL语言：定义数据库的结构，比如创建，修改删除
    1. CREATE TABLE：创建数据库表
    2. ALTER TABLE：更改表结构、添加、删除、修改列长度
    3. DROP TABLE：删除表
    4. CREATE INDEX：在表上建立索引
    5. DROP INDEX：删除索引
    6. SELECT是SQL语言的基础，最为重要。
21. DCL语言：
    1. GRANT：授予访问权限
    2. REVOKE：撤销访问权限
    3. COMMIT：提交事务处理
    4. ROLLBACK：事务处理回退
    5. SAVEPOINT：设置保存点
    6. LOCK：对数据库的特定部分进行锁定
22. DQL：数据库查询语句
23. DML：数据库的增删改

##### 第八章：用通配符进行过滤

1. like进行模糊匹配 %ans%：含有ans的数据 ans%：以ans开头的数据  s%an：以s开头，然后an结尾的数据
2. 使用_来匹配单个字符
3. 使用通配符的几个技巧
   1. 尽量不要使用通配符，能用其他方式达到效果的应该使用其他的操作符
   2. 在确实需要使用通配符的地方，除非绝对必要，否则不要将他们用在搜索模式的开始处，因为这样搜索起来是最慢的
   3. 搜索模式：由字面量和通配符两者组合构建的搜索条件 例如：%hello  he%llo  hell0%,通配符放在开头是最慢的

##### 第九章：用正则表达式搜索

1. 使用正则表达式来匹配select查询出来的结果，对结果进行一个过滤
2. 基本字符匹配：select prod_name from products where prod_name REGEXP  ' 1000' 关键字like被regexp代替 表示查询出包含1000的数据
3. 点：在正则表达式中表示任意匹配任意一个字符  .000:表示任意一个数后面三个0都能匹配上
4. 在MySQL中正则表达式中是不区分大小写的，为区分大小写可以使用 BINARY关键字 ```select cust_name from customers where cust_name regexp binary 'wascals';```
5. ==进行or匹配==：使用 | 符号 用该符号在多个条件之间隔开 ```select name from user where name regexp 'kyrie|James'```查询语句的意思是查询包含kyrie或者James的列
6. ==匹配几个字符之一==：'[123] hello' :表示的含义是查询中有 1 hello  2 hello  3 hello的都满足条件，能匹配上括号中的任意一个就可以
7. [123]表示的含义可以等同于[1-3] 1或者2或者3包含其中一个就可以
8. ==匹配特殊字符==：由于一些字符在正则表达式中有特殊的含义，例如点表示任意一个字符，但是如果需要查询包含点的数据需要使用\\作为前导\\ \ 例如： ```select name from user where name regexp '\\  \ \. ' 表示的含义就是查询name中含有点的数据 

![image-20211004182349054](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/files/notes202207301208746.png)

9. 匹配多个实例：‘\ \ ([0-9] sticks?\ \ )’ 表示的含义是匹配0-9任意一个数，然后？是匹配它前面任何字符出现0次或者1次 ![image-20211004182704378](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/files/notes202207301208747.png)
10. 定位元字符 我们进行正则匹配的时候数据中任意位置满足匹配都会查询出来，如果我们只想查询以某些开头或者以某些结尾的数据就可以用 定位元字符 '^[0-9]\ \ .'表示的含义是匹配以数字或者.开头的数据，就不会查询到中间包含的情况
11. ![image-20211004183527578](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/files/notes202207301208748.png)
12. ^的两种含义：在[ ^ 0-9]中的表示否定，相当于not in的含义 在^[123]的外部表示定位元字符

##### 第十章：创建计算字段

1. 在实际的数据查询过程中，数据库中存放的数据一般都不是我们想要直接展示的数据，可能我们需要的字段数据在多个表中，这种情况下我们就需要使用到计算字段，无论是拼接，或者是大小写转换
2. 计算字段不是真实存放在数据库中的，而是在select查询的时候创建的
3. ==拼接字段==：使用concat函数可以将多个字段进行拼接，多个字段之间用逗号分隔、如果有拼接数据为NULL，那么拼接后的数据都为NULL，可以使用==IFNULL函数 IFNULL(字段，0)==：表示的含义就是如果为空的话默认用0
4. 去除多余空格：trim是去除两边的空格，ltrim去除左边的空格，rtrim是去除右边的空格
5. ==执行算术运算==：可以对不同的列进行算术运算 + - * / 等

##### 第十一章：使用数据处理函数

1. ==文本处理函数==：upper()将数据转换成大写
2. ![image-20211005174639856](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/files/notes202207301208749.png)![image-20211005174755751](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/files/notes202207301208750.png)
3. Soundex考虑了发音和音节的问题，当我们查询一个单词或者人名的时候，精确查询不到，可以使用Soundex来查找发音类似的人名
4. ==日期和时间处理函数==：常见的日期和时间处理函数如下
5. ![image-20211005175656155](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/files/notes202207301208751.png)

##### 第十二章：汇总数据

1. ==聚集数据==：我们经常需要的不是将数据查询出来，而是将数据进行一个汇总，如查询一列中的最大或者最小值，获取行数等
2. avg():返回某列的平均值且忽略值为NULL的数据 count() 返回某列的行数 max() 返回某列的最大值 min()返回某列的最小值 sum() 返回某列的和
3. count(某个字段) ：忽略NULL值， count(*) 不忽略NULL值
4. SUM(A+B+C)：当其中ABC任意一行的数据为NULL的时候，SUM会跳过改行
5. MAX和MIN忽略值为NULL的行

##### 第十三章：分组数据

1. ==数据分组==：以前学习的都是对某些行的操作，或者是上一章的对数据进行汇总操作，例如如果想查询到每一个供应商提供的类型数量的话，就需要用到分组函数，

2. 使用group by关键字进行分组

3. ==使用group by的相关规定==

   1. group by子句中列出的每个列都必须是检索列或者有效的表达式（但不能是聚集函数）。如果在select中使用表达式，则必须在group by子句中指定相同的表达式，不能使用别名
   2. 除聚集计算语句外，select语句中的每个列都必须在group by子句中给出
   3. 如果分组列中有具有NULL值，则将NULL作为一个分组返回，如果有多个NULL值，它们将分为一组
   4. group by语句必须出现在where之后，order by之前

4. ==过滤分组==：很多情况下我们需要对分组后的数据进行再次条件筛选，where已经不能满足，因为where没有分组的概念，所以使用HAVING关键字，HAVING后面能做where能做的所有事情

   1. | order by                         | group by                                                     |
      | -------------------------------- | ------------------------------------------------------------ |
      | 排序产生的输出                   | 分组产生的输出，但不一定是分组顺序                           |
      | 任意列都可以使用(甚至是非选择列) | 只可能使用选择列或者表达式列，而且必须使用每个选择列表达式 group by一般联合聚合函数一起使用，且先于聚合函数执行 |
      | 没有限制                         | group by有一个原则，就是select后面所有的列中，没有使用聚合函数的列，必须出现在group by子句中，（个人测试，select 后面 跟上 非分组列也能执行，但是查询出来的数据没有意义） |

   ##### 第十四章：子查询

   1. 子查询：嵌套在其他查询中的查询，在实际的工作中太多的子查询可读性不好，不能嵌套太多的子查询会导致性能降低
   2. 当在写复杂的sql语句的时候可以先拆分成简单的SQL，一步一步验证，方便调试哪个步骤出现了错误

   ##### 第十五章：联结表

   1. 联结查询：select p.* ,v.vend_name,v.vend_address from vendors v ,products p  where v.vend_id=p.vend_id;
   2. 笛卡尔积：如果没有联结条件的话，第一张表会对第二张表进行逐行匹配，全部检索出来，称为笛卡尔积
   3. 内部联结：目前我们学到的联结称为等值联结，是基于两张表中相等测试
      1. select p.* ,v.vend_name,v.vend_address from vendors v inner join products p on v.vend_id=p.vend_id;
   4. 联结多个表：select cust_name，cust_contact from customers,orders,orderitems where customers.cust_id=orders.cust_id and orderitems.order_num=orders.order_num and prod_id='TNT2';

   ##### 第十六章：创建高级联结

   1. ==自联结==：自联结顾名思义就是自己和自己联结，是同一张表取不同的表名进行自我连接	
   2. ==外部联结==：也就是我们说的外联结，包括left outer join 和right outer join ，外部联结可以查询出没有匹配上的行，left以左边为主表，查询出左边的所有数据

   ##### 第十七章：组合查询

   1. 讲述如何使用UNION操作符将多条select语句组成一个结果集

   2. 使用union关键字可以将多个select语句连接在一起

   3. ```sql
      select vend_id,prod_id,prod_price from products
      where prod_price<=5
      union
      select vend_id,prod_id,prod_price from products
      where vend_id in (1001,1002)
      ```

   4. ==UNION规则==

      1. UNION必须由两条以上的select语句组成，语句之间用UNION分隔（例如四条SQL语句需要三个UNION）
      2. UNION的每个查询必须包含相同的列，表达式或者聚集函数
      3. 列数据类型必须兼容，类型不必完全相同，但必须是DBMS可以隐含转换的类型

   5. UNION在合并多条select语句的执行结果的时候默认会去掉重复的行，但是也可以取消这种行为，使用UNION ALL即可

   6. UNION几乎总是完成与多个where条件相同的工作

   7. 如果想要对UNION的数据进行排序的话，在最后一条select语句后面使用order by进行排序

   ##### 第十八章：全文本搜索

   1. 并非所有的搜索引擎都支持全文本搜索，最常用的两个是MyISAM和InnoDB，前者支持全文本搜索，后者不支持,MySQL 5.6之后两者都支持全文索引

   2. 使用全文本搜索：Match（）指定搜索的列，Against（）指定搜索表达式

      1. ```sql
         select note_text form productnotes 
         where Match(note_text) Against('rabbit')
         ```

   3. 要想使用全文索引，必须通过fulltext指定索引列，可以在创建表的时候添加也可以后面添加

   4. 传递给Match的值必须和fulltext()的定义的值相同

   5. ==布尔文本搜索==：可以在没有fulltext索引的情况下使用，没有仔细阅读，等到用到的时候再来仔细看

   ##### 第十九章：插入数据

   1. 插入一行中的所有数据，插入指定列的数据，插入多条数据，通过select 查询出来的结果作为插入

   ##### 第二十章：更新和删除数据

   1. update table_name set column_name=XXX where column=XXX;
   2. 1.修改记录: **update 表名 set 列1 = 列1的值, 列2 = 列2的值 where …**修改多条语句
   3. 如果在更新多行数据的时候，有一行出现了错误，那么之前所有的修改都会实效，除非使用IGNORE 关键字 update IGNORE table_name.....
   4. delete from table_name where column_name=xxxx;
   5. 如果想要删除表中的所有数据，不要使用delete 去删除每一行，而是用truncate table_name 速度更快，原理是删除原来的表再创建一个新的表 参考truncate https://segmentfault.com/a/1190000022254508的用法  
   
   ##### 第二十一章：创建和操纵表
   
   1. 一个表中只有一个AUTO_INCREMENT列，而且它必须被索引，主键列不能为NULL‘
   2. 如果在插入的时候没有值，可以使用MySQL指定的默认值 default关键字
   3. 外键不能跨引擎，用一个引擎的一张表不能引用另一个引擎的一张表中的列作为外键
   4. ==更新表==：alter table vendors add vend_add char(20):给表添加一个列
   5. 删除表中的一列：alter table vendors drop columns vend_add;
   6. 更多情况是用来给表添加外键
   7. ==重命名表==：rename table customer to customers_provider
   
   ##### 第二十二章：使用视图
   
   1. 视图是虚拟的表，视图只包含使用时动态查询检索数据的查询，我们可以将很复杂的查询语句包装成一个虚拟的表
   2. 为什么要使用视图？
      1. 重用SQL语句
      2. 简化复杂的SQL操作，在编写查询后，可以方便的重用而不必知道它的基本查询细节
      3. 使用表的组成部分而不是整个表
      4. 保护数据，可以给用户授予表的特定部分的访问权限而不是整个表的访问权限
      5. 更改数据格式和表示，视图可返回与底层表的表示和格式不同的数据
   3. **视图只是一个虚表，它只记录了表结构，没有实际数据，每当你调用该视图时，数据库都会先去原始表中提取相关数据来形成映射的，所以视图是时时与原始表保持同步的。**
   4. 需要再次强调的是视图不存在数据，它只是其他表中检索出来的
   5. 因为视图不包含数据，所有每次使用视图的时候都要去检索数据
   6. ==视图的规则和限制==
      1. 视图也是有唯一命名，不能和其他表冲突
      2. 视图可以嵌套，可以用其他视图查询出来的数据作为一个视图
      3. 视图不能索引，视图可以和表一起联结使用
   7. 使用视图：
      1. create view view_name as ........; 创建视图
      2. show create view view_name:查询创建视图的语句
      3. 更新视图可以先删除，然后再创建或者是 create or replace view
   8. **更新视图**：更新视图实际上是对原表进行操作，因为视图本身是不存储数据的，并非所有的视图都能更新，以下情况的视图就不能更新
      1. 使用了group by 和 having 的
      2. 联结查询的
      3. 子查询的
      4. union联结的
      5. 聚集函数 例如 min max sum
      6. distinct关键字的
   
   ##### 第二十三章：存储过程的使用（暂时跳过）
   
   1. 存储过程简单的来说就是为以后的使用而保存的一条或者多条MySQL语句的集合，可将其视为批文件
   
   ##### 第二十四章：使用游标 （暂时跳过）
   
   ##### 第二十五章：触发器 （暂时跳过）
   
   ##### 第二十六行：使用事务（默认隔离级别，可重复读）
   
   1. MyISAM和InnoDB是数据库常用的两种引擎，前者不支持事务，后者支持事务
   
   2. 事务是用来保证数据库的完整性，保证批量的SQL语句要么都成功，要么都不成功
   
   3. transaction  commit rollback savepoint 保存点
   
   4. start transaction开启事务
   
   5. **事务是用来管理insert，delete，update select 语句的，但是也可以在事务语句块中使用create drop 但是这两种操作rollback之后并不能回滚**
   
   6. savepoint  名称 ：设置回滚点   rollback  to  名称 :回滚到指定的保存点，保存点会在事务结束后自动释放
   
   7. MySQL的默认行为是==自动提交==，设置MySQL不自动提交 set autocommit=0|off; 查看是否自动提交命令如下
   
      ![image-20211009095454981](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/files/notes202207301208752.png)
   
      8. 默认自动提交的时候每一条SQL语句就会被当做一个单独的事务，除非显示的开启事务
      9. ==delete和truncate在事务使用中的区别-----delete语句在十五中可以回滚,truncate在事务中不能回滚==
      10. 事务的四大特性：
         1. 原子性：事务包含的操作要么全部成功，要么全部失败回滚
         2. 一致性：从一个一致性状态转变成另一个一致性状态，例如A和B相关转账一共有1000元钱，无论怎么转，在事务结束的时候两个人的钱不能凭空的增加或者减少
         3. 隔离性：多个事务之间的操作时相互隔离的
         4. 持久性：事务一旦提交name对数据库中的数据的改变就是永久性的
      11. 查询当前事务隔离级别：SELECT @@tx_isolation;
      12. 事务的四大隔离级别：在并发事务的时候，可以是 读  读| 读 写 |写 读| 写 写，为了保证并发事务的数据准确，所有才有隔离级别( 数据库的默认隔离级别是可重复读)
          1. 读未提交：可能出现脏读问题（读取到了其他事务还没有提交的数据），更别说不可重复读和幻读了，A事务还没提交，B事务修改了一行的是数据，然后A事务再次查询的话就发现数据修改了，这就是脏读现象
          2. ![image-20211009104627399](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/files/notes202207301208753.png)
          3. 读已提交：读取其他事务提交后的数据，==解决了脏读问题==，但是出现了不可重复读现象，A事务还没结束，但是两次读取到的数据不一致，就是不可重复读(**主要针对的是update操作**)
          4. ![image-20211009105119207](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/files/notes202207301208754.png)
          5. 可重复读：解决了不可重复读，但是出现了==幻读现象==，事务B插入一条数据，然后提交了事务，但是A事务还没有提交事务，无论怎么查数据都和开始读取到的数据一致，到做更新操作的时候，发现受影响的行数多了一行，就像出现了幻觉一样。（**主要针对的是insert操作**）
          6. 串行读：并行事务进行排队，解决了幻读现象，这就不说了是效率最低的一种情况了

##### 视频学习笔记

1. 字符函数：length(字段)获取参数值的字节个数，和字符编码有关，例如utf-8就是中文三个字节，英文一个字节
2. upper('join') lower()：转换大小写的函数
3. substr：截取字符串，具体用法到时候查
4. instr(a,b):返回b在a中第一次出现的位置索引，MySQL中索引是从1开始的
5. select trim('a' from 'aaaaakyrieaaaaa') 表示去掉两边的a字符
6. lpad:用指定的字符实现左填充指定长度   select LPAD('kyrie',10, '*'  ) 表示一个10个字符，不够的左边用星号补齐
7. replace(a,b,c) 替换 用c去替换a中的所有b 
8. round():四舍五入 先去掉符号，四舍五入之后再加上符号
9. ceil():向上取整，返回>=该参数的最小整数
10. floor 下乡取整，返回<=该参数的最大整数
11. str_to_date:字符串转换成日期格式

### 1.MySQL中的SQL是如何执行的

首先MySQL是典型的C/S结构,服务器端用的是mysqld

![image-20210930173430890](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/files/notes202207301208755.png)

连接层：主要是客户端和服务器建立连接，客户端向服务器发送SQL到服务器端

SQL层：对SQL语句进行查询操作

存储引擎层：与数据库文件打交道，负责文件的读取和写入

### 2.SQL层的结构图

![image-20210930173448300](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/files/notes202207301208757.png)

1. 查询缓存：Server如果在查询缓存中发现了这条SQL语句，就会直接将结果返回给客户端；如果没有，就进入到解析器阶段。需要说明的是，因为查询缓存往往效率不高，所以在MySQL8.0之后就抛弃了这个功能

2. 在解析器中对SQL语句进行语法分析、语义分析
3. 优化器：在优化器中会确定SQL语句的执行路径，比如是根据全表检索，还是根据索引来检索等
4. 执行器：在执行之前需要判断该用户是否具备权限，如果具备权限就执行SQL查询并返回结果。在MySQL8.0以下的版本，如果设置了查询缓存，这时会将查询结果进行缓存。（默认查询缓存关闭）
5. 在MySQL8.0以上的版本不再支持查询缓存，因为数据库数据一旦后更新的话那么查询缓存就会清空，只有当数据不会被修改的时候查询缓存才有意义

### 3.常见的存储引擎

1. InnoDB存储引擎：它是MySQL 5.5版本之后默认的存储引擎，最大的特点是支持事务、行级锁定、外键约束等。
2. MyISAM存储引擎：在MySQL 5.5版本之前是默认的存储引擎，不支持事务，也不支持外键，最大的特点是速度快，占用资源少。
3. Memory存储引擎：使用系统内存作为存储介质，以便得到更快的响应速度。不过如果mysqld进程崩溃，则会导致所有的数据丢失，因此我们只有当数据是临时的情况下才使用Memory存储引擎。
4. NDB存储引擎：也叫做NDB Cluster存储引擎，主要用于MySQL Cluster分布式集群环境，类似于Oracle的RAC集群。
5. Archive存储引擎：它有很好的压缩机制，用于文件归档，在请求写入时会进行压缩，所以也经常用来做仓库

### 4.在MySQL中查看一条SQL的执行时间

1.先查看profiling是否开启，查询的命令为select @@profiling;  0表示没有开启1表示开启了，

2. 使用set profiling=1; 开启profiling，开启后可以在SQL执行的时候收集它所消耗的资源

3. 使用show profile;查询所有已执行SQL的消耗时间

4. Show profile for query ID 可以查询出系统所有阶段消耗的时间

5. ### 查看数据库版本  select version();

   