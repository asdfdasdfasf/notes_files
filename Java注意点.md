**韩顺平0基础入门**

实用快捷键：

* ctrl +H 查看当前类的继承结构图 
* ctrl +F12: 查看当前类的所有方法  
* ctrl +shift +R：全局搜索文本 
* shift +ctrl +↓↑ 挪动代码位置
* shift +Enter:从上一行的任意位置新创建一行 
* ctrl +alt +L：格式化代码
* shift +F9：以debug的方式运行
* ctrl+d;复制当前行内容到下一行
* ctrl+E：查看最近使用的文件 
* ctrl +[ or ] ：跑到最近方法体的括号的开始或者结尾
* ctrl +N :快速输入打开文件或者类
* ctrl +alt +t：快速trycatch 语句块
* ctrl +D：复制当前行
* alt +上下箭头：在方法之间快速定位移动
* ctrl +N: 查找指定类
* ctrl+shift：选中的代码向前缩进
* ctrl +shift +Y:快速查询指定的单词含义
* ctrl+W:递进式的选择代码块，可选中光标所在的单词或者段落，连续按压的话会在原来的基础上再次的增加所选择的范围
* ctrl+O:选择重写方法
* ctrl+Backspace:删除光标前面的单词或者中文句子
* ctrl+Delete:删除光标后面的句子或者单词
* ctrl+shift+o：呼出翻译插件

1.double数据在进行运算的时候可能会存在精度丢失的问题 （加减乘除都可能出现）

2.ascii码存在128个字符，unicode编码是中文两个字节，字母也是两个字节，uft-8的话中文是三个字节，字母是两个字节

3.double默认保留一位小数。例如80.0

4.byte，short，char进行运算的时候都是转换成int再进行转换，只要有这三种的任何一个参与运算结果都是int类型（除非还有更高的精度）

5.boolean类型不参与自动类型转换

6.基本类型转换为string	string +'' ''

7.String转为基本数据类型

 	1. Integer.parseInt()；
 	2. Double.parseDouble();
 	3. Float.parseFloat();
 	4. Long.parseLong();
 	5. Byte.parseByte();
 	6. Boolean.parseBoolean();
 	7. short.parseShort();

8.取模，取余的本质 a%b=a - a / b * b ,总结出来的结论就是余数永远和 ==被模数==同符号 ，当被除数是小数的时候 a%b  == a-(int)a/b*b

9.独立使用前置++等符号没有区别

10. byte b=2; b+=2 等价于 b=(byte)(b+2);
11. switch表达式中不能使用的类型是：无论哪个版本的JDK，都是不支持 long，float，double，boolean 这个一定要注意！（不支持浮点数也很正常，因为可能存在精度丢失，所以无法精确匹配）
12. switch中的case不能使用变量
13. 可变参数的实参可以是0个或者任意个
14. 可变参数的实参可以是数组
15. 可变参数的本质就是数组
16. 可变参数和普通类型的参数一起放在形参列表中，可变参数必须保证在最后
17. **一个形参列表中只能出现一个可变参数**

#### 原码反码补码

* 0表示正数，1表示负数
* 正数的原码，反码，补码都是一样的
* 负数的反码等于符号位不变其他位取反
* 负数的补码等于反码+1
* 0的反码和补码都是0
* Java中没有无符号数，Java中的数都是有符号的
* **计算机运算的时候都是以==补码的方式==运算的**
* ==我们看的运算结果看的是原码==  当我们运算完后的补码符号位是1的时候，需要转成原码 

##### 数组元素定义的几种正确方式

```java
package com.ls.test;

public class Test {
    public static void main(String[] args) {
        int[] arr=new int[]{12,3};
        int[] b=new int[4];
        int[] c={1,2,3,4};
        int[][] d=new int[][]{{1,2,3},{2,3,3,3}};
        int[][] e=new int[3][];
        int[][] f=new int[3][4];
        int[][] g={{1,2,3},{2,3,4,5}};
    }
}
```

##### 数组打印回型数

```java
public class Test {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("请输出你想打印的矩阵范围:");
        int n = scanner.nextInt();
        int length = n * n, i = 0, j = 0;
        int flag = 1; //右边 1 下边 2 左边 3 上边 4
        int[][] nums = new int[n][n];
        for (int m = 1; m <= length; m++) {
            if (flag == 1) {
                if (j < n && nums[i][j] == 0) { //从左往右赋值
                    nums[i][j++] = m;
                } else { //赋值到了最右边的话
                    j--;
                    i++;
                    m--; //本次循环没有赋值，所以需要减一
                    flag = 2;//向下赋值
                }
            } else if (flag == 2) { //从上往下
                if (i < n && nums[i][j] == 0) {
                    nums[i++][j] = m;
                } else {
                    i--;
                    j--;
                    m--;
                    flag = 3;
                }
            } else if (flag == 3) { //从右到左
                if (j >= 0 && nums[i][j] == 0) {
                    nums[i][j--] = m;
                } else {
                    j++;
                    i--;
                    m--;
                    flag = 4;
                }
            } else { //从下往上
                if (j < n && nums[i][j] == 0) {
                    nums[i--][j] = m;
                } else {
                    i++;
                    m--;
                    j++;
                    flag = 1;
                }
            }
        }
        for (int w = 0; w < nums.length; w++) {
            for (int h = 0; h < nums[w].length; h++) {
                System.out.print(nums[w][h] + "\t");
            }
            System.out.println();
        }
    }
}
```

##### 访问权限的作用范围：

| 访问权限     | 本类 | 子类                           | 同包 | 不同包 |
| ------------ | ---- | ------------------------------ | ---- | ------ |
| public       | 可以 | 可以                           | 可以 | 可以   |
| private      | 可以 | 不行                           | 不行 | 不行   |
| default 默认 | 可以 | 不行(同包可以访问，不同包不行) | 可以 | 不行   |
| protected    | 可以 | 可以                           | 可以 | 不行   |

1. 子类不可以直接调用父类的私有属性和方法，但是可以间接的调用公有方法获取
2. 子类必须调用父类的构造器，完成父类的初始化
3. 当创建子类对象时，不管使用子类的哪个构造器，默认会调用父类的无参数构造器，如果父类没有无参数构造器，需要在子类中super指定使用父类的哪个构造器
4. ==知识盲区==:**子类继承父类的属性和方法，属性和方法是子类和父类==共用一份==，当子类定义了和父类一样的属性或者方法的时候，哪么就是两份调用方法和属性的时候会优先去子类找**
5. super必须放在子类构造器的第一行，同理this()也是放在构造器的第一行，两者不能共存
6. 父类构造器的调用不限于直接父类，将一直往上追溯到Object类

内存继承图

![image-20211008143923004](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201379.png)

```java
package com.kyrie.hsp;

/**
 * 1.当对象去访问属性的时候会优先看本类是否有，如果有的话输出本类的属性
 * 2.如果本类没有对应属性就使用父类的，如果父类还没有就又往上找，直到Object对象
 * 3.以上的前提是属性要能够直接访问
 * 4.私有属性也是能被继承下来的，只是不能够直接访问而已，但是可以用公有方法去访问
 */
public class Test {
    public static void main(String[] args) {
        Son son=new Son();
        System.out.println(son.name);//大头儿子
        System.out.println(son.age);//39
        System.out.println(son.hobby);//旅游
    }
}
class GrandFather{
    String name="大头爷爷";
    String hobby="旅游";
}
class Father extends GrandFather{
    String name="大头爸爸";
    int age=39;
}
class Son extends Father{
    String name="大头儿子";
}

```

可以使用super来访问父类的属性和方法，但是不能访问私有的属性和方法   super（）只能使用在构造器当中

==当子类中有和父类中的成员(属性和方法)重名的时候，为了访问父类的成员，必须使用super，如果没有重名，super，this访问效果是一样的==，如果查找方法或者属性的过程中，找到了，但是不能访问，则报错！

super不限于只访问直接的基类，还可以往上访问基类的基类

重写的细节：

1. 子类的方法的形参列表，方法名称要和父类方法的形参列表，方法名称完全一样
2. 子类方法的返回类型和父类方法返回类型一致或者是父类返回类型的子类
3. 子类方法不能缩小父类方法的访问权限 public > protected > 默认 > private

属性没有重写一说，属性的值看编译类型

```java
package com.kyrie.hsp;

public class AccountTest {
    public static void main(String[] args) {
        Base base=new MySon();
        System.out.println(base.count); //10 直接看编译类型，没有重写属性这么一说！编译是Base类型就去Base类找count属性
    }
}
class Base{
    int count=10;
}
class MySon extends Base{
    int count=20;
}

```

instance of 关键字用于判断对象的运行类型是否是XX类型的子类型或者是XX类型

==Java动态绑定机制（非常非常非常重要）==

1. 当调用对象方法的时候，该方法会和该==对象的内存地址/运行类型绑定==
2. 当调用对象属性时，没有动态绑定机制，哪里声明哪里使用

```java
package com.kyrie.hsp;

public class AccountTest {
    public static void main(String[] args) {
        // 编译类型 base ,运行类型MySon
        Base base = new MySon();
        System.out.println(base.sum()); //30 当发现子类没有sum方法时，去父类找，找到了getI()+10,因为运行时是MySon类型，且有getI方法，所有调用的是子类的getI 也就是20 + 10
        System.out.println(base.sum1());//20 同理找不到去父类找，因为对象属性没有动态绑定，在哪里调用就用谁的 所以是 10 +10
    }
}

class Base {
    public int i = 10;

    public int sum() {
        return getI() + 10;
    }

    public int sum1() {
        return i + 10;
    }

    public int getI() {
        return i;
    }
}

class MySon extends Base {
    public int i = 20;

    public int getI() {
        return i;
    }

}
```

Object 类详细解释

String equals 源代码

```java
private final char value[];
public boolean equals(Object anObject) {
    if (this == anObject) { //判断是否是本身
        return true;
    }
    if (anObject instanceof String) { //判断类型
        String anotherString = (String)anObject;
        int n = value.length;
        if (n == anotherString.value.length) { //判断字符长度是否相等
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) { 
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
}
```

hashCode方法

==知识盲区==：当使用 == 来判断两个变量是否相等时，如果两个变量是基本类型变量，且都是数值类型(不一定要求数据类型严格相同)，则只要两个变量的值相等，就返回true。好比 65.0==65结果是true

1. 两个引用如果指向的是同一个对象，那么哈希值肯定是一样的
2. 两个引用如果指向的是不同的对象，那么哈希值绝大多数不一样(出现hash冲突时可能会相同)、
3. 哈希值主要是根据地址号，不能完全将哈希值等价于地址
4. 在集合中hashCode如果需要的话，也会重写

toString方法

1. 默认返回：全类名+@+哈希值的十六进制

```java
 public String toString() {
        return getClass().getName() + "@" + Integer.toHexString(hashCode());
    }
```

finalize方法

1. finalize是Object的一个方法，垃圾回收的时候会调用对象的finalize方法
2. 可以使用System.gc（）来要求编译器调用finalize方法，但是不一定管用

debug的相关快捷键

1. F7进入方法内 (进入7黑的方法内部)
2. F8单行执行  （一步一步发发发）
3. shift+F8跳出方法
4. F9执行到下一个断点
5. Settings->Stepping 然后取消java.* 和javax.*就可以进入源码的方法里面了

**非静态方法可以访问类里面的所有属性**，**静态的不能访问类相关的属性和关键字**

普通代码块每创建一次对象就会调用一次，静态代码块是类加载的时候执行，并且只执行一次

1. static代码块类加载的时候执行，只会执行一次
2. 普通代码块是创建对象的时候调用，创建一次调用一次
3. 调用静态变量或者方法都会加载类

创建一个对象时在一个类的调用顺序：

1. 调用静态代码块和静态属性初始化（静态代码块和静态属性初始化调用的优先级一样，如果有多个静态代码块和多个静态变量初始化，按照定义的位置顺序调用)
2. 调用普通代码块和普通属性的初始化，按照声明的位置进行调用，普通代码块可以调用任意的属性无论是静态还是非静态
3. 静态代码块和普通代码块一起的时候，不论声明位置前后，优先调用的是静态先关的东西(静态变量初始化，代码块执行  )
4. ![image-20211011093111306](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201380.png)
5. ==对于一个构造器来说，首先由默认的super(),其次是调用普通代码块，然后才会执行构造器中的代码==
6. 当存在多个继承关系的时候遵循下面的加载顺序
   1. 首先回去加载父类的静态属性和静态代码块
   2. 然后加载子类的静态代码块和静态属性
   3. 加载父类的普通代码块和普通属性的初始化
   4. 执行父类构造器
   5. 加载子类的普通代码块和普通属性初始化
   6. 子类的构造方法

##### 单例设计模式(饿汉式和懒汉式)

1. 构造器私有化->防止用户直接new
2. 类的内部创建对象
3. 向外暴露一个静态的公共方法用于返回实例、
4. 饿汉式实现方式，可以并没有使用单例对象从而造成资源浪费(不存在线程安全问题)

```java
package com.kyrie.single;

public class SingleTon01 {
    public static void main(String[] args) {
        GirlFriend girlFriend=GirlFriend.getInstance();
        GirlFriend girlFriend1=GirlFriend.getInstance();
        System.out.println(girlFriend1==girlFriend); //true ,静态属性的初始化在类加载的时候就完成了，且只加载一次，所以为同一个对象
    }
}
//饿汉式 ，所谓的饿汉式就是在一开始类加载的时候就已经实例化了，并且创建了单例对象
class GirlFriend{
    private String name;
    //2.类的内部创建类的实例,为了能够直接获取对象设置为static
    private static GirlFriend girlFriend=new GirlFriend("DLL");
    //1.将构造器私有化
    private GirlFriend(String name){
        this.name=name;
    }
    //3.提供一个公共的static方法，返回对象
    public static GirlFriend getInstance(){
        return girlFriend;
    }
}

```

经典应用：java.lang.Runtime类

```java
public class Runtime {
    private static Runtime currentRuntime = new Runtime();

    /**
     * Returns the runtime object associated with the current Java application.
     * Most of the methods of class <code>Runtime</code> are instance
     * methods and must be invoked with respect to the current runtime object.
     *
     * @return  the <code>Runtime</code> object associated with the current
     *          Java application.
     */
    public static Runtime getRuntime() {
        return currentRuntime;
    }

    /** Don't let anyone else instantiate this class */
    private Runtime() {}
}
```

1. 懒汉式，只有当用户使用getInstance的时候才会创建单例对象，再次使用的时候也是用上次创建的单例对象(存在线程安全，可能会创建多次单例对象)

```java
package com.kyrie.single;

import java.util.Base64;

public class SingTon02 {
    public static void main(String[] args) {
        Basketball basketball = Basketball.getInstance("斯伯丁");
        Basketball basketball1 = Basketball.getInstance("斯伯丁");
        System.out.println(basketball1==basketball);//true
    }
}
class Basketball{
    private String name;
    private static Basketball basketball=null;
    private Basketball(String name){
        this.name=name;
    }
    public static Basketball getInstance(String name){
        if(basketball==null){
            basketball=new Basketball(name);
        }
        return basketball;
    }

}
```

##### final关键字的使用

1. final修饰的类无法被继承
2. final修饰的方法无法被重写
3. final修饰的变量一旦赋值后就无法修改
4. 如果final修饰的是成员变量，那么可以在普通代码块，或者构造方法中赋值
   1. ==复习感悟：因为构造方法先于成员变量的初始化（成员变量的初始化在构造方法中执行），而在构造方法中又会先执行普通代码块，所以放在这两个地方对其赋值没有问题==
5. 如果final修饰的是静态变量，那么要么定义的时候赋值，要么在静态代码块中赋值
   1. ==复习感悟：其实也很好理解，因为类加载的时候首先执行的是静态代码块，然后才是对静态变量赋值==
6. final方法虽然不能重写，但是可以继承，final修饰的类虽然不能被继承，但是可以创建实例
7. final不能修饰构造器
8. **final和static关键字连用修饰的常量的话，不会导致类被加载，底层做了优化**

##### 抽象类

1. 抽象类中可以有抽象方法，也可以有其他的正常方法，可以有构造函数，但是抽象类无法创建实例
2. 抽象方法必须在抽象类中，但抽象类中可以没有抽象方法
3. abstract关键字只能修饰类和方法，==抽象类的本质还是类==
4. 类继承抽象类必须实现所有的抽象方法，除非也是声明为抽象方法
5. **private,final,static关键字不能与abstract关键字连用 ，final修饰的方法不能被重写，private方法子类也不能重写，static与类级别无关，和重写无关**

##### 抽象类-模板设计模式

1. 核心思想就是当多个类要实现的功能流程基本一致，唯一不同的地方就是两个相同方法中执行的代码不同
2. 例如: 实现一个需求为：统计不同的代码执行消耗的不同时间，将job方法抽象出来，实现类中只用实现job方法，不用写冗余的统计时间的东西了。

```java
package com.kyrie.single;

import java.time.temporal.TemporalAccessor;

public class Abstract01 {
    public static void main(String[] args) {
        Template template = new A();
        Template template1 = new B();
        template.calculateTime();
        template1.calculateTime();
    }
}

abstract class Template {
    public abstract void job();

    public void calculateTime() {
        long startTime = System.currentTimeMillis();
        job(); //在这里会进行动态绑定，如果是子类类型，则会调用子类的job方法
        long endTime = System.currentTimeMillis();
        System.out.println("A类job方法执行消耗:" + (endTime - startTime));
    }
}

class A extends Template {
    public void job() {
        int sum = 0;
        for (int i = 0; i < 100000; i++) {
            sum += i;
        }
    }
}

class B extends Template {
    public void job() {
        int sum = 0;
        for (int i = 0; i < 300000; i++) {
            sum += i;
        }
    }
}
```

##### 接口使用

1. 在JDK7.0之前接口里面的所有方法都是没有方法体，都是抽象方法
2. 在JDK8后接口可以有静态方法，默认方法，也就是说接口中可以有方法的具体实现
3. 在接口中方法可以省略abstract关键字，接口中定义的属性都是常量都是省略了static final 的，且公开。
4. 抽象类实现接口时，可以不实现接口的抽象方法
5. ==类可以同时实现多个接口，但是只能继承一个类==
6. 接口和接口之间是继承关系，一个接口可以继承多个接口

```java
interface Computer {
    //可以定义属性
    public int n1 = 10;
	int n2=300; //等价于public static final int n2=300;
    //定义抽象方法
    public void hi();

    //默认实现方法
    default public void ok() {
        System.out.println("ok");
    }

    //可以用静态方法
    public static void cry() {
        System.out.println("cry....");
    }
}
```

##### 多态的传递性

```java
package com.kyrie.single;

public class Test {
    public static void main(String[] args) {
             //多态的传递现象  C实现了Second ，Second继承了First，所以相当于C实现了First
            First first=new C();
    }
}

interface First {
    void show();
}

interface Second extends First {
}

class C implements Second {

    @Override
    public void show() {
        
    }
}
```

##### 问题分析

```java
package com.kyrie.single;

public class Test {
    public static void main(String[] args) {
        //多态的传递现象  C实现了Second ，Second继承了First，所以相当于C实现了First
        new C().show();
    }
}

interface Aa {
    int x = 0;
}

class Bb {
    int x = 1;
}

class C extends Bb implements Aa {
    public void show() {
        //这里有歧义了，因为继承下来的Bb里面有x，接口中还有个常量x，出现冲突
        //因为成员方法中可以使用静态变量和成员变量，所以会不知道调用哪个
        System.out.println(x);
    }
}
```

##### 内部类

1. 一个类的内部又完整的嵌套了另一个类结构，被嵌套的类称为内部类，嵌套其他类的称为外部类
2. 类的五大成员：属性，方法，构造器，代码块，内部类
3. ==内部类的最大特点就是可以直接访问私有属性，并且可以体现类于类之间的包含关系==
4. 内部类的分类
   1. 定义在外部类局部位置上(比如方法内)
      1. 局部内部类（有类名）
      2. 匿名内部类（没有类名 重点）
   2. 定义在外部类的成员位置上
      1. 成员内部类（没用static修饰）
      2. 静态内部类（使用static修饰）

##### 局部内部类

```java
package com.kyrie.inner;

/**
 * 局部内部类的使用
 * 1.是定义在外部类的局部位置，通常在方法中
 * 2.不能添加访问权限修饰符，但是可以添加final关键字,因为相当于是一个局部变量，局部变量没有访问权限修饰符
 * 局部类的本质还是一个类
 * 3.可以访问外部类的所有属性和方法
 * 4.访问范围只能是定义它的方法或者是代码块中
 * 5.外部类直接在定义内部类的方法或者代码块中创建对象，然后就可以调用内部类的方法了
 * 6.外部其他类不能直接创建内部类的对象
 * 7.如果外部类和内部类的成员重名的时候，遵循的就近原则，如果想访问外部的成员使用 （外部类名.this.属性名）
 * Outer.this 本质就是外部类的对象，谁调用了m1()方法，谁就是那个对象，以下代码为outer对象
 */
public class InnerClass01 {
    public static void main(String[] args) {
        Outer outer = new Outer();
        outer.m1();
        System.out.println("outer的hashCode=" + outer.hashCode());
    }
}

class Outer {
    private int n1 = 100;
    private String name = "kyrie";

    private void m2() {
        System.out.println("Outer m2方法");
    }

    public void m1() {
        //局部内部类
        class Inner02 {
            private String name = "James";

            public void f1() {
                //可以直接访问外部类的所有成员，包含私有的
                System.out.println("n1=" + n1);
                //调用外部类的私有方法
                m2();
                //就近原则输出内部类的name值
                System.out.println("内部类的name=" + name);
                //去访问外部类的属性
                System.out.println("外部类的name=" + Outer.this.name);
                System.out.println("Outer.this的hashCode=" + Outer.this.hashCode());
            }
        }
        //在创建内部类的方法中直接穿件内部类对象
        Inner02 inner = new Inner02();
        inner.f1();
    }

    public void show() {

        // Inner02 inner02=new Inner02(); 尝试调用但是访问不到
    }
}
```

匿名内部类

1. 本质还是类，2. 四个内部类，3. 该类没有名字 4.同时还是一个对象

   ```java
   package com.kyrie.inner;
   
   /**
    * 底层会分配类型 类名为B$1
    * class B$1 implements Tool{
    *	@Override
    *  public void showTool() {
    *
    * 	}
    * }
    * 底层创建了B$1的实例，然后将实例的地址赋值给tool变量
    * 1.匿名内部类本身就是一个对象，可以直接去调用方法
    * 2.可以直接访问外部类的所有成员，包含私有的
    * 3.不能添加访问修饰符，因为它的地位就是一个局部变量
    * 4.作用域：仅仅在定义它的方法或者代码块中可以使用
    */
   public class InnerClass02 {
       public static void main(String[] args) {
           new B().infoShow();
       }
   }
   
   class B {
       private String toolName;
       private int n1 = 100;
   
       public void infoShow() {
           //1.编译类型是Tool类型
           //2.运行类型是B$1类型
           Tool tool = new Tool() {
               private int n1 = 200;
   
               @Override
               public void showTool() {
                   System.out.println("内部类的n1=" + n1); //200 
                   System.out.println("外部类的n1=" + B.this.n1); //100
               }
           };
           tool.showTool();
       }
   }
   
   interface Tool {
       public void showTool();
   }
   ```

##### 成员内部类

1. 成员内部类是定义在外部类的成员位置，并且没有static修饰
2. 可以直接访问外部类的所有成员，包含私有的
3. 可以添加任意访问修饰符，因为它的地位是一个成员(public protected,private,default)
4. 作用域和外部类的其他成员一样，为这个类体
5. 外部类访问成员内部类先创建对象，然后再访问
6. 外部其他类访问内部类也就是先创建对象
7. 内部类的属性和外部类的属性重名的时候还是就近原则

```java
package com.kyrie.inner;

public class InnerClass03 {
    public static void main(String[] args) {
        Outer01.Inner01 OI = new Outer01().new Inner01();
    }
}

class Outer01 {
    private int n1 = 10;
    public String name = "张三";

    class Inner01 {//成员内部类
		private int n2=20;
        public void say() {
            System.out.println("Outer01的n1=" + n1 + " Outer01的name=" + name);
            System.out.println("内部类的n1=" + n1); 
            System.out.println("外部类的n1=" + Outer01.this.n1);
        }
    }
}

```

##### 静态内部类

1. 定义在外部类的成员位置，并且有static修饰
2. 可以直接访问外部类的所有静态成员，包含私有的，但是不能直接访问非静态成员
3. 可以添加任意访问修饰符(public ,protected,default ,private )因为它的地位就是一个成员
4. 作用域：同其他的成员，为整个类体

```java
package com.kyrie.inner;

public class InnerClass04 {
    public static void main(String[] args) {
        //外部其他类使用静态内部类
        Outer02.Inner02 inner02 = new Outer02.Inner02();
        inner02.say();
    }
}

class Outer02 {
    private int n1 = 10;
    private static String name = "kyrie";

    public static class Inner02 {
        private static String name = "zhangsan";

        public void say() {
            System.out.println(name);
//            System.out.println(n1);//报错，不能直接访问非静态成员
            System.out.println("inner name=" + name);
            System.out.println("outer name=" + Outer02.name);
        }
    }

    public void m1() {
        new Inner02().say();
    }
}
```

##### 枚举类

1. 使用enum关键字来声明枚举类

```java
package com.kyrie.enum_;


/**
 * 如果使用enum来实现枚举，要求定义常量对象，写在前面
 * 使用enum关键字开发枚举类默认会继承Enum类，并且是一个final类，可以用javap 类.class 文件反编译证明
 * SPRING("春天","温暖")底层创建的实际上还是一个public static final 的Season对象
 * Compiled from "Enum01.java"
 * final class com.kyrie.enum_.Season extends java.lang.Enum<com.kyrie.enum_.Season> {
 * public static final com.kyrie.enum_.Season SPRING;
 * public static final com.kyrie.enum_.Season SUMMER;
 * public static com.kyrie.enum_.Season[] values();
 * public static com.kyrie.enum_.Season valueOf(java.lang.String);
 * public java.lang.String getName();
 * public java.lang.String getDesc();
 * static {};
 * }
 * 如果使用无参构造器，可以省略小括号
 * Enum的toString方法
 */
public class Enum01 {
    public static void main(String[] args) {
        System.out.println(Season.SPRING);
    }
}

enum Season {
    SPRING("春天", "温暖"), SUMMER("夏天", "酷热"), WINNER;
    private String name;
    private String desc;

    private Season() {
    }

    private Season(String name, String desc) {
        this.name = name;
        this.desc = desc;
    }

    public String getName() {
        return name;
    }

    public String getDesc() {
        return desc;
    }
}
```

自定义枚举类

1. ```java
   package com.kyrie.enum_;
   
   /**
    * 自定义枚举类
    * 1.将构造方法私有化
    * 2.对外不提供set方法
    */
   public class SelfEnum {
       public static void main(String[] args) {
           NBA nba = NBA.CURRY;
           System.out.println(nba);
       }
   }
   
   class NBA {
       public static final NBA JAMES = new NBA("James", "历史第一小前锋");
       public static final NBA KYRIE = new NBA("KYRIE", "历史第一后卫");
       public static final NBA CURRY = new NBA("CURRY", "历史第一扣篮王");
       private String name;
       private String description;
   
       public NBA(String name, String description) {
           this.name = name;
           this.description = description;
       }
   
       public String getName() {
           return name;
       }
   
       public String getDescription() {
           return description;
       }
   
       @Override
       public String toString() {
           return "NBA{" +
                   "name='" + name + '\'' +
                   ", description='" + description + '\'' +
                   '}';
       }
   }
   ```

![image-20211012113742511](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201381.png)

enum类不能再继承了，因为它默认继承了Enum，无法实现多继承

enum实现接口问题

```java
package com.kyrie.enum_;

public class Enum3 {
    public static void main(String[] args) {
        Number number = Number.FIRST;
        number.playMusic();
        TEST.MyTEST.playMusic();
    }
}

interface Music {
    public void playMusic();
}

//枚举类的本质还是一个类
enum Number implements Music {

//    使用匿名内部类的方式 相当于public
//    static final Number FIRST = new Number() {
//        @Override
//        public void playMusic() {
//            System.out.println("first play music");
//        }
//    }
//    }
    FIRST() {
        @Override
        public void playMusic() {
            System.out.println("first play music");
        }
    },
    SECOND() {
        @Override
        public void playMusic() {
            System.out.println("second play music");
        }
    }
}

enum TEST implements Music {
    MyTEST();

    @Override
    public void playMusic() {
        System.out.println("Hello world");
    }
}
```

##### 注解：修饰注解的注解称为元注解

1. @Override:限定某个方法是重写父类的方法

   ```java
   @Target(ElementType.METHOD) //表示只能放在方法上该注解
   @Retention(RetentionPolicy.SOURCE)
   public @interface Override {
   }
   ```

2. @Deprecated:表示某个类或者方法是过时的，不推荐使用，但是任然可以使用

   ```java
   @Documented
   @Retention(RetentionPolicy.RUNTIME)
   //构造器，字段，局部变量，方法，包，参数，类
   @Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, PARAMETER, TYPE})
   public @interface Deprecated {
   }
   ```

3. @SuppressWarnings:消除程序存在的警告

   ```java
   @SuppressWarnings({"all","unchecked"}) 消除指定范围的对应警告
       
   @Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE})
   @Retention(RetentionPolicy.SOURCE)
   public @interface SuppressWarnings {
       String[] value();//所以可以传递多个值
   }
   ```
   
4. 几个对应的元注解

   1. Retention:指定注解的作用范围，三种 SOURDE(作用于编译级别),CLASS(作用于class文件),RUNTIME（运行时生效）
   2. Target：指定注解可以在哪些地方使用
   3. Documented：指定该注解是否会在javadoc体现，该元注解修饰的注解会保留在Javadoc文档上面
   4. Inherited:子类会继承父类注解，如果父类用了被该元注解修饰的注解a，那么子类继承它的时候也会默认有a注解

#### 异常相关知识

1. ==异常分为Error和Exception==
   1. Error:Java虚拟机无法解决的严重问题，如JVM系统内部错误，资源耗尽等，Error是严总错误，程序会奔溃
   2. Exception:分为运行时异常和编译时异常，运行时异常是程序运行的时候才发生的，编译时异常是在编译阶段就出现的异常

异常的大概继承图![image-20211012221242052](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201382.png)

2. try catch finally解决或者throws 抛出异常给上级  
3. 可以使用多个catch捕获多个异常，异常捕获从小到大，如果从大到小的话会报错
4. try finally组合相当于没有捕获异常，执行完finally之后还是会终止程序
5. 子类重写父类的方法的时候，不能扩大异常的范围，但是可以缩小异常的范围 
6. 自定义异常，继承RuntimeException就是运行时异常，继承Exception就是编译时异常
7. throws是异常处理的一种方式，在方法声明出，后面跟的是异常的类型
8. throw是手动生成异常对象的关键字，在方法体中，后面跟的是异常对象
9. 当遇到抛出异常对象的时候，没有cathc语句块且有finally，优先执行finally语句块，执行完后才会执行捕获该异常的方法

```java
package com.kyrie.exception_;


public class Exception01 {
    public static void main(String[] args) {
        Util.showAge(10);
    }
}

/**
 * 一般自定义异常是继承 RuntimeException
 * 即可以使用默认的处理机制
 */
class MyException extends RuntimeException {
    public MyException(String message) { //构造器
        super(message);
    }

}

class Util {
    public static void showAge(int age) {
        if (age == 10) {
            throw new MyException("年龄不符合!");
        }
    }
}
```

#### 自动装箱和自动拆箱

1. 在JDK5之前只能通过手动的装箱和拆箱，到JDK5之后就有了自动装箱和自动拆箱了

```java
package com.kyrie.exception_;

public class IntegerTest {
    @SuppressWarnings({"all"})
    public static void main(String[] args) {
        int n = 100;
        Integer integer = new Integer(n); //手动装箱
        Integer value = Integer.valueOf(200);//手动装箱
        int i = integer.intValue();//手动拆箱

        Integer m = 200; //自动装箱
        int w = new Integer(300); //自动拆箱
    }
}
```

Integer类和Character类的常用方法

![image-20211013101219676](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201383.png)

##### String类的理解和创建对象

1. String类实现了Serializable接口，表示串行化，可以在网络传输
2. String是final类，不能被其他的类继承
3. String有属性private final char values[]：用于存放字符串内容，values的地址指向不能修改，但是可以修改指向地址的内容
4. ==创建对象的两种方式==
   1. 直接赋值Strnig s="hsp" :先从常量池查看是否有hsp数据空间，如果有直接指向，如果没有则重新创建然后指向，s最终指向的是常量池的空间地址
   2. 调用构造器String s2=new String("hsp")：先在堆中创建空间，里面维护了value属性，指向常量池hsp空间，如果常量池没有hsp，重新创建，如果有直接通过value指向，最终指向的是堆中的空间地址
5. ![image-20211013152304543](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201384.png)
6. String的常用方法
   1. equals , equalsIngoreCase, length ,indexOf,lastIndexOf,subString截取指定范围的子串，trim去除前后空格，charAt
   2. toUpperCase,toLowerCase,concat,replace 替换字符串中的字符 split分隔字符串 ，compareTo 比较两个字符串的大小，toCharArray 转换成字符数组  **format**

##### StringBuffer

1. 直接父类是AbstractStringBuilder
2. StringBuffer实现了Serializable接口
3. 在父类汇总AbstractStringBuilder 有属性 char[ ] value 不是final的所有存放在堆区
4. StringBuffer是线程安全的，是一个final类不能被继承
5. 不是每次都更换地址(不是每次都创建新的对象)，所以效率高于String
6. StringBuffer buffer=new StringBuffer()默认创建16大小的char[ ]数组

```java
public StringBuffer() {
    super(16);
}
public StringBuffer(int capacity) {
    super(capacity);
}
public StringBuffer(String str) {
    super(str.length() + 16);
    append(str);
}
public synchronized StringBuffer append(String str) {
    toStringCache = null;
    super.append(str);
    return this;
}
```

```java  
public class IntegerTest {
    public static void main(String[] args) {
        //StringBuffer->String
        StringBuffer buffer = new StringBuffer("hello");
        String s = buffer.toString();

        //使用构造器
        String s1 = new String(buffer);
    }
}
```

![image-20211014081948522](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201385.png)

#### StringBuilder不是线程安全的

1. 用在字符串缓冲区被单个线程使用的时候，可以使用StringBuilder
2. StringBuilder的使用方式和StringBuffer使用方式相同
3. StringBuilder是final类不能被继承，数据还是存放在一个char[ ] 数组中

String,StringBuilder,StringBuffer比较

1. String:不可变字符序列，效率低，但是复用率高
2. StringBuffer：可变字符序列，效率较高(增删),线程安全
3. StringBuilder：可变字符序列，效率最高，线程不安全
4. StringBuilder的效率高于StringBuffer高于String

#### Arrays的常用方法

1. toString 返回数组的字符串形式
2. sort 排序（自然排序和自定义排序 传入一个实现了Comparetor接口的变量 重写compareTo方法）
3. copyOf:数组的复制 Arrays.copyOf(arr,arr.length) 返回一个新的数组 底层调用的是System.ArrayCopy方法
4. fill：数组元素填充
5. asList:将一组数转换成list

#### System常用方法

1. arraycopy：数组的复制，一般用Arrays.copyof
2. currentTimeMillens：返回当前时间举例1970-1-1的毫秒数
3. gc:运行垃圾回收机制 System.gc()

#### BigDecimal注意点

![image-20211014141705949](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201386.png)

#### 日期类

##### Date类：很多方法都过时了，掌握的也还行，就不罗列了

##### Calendar类：是一个抽象类，为特定瞬间于一组例如YEAR，MONTH等日历字段之间的转换提供了一些方法啊，并为操作日历字段提供了一些方法

* 构造器是私有的，可以通过getInstance()来获取实例
* 没有提供对应的格式化类，需要自己组合来输出
* ![image-20211014142701136](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201387.png)

##### 第三代日期类

1. Calendar存在的问题是：
   1. 可变性：日期和时间这样的类应该是不可变的e
   2. 偏移性: Date中的年份是从1900开会的，月份都是从0开始的
   3. 格式化：格式化只对Date有用，Calendar则不行
   4. 此外他们也不是线程安全的
2. LocalDate(年月日)，LocalTime（时分秒），LocalDateTime（年月日时分秒） jdk8加入的
3. ![image-20211014143044878](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201388.png)

#### **集合**

1. Collection集合的继承图

![image-20211014151113319](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201389.png)

2. Map接口的继承图

![image-20211014151404116](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201390.png)

3. Collection集合的常用方法

![image-20211014152444493](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201391.png)

4. 所有实现了Collection接口的都可以使用Iterator的iterator()方法来遍历这个集合

5. ```java
   package com.kyrie.collection_;
   
   import java.util.ArrayList;
   import java.util.Iterator;
   import java.util.List;
   
   public class Collection01 {
       @SuppressWarnings({"all"})
       public static void main(String[] args) {
           List list = new ArrayList();
           list.add("jack");
           list.add(10);
           list.add(true);
           Iterator iterator = list.iterator();
           while (iterator.hasNext()) {
               System.out.println(iterator.next());
           }
       }
   }
   ```

6. List集合中元素有序(取出顺序和添加的顺序相同)，可以重复
7. List集合中的每个元素都有对应的顺序索引
8. List集合的三种遍历方式 
   1. 通过迭代器遍历
   2. 增强for来遍历
   3. list.get(索引)遍历获取

##### ArrayList

1. 可以添加多个null值，有序可重复

2. 底层是基于数组实现的

3. 基本等同于Vector，线程不安全的，执行效率高，在多线程下可以考虑使用Vector

4. ==底层结构和源码分析==

   1. ArrayList中维护了一个Object类型的数组elementData     ==transient== Object[ ] elementData;  表该属性不会被序列化

   2. 创建对象时，如果使用的是无参数构造器，则初始化elementData容量为0，第一次添加，扩容为10，再次扩容为原来的1.5倍

   3. 如果使用的是指定大小的构造器，则初始elementData容量为指定大小，如果需要扩容，则直接扩容elementData为1.5倍

   4. ```java
      private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
      private static final int DEFAULT_CAPACITY = 10;
      transient Object[] elementData;
      private int size;
      //无参数构造
      public ArrayList() {
          this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
      }
      
      //有参数构造
      public ArrayList(int initialCapacity) {
              if (initialCapacity > 0) {
                  this.elementData = new Object[initialCapacity];
              } else if (initialCapacity == 0) {
                  this.elementData = EMPTY_ELEMENTDATA;
              } else {
                  throw new IllegalArgumentException("Illegal Capacity: "+
                                                     initialCapacity);
              }
      }
      
      //add 方法先判断容量是否满足 
        public boolean add(E e) {
              ensureCapacityInternal(size + 1);  
              elementData[size++] = e;
              return true;
          }
      
      //计算最小需要的容量 
        private static int calculateCapacity(Object[] elementData, int minCapacity) {
              if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
                  return Math.max(DEFAULT_CAPACITY, minCapacity);
              }
              return minCapacity;
          }
      //确保最小的容器容量
          private void ensureCapacityInternal(int minCapacity) {
              ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
          }
      //
        private void ensureExplicitCapacity(int minCapacity) {
              modCount++; //记录修改的次数
      
              // overflow-conscious code
              if (minCapacity - elementData.length > 0) //如果需要的最小的容量比现在的容量大
                  grow(minCapacity);//调用进行扩容
          }
      
          private void grow(int minCapacity) {
              // overflow-conscious code
              int oldCapacity = elementData.length;
              int newCapacity = oldCapacity + (oldCapacity >> 1);
              if (newCapacity - minCapacity < 0)
                  newCapacity = minCapacity;
              if (newCapacity - MAX_ARRAY_SIZE > 0)
                  newCapacity = hugeCapacity(minCapacity);
              // minCapacity is usually close to size, so this is a win:
              elementData = Arrays.copyOf(elementData, newCapacity);
          }
      ```

##### Vector

1. 如果是无参，默认10，满之后就按照两倍扩容
2. 如果指定容量大小，那么每次直接按2倍扩容

#### LinkedList底层是双向链表

1. 实现了双向链表和双端队列特点
2. 可以添加任意元素，包括null
3. 线程不安全，没有实现同步

#### ArrayList和LinkedList的比较

![image-20211015093028173](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201392.png)

#### Set接口介绍

1. 无序，没有索引
2. 不允许添加重复元素，最多允许添加一个null
3. 可以通过迭代器和增强for来遍历Set集合
4. 取出的顺序不一定是存的顺序但是每一次的取出的顺序都是固定。

```java
1、HashSet的无参构造方法
2、HashSet可以存放空值，但是只能存放一个
public HashSet() {
    map = new HashMap<>();
}
```

##### 如何保证传入的元素判定是不重复的？，先看一个现象

```java
package com.kyrie.collection_;

import java.util.*;

public class Collection01 {
    @SuppressWarnings({"all"})
    public static void main(String[] args) {
        HashSet set = new HashSet<String>();
        //添加元素是否成功会返回一个boolean值
        boolean hello = set.add("hello");
        //可以通过remove指定删除哪个对象
        boolean remove = set.remove("hello");
        //那么如何判断是否是重复对象呢？也就是判定以下是不同的两个对象
        set.add(new Dog("tom")); //成功
        set.add(new Dog("tom")); //成功
        System.out.println(set);//[Dog{name='tom'}, Dog{name='tom'}]

        //再加深以下，非常经典的面试题
        set.add(new String("linshuai"));//成功
        set.add(new String("linshuai"));//不成功 因为String重写了hashCode方法和equals方法

        System.out.println(set);
    }
}

class Dog {
    private String name;

    public Dog(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Dog{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

##### HashSet底层机制说明

1. HashSet的底层是HashMap，HashMap的底层是（数组+链表+红黑树）当链表的长度到达一个范围之后会转化成红黑树
2. 添加一个元素时，先得到hash值，会转化成索引值
3. 找到存储数据表table，看这个索引位置有没有已经存放的有元素，如果没有直接添加，如果有调用equals比较，如果相同，就放弃添加，如果不同，则添加到最后
4. 在java8中，如果一条链表的元素个数TREEIFY_THRESHOLD(默认是8),并且talbel的大小>=MIN_TREEIFY_CAPACITY(默认64)就会进行树化，当链表的长度超过了8但是table的长度没有超过64的话，就会进行table的扩容，扩容是直接扩容两倍
5. 我们可以猜想：如果没有重写hashcode的话，不同的hashcode会加入到不同的数组的链表上，但实际上是同一个对象，如果只重写了hashcode的话，会加入到数组的同一个下标的链表中，但是没重写equals方法的话就会判定为不同对象，所以要同时重写hashcode和equals方法 
6. 第一次添加的时候会进行默认的初始化大小是16，加载因子0.75，临界值就是 加载因子*数组容量大小，数组到达临界值就开始扩容，每次扩容就是原来的两倍32， 临界值就是32*乘0.75

```java

    1. 执行 HashSet()
    public HashSet() {
    map = new HashMap<>();
}
2. 执行 add()
    public boolean add(E e) {//e = "java"
    return map.put(e, PRESENT)==null;//(static) PRESENT = new Object();
}
3.执行 put() , 该方法会执行 hash(key) 得到 key 对应的 hash 值 算法 h = key.hashCode()) ^ (h >>> 16)
    public V put(K key, V value) {//key = "java" value = PRESENT 共享
    return putVal(hash(key), key, value, false, true);
}
    第 649页
    4.执行 putVal
    final V putVal(int hash, K key, V value, boolean onlyIfAbsent, boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i; //定义了辅助变量
    //table 就是 HashMap 的一个数组， 类型是 Node[]
    //if 语句表示如果当前 table 是 null, 或者 大小=0
    //就是第一次扩容， 到 16 个空间.
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
    //(1)根据 key， 得到 hash 去计算该 key 应该存放到 table 表的哪个索引位置
    //并把这个位置的对象， 赋给 p
    //(2)判断 p 是否为 null
    //(2.1) 如果 p 为 null, 表示还没有存放元素, 就创建一个 Node (key="java",value=PRESENT)
    //(2.2) 就放在该位置 tab[i] = newNode(hash, key, value, null)
    if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);
    else {
        //一个开发技巧提示： 在需要局部变量(辅助变量)时候， 在创建
        Node<K,V> e; K k; //
        //如果当前索引位置对应的链表的第一个元素和准备添加的 key 的 hash 值一样
        //并且满足 下面两个条件之一:
        //(1) 准备加入的 key 和 p 指向的 Node 结点的 key 是同一个对象
        //(2) p 指向的 Node 结点的 key 的 equals() 和准备加入的 key 比较后相同
        //就不能加入韩顺平循序渐进学 Java 零基础
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
        //再判断 p 是不是一颗红黑树,
        //如果是一颗红黑树， 就调用 putTreeVal , 来进行添加
        else if (p instanceof TreeNode)
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        else {//如果 table 对应索引位置， 已经是一个链表, 就使用 for 循环比较
            //(1) 依次和该链表的每一个元素比较后， 都不相同, 则加入到该链表的最后
            // 注意在把元素添加到链表后， 立即判断 该链表是否已经达到 8 个结点
            // , 就调用 treeifyBin() 对当前这个链表进行树化(转成红黑树)
            // 注意， 在转成红黑树时， 要进行判断, 判断条件
            // if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY(64))
            // resize();
            // 如果上面条件成立， 先 table 扩容.
            // 只有上面条件不成立时， 才进行转成红黑树
            //(2) 依次和该链表的每一个元素比较过程中， 如果有相同情况,就直接 break
            for (int binCount = 0; ; ++binCount) {
                if ((e = p.next) == null) {
                    p.next = newNode(hash, key, value, null);
                    if (binCount >= TREEIFY_THRESHOLD(8) - 1) // -1 for 1st
                        treeifyBin(tab, hash);
                    break;
                } if
                    (e.hash == hash &&
                     ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                p = e;
            }
        } if
            (e != null) { // existing mapping for key
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                e.value = value;
            afterNodeAccess(e);
            return oldValue;
        }
    } 
        ++modCount;
    //size 就是我们每加入一个结点 Node(k,v,h,next), size++
    if (++size > threshold)
        resize();//扩容
    afterNodeInsertion(evict);
    return null;
}
*/
}
}
```

#### LinkedHashSet

1. 底层是一个linkedHashMap，底层维护了一个数组加双向链表
2. LinkedHashSet根据元素的hashCode值来决定元素的存储位置，同时用链表维护元素的次序，使得元素看起来是以插入顺序保存
3. 不允许添加重复参数

![image-20211015145827528](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201393.png)

#### Map接口的特点

1. Map保存有映射关系的key-value数据
2. Map中的key和value封装到HashMap$Node类型中，Node是一个静态内部类
3. ==当放入的Key已经存在的时候，会用新的Key去替换旧的值==
4. value是可以重复的
5. Map的key可以为空，value也可以为空，但是Map中的key只能有一个null
6. 常用类String作为key的效率比较高
7. ==Key和value之间存在一对一关系，即通过指定的key总能能够找到对应的value==
8. 为了方便遍历创建了EntrySet 集合，该集合存放的元素类型是Entry，一个Entry对象就包含了key和value
9. entrySet中，定义的类型是Map.Entry，但是实际上还是存放的HashMap$Node类型，因为```static class Node<K,V> implements Map.Entry<K,V>``` Node 实现了Map.Entry类型
10. 当吧HashMpa$Node对象存放到entrySet就方便了我们的遍历 ，提供了getKey和getValue方法
11. ==一对Key value是存放在HashMap$Node中的，又因为Node实现了Entry接口，所有也就说法是一对Key，value就是一个Entry，就是EntrySet中的Entry引用指向了Node对象，不是重新的赋值==
12. 如果只想获取HashMap中的key的话，她内部封装了KeySet集合，可以获取所有的key
13. 如果只想获取HashMap中所有的value的话，内部封装了values集合，可以获取所有的value
14. ==HashMap没有实现同步，线程不安全==

![image-20211016110527497](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201394.png)

Map接口的常用方法

![image-20211016111538174](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201395.png)

结构示意图![image-20211016114305705](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201396.png)

HashMap源码分析结论

![image-20211016114741919](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201397.png)

Map初体验

```java
package com.kyrie.collection_;

import java.util.HashMap;
import java.util.Map;

public class Map01 {
    @SuppressWarnings({"all"})
    public static void main(String[] args) {
        Map map = new HashMap<>();
        map.put(1, "hello");
        map.put(2, "kyrie");
        map.put(null, null);
        map.put(null, "hello");//后面的值覆盖了前面的值
        System.out.println(map.get(1));//hello
        System.out.println(map.toString());//{null=hello, 1=hello, 2=kyrie}
    }
}
```

HashMap的遍历方式

```java
package com.kyrie.collection_;

import java.util.*;

public class Map01 {
    @SuppressWarnings({"all"})
    public static void main(String[] args) {
        Map map = new HashMap<>();
        map.put(1, "hello");
        map.put(2, "kyrie");
        map.put(null, "hello");

        //1.map的遍历方式 keySet获取所有的key 迭代器方式
        Set set = map.keySet();
        Iterator iterator = set.iterator();
        while (iterator.hasNext()) {
            Object key = iterator.next();
            System.out.println("key=" + key + "\tvalue=" + map.get(key));
        }

        //2.获取所有的values,可以用迭代器，增强for
        Collection values = map.values();
        for (Object value : values) {
            System.out.println(value);
        }
        //3.通过EntrySet来获取k-v EntrySet里面存放的就是Entry EntrySet<Entry<k,v>>
        Set entrySet = map.entrySet();
        for (Object entry : entrySet) {
            Map.Entry entry1 = (Map.Entry) entry;
            Object key = entry1.getKey();
            Object value = entry1.getValue();
            System.out.println("key===" + key + " value====" + value);
        }

        //迭代器方式遍历
        Iterator iterator1 = entrySet.iterator();
        while (iterator1.hasNext()) {
            Object next = iterator1.next();
            Map.Entry m = (Map.Entry) next;
            Object key = m.getKey();
            Object value = m.getValue();
            System.out.println("key****" + key + " value***" + value);
        }

        //HashMap遍历要么获取 entrySet，entrySet里面每一个都是一个entry,entry有getKey和getValue的方法来获取
        // 要么获取KeySet，然后通过key获取value
        HashMap<String, Integer> hashMap = new HashMap<>();
        hashMap.put("kyrie", 28);
        hashMap.put("James", 18);
        hashMap.put("Curry", 36);
        Set<Map.Entry<String, Integer>> entrySet1 = hashMap.entrySet();
    }
}
```

#### HashTable的基本介绍

1. hashtable的k和v都不能为空，否则抛出NullPointerException异常
2. hashTable的使用方法大致和hashMap相同
3. hashTable是线程安全的，hashMap是线程不安全的
   1. 底层有数组```private transient Entry<?,?>[] table;``` 初始化大小是11
   2. 临界值 threshold 8= 11*0.75
   3. 扩容机制是原来的容量*2+1 

#### Hashtable和HashMap的区别

![image-20211016142032035](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201398.png)

#### Properties类的介绍

1. Properties类继承Hashtable类，并且实现了Map接口，也是使用一种键值对的形式来保存数据
2. 使用特点和Hashtable类似
3. Properties还可以用于从xxx.propeties文件中，加载数据到Propeeties类对象，并进行读取和修改
4. ==Properties继承了Hashtable，所以它也不允许k和v为null==
5. 同样如果有相同key的值也是进行value的替换

#### TreeSet类的介绍

1. TreeSet底层是TreeMap，底层会调用compare方法，去转换传入的值，所以值不能为空
2. TreeSet不能存放重复的元素
3. 如果构造器不传入Comparator的实例对象，默认底层用的是Comparable接口的compareTo方法，因为大部分类都是实现了这个接口的，但是如果传入了Comparator的实例用的就是传入的实例的compare方法
4. 结论：==所以当我们要放入TreeSet集合中的元素是要实现Comparable接口的，因为，如果使用无参构造的话，会将传入的key强制转换成Comparable接口的类型，调用compareTo方法来达到比较去重的效果==

```java
public V put(K key, V value) {
    Entry<K,V> t = root;
    if (t == null) { //第一次添加的时候t等于null
        compare(key, key); // type (and possibly null) check //用来检测key是否为空

        root = new Entry<>(key, value, null);
        size = 1;
        modCount++;
        return null;
    }
    int cmp;
    Entry<K,V> parent;
    // split comparator and comparable paths
    Comparator<? super K> cpr = comparator;
    if (cpr != null) {
        do {
            parent = t;
            cmp = cpr.compare(key, t.key);
            if (cmp < 0)
                t = t.left;
            else if (cmp > 0)
                t = t.right;
            else
                return t.setValue(value);
        } while (t != null);
    }
    else {
        if (key == null)
            throw new NullPointerException();
        @SuppressWarnings("unchecked")
        Comparable<? super K> k = (Comparable<? super K>) key;
        do {
            parent = t;
            cmp = k.compareTo(t.key);
            if (cmp < 0)
                t = t.left;
            else if (cmp > 0)
                t = t.right;
            else
                return t.setValue(value);
        } while (t != null);
    }
    Entry<K,V> e = new Entry<>(key, value, parent);
    if (cmp < 0)
        parent.left = e;
    else
        parent.right = e;
    fixAfterInsertion(e);
    size++;
    modCount++;
    return null;
}
```

#### Collections的常用方法

![image-20211016171605098](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201399.png)

![image-20211016171710795](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201400.png)

#### 泛型的使用细节

1. 自定义泛型类使用细节

   1. 普通成员可以使用泛型(属性，方法)
   2. 使用泛型的数组，不能初始化 很好理解，因为不能确定数据的类型不知道开辟多少的空间
   3. 静态方法中不能使用类的泛型  静态是和类相关的，在类加载的时候对象还没创建，静态变量和方法加载先于对象的创建 （参考第四点）
   4. 泛型类的类型，是在创建对象时确定的(因为创建对象时，需要指定确定类型)    
   5. 如果创建对象时，没有指定类型，默认是Object

2. 自定义泛型类格式 class 类名<T,R,M.....>{  }

3. 自定义泛型接口

   1. 语法： interface 接口名<T,R...>{  }
   2. 接口中，静态成员也不能使用泛型
   3. 泛型接口的类型，在继承接口或者实现接口时确定
   4. **没有指定类型，默认是Object**

   ```java
   interface Animal<T,E>{
       T showInfo(E msg);
   }
   class Cat implements Animal<String,Integer>{
       @Override
       public String showInfo(Integer msg) {
           return "msg="+msg;
       }
   }
   ```

4. 自定义泛型方法

   1. 泛型方法，可以定义在普通类中 ，也可以定义在泛型类中
   2. 当泛型方法被调用的时候，类型会确认
   3. public void eat(E e){} 修饰符后面没有<T,R...> 不是泛型方法，而是使用了泛型

   ```java
   public class Test {
       @SuppressWarnings({"all"})
       public static void main(String[] args) {
           USB<Integer> usb = new USB();
           usb.useFly("100", 200, 38);//调用泛型方法传入参数编译器会确定类型
       }
   }
   
   class USB<M> {
       public <T, R> void useFly(T t, R r, M m) { //泛型方法可以用自己的泛型，也可以用类定义的泛型
   
       }
   }
   
   class B<T> {
       public void show(T t) {//不是泛型方法，只是使用了类的泛型而已
   
       }
   }
   ```
   
5. 泛型的继承和通配符

   1. 泛型不具备继承性 List<Object> list=new ArrayList<String>(); 不行
   2. <?>:支持任意泛型类型
   3. <? extends A> :支持A类以及A类的子类，规定了泛型的上限
   4. <? super A>:支持A类以及A类的父类，不限于直接父类，规定了泛型的下限
   5. 起到了约束传入泛型的范围的作用

#### 多线程基础

1. 并发：同一个时刻，多个任务交替执行，造成一种“同时”的错觉，简单的说，**单核cpu**实现的多任务就是并发
2. 并行：同一个时刻，多个任务同时执行，多核cpu可以实现 （**多CPU**）
3. Java中最常用的两种方式是继承Thread类和实现Runnable接口，调用线程的start方法
4. ==start()方法调用start0()方法后，该线程并不一定会立马执行，只是将线程变成了可运行状态，具体什么时候执行，取决于CPU，由CPU统一调度==
5. 继承Thread和实现Runnable的区别
   1. 实现Runnable接口方式更加适合多个线程共享一个资源的情况，并且避免了单继承的限制
6. 线程终止：当线程完成任务后，会自动退出，可以通过==使用变量==来控制run方法退出的方式停止线程，用一个boolean值保存循环变量，外界调用set方法修改为false达到结束循环的目的

![image-20211017214436335](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201401.png)

![image-20211017214505514](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201402.png)

7. 守护线程：一般是为工作线程服务，当所有的而用户线程结束，守护线程自动结束

```java
package com.kyrie.method;

/**
 * @author KyrieStudy
 * @version 1.0
 * @date 2021/6/23 16:05
 */
//t线程不是守护线程，是单独的，所以当主线程结束的时候，其实t线程是不受影响的，任然可以继续的执行
//我们想要主线程结束的时候t线程也要结束的话，就将t设置为守护线程,
//    总结:当jvm中不存在用户线程的时候，守护线程就会结束，而不是针对守护具体的某一个线程
public class ThreadMethod03 {
    public static void main(String[] args) throws InterruptedException {
        T3 t3 = new T3();
        t3.start();
        MyDaemonThread t = new MyDaemonThread();
        t.setDaemon(true);
        t.start();
        for (int i = 0; i < 10; i++) {
            System.out.println("欧文在打篮球.....");
            Thread.sleep(1000);
        }
    }
}

class MyDaemonThread extends Thread {
    @Override
    public void run() {
        while (true) {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("守护最好的坤坤");
        }

    }
}

class T3 extends Thread {
    public void run() {
        for (int i = 0; i < 40; i++) {
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("我正在运行，谁都不准走");
        }
    }
}
```

8. JDK中Thread.State枚举表示了线程的几种状态

```java
package com.kyrie.state;

/**
 * @author KyrieStudy
 * @version 1.0
 * @date 2021/6/23 16:24
 */
/*
* 描述线程的6中状态(官方文档)，也可以细分为7中是讲Runnable分为正在运行的，和等待运行的
*   NEW：毫无疑问表示的是刚创建的线程，还没有开始启动。

RUNNABLE:  表示线程已经触发start()方式调用，线程正式启动，线程处于运行中状态。，可以认为有两种，一种是就绪态，一种是运行态，是否运行还要看cpu调度

BLOCKED：表示线程阻塞，等待获取锁，如碰到synchronized、lock等关键字等占用临界区的情况，一旦获取到锁就进行RUNNABLE状态继续运行。

WAITING：表示线程处于无限制等待状态，等待一个特殊的事件来重新唤醒，如通过wait()方法进行等待的线程等待一个notify()或者notifyAll()方法，通过join()方法进行等待的线程等待目标线程运行结束而唤醒，一旦通过相关事件唤醒线程，线程就进入了RUNNABLE状态继续运行。

TIMED_WAITING：表示线程进入了一个有时限的等待，如sleep(3000)，等待3秒后线程重新进行RUNNABLE状态继续运行。

TERMINATED：表示线程执行完毕后，进行终止状态。
* */
public class ThreadState {
    public static void main(String[] args) throws InterruptedException {
        T t = new T();
        System.out.println(t.getName() + " 状态" + t.getState());
        t.start();
        while (Thread.State.TERMINATED != t.getState()) {
            System.out.println(t.getName() + " 状态" + t.getState());
            Thread.sleep(1000);
        }
        System.out.println(t.getName() + " 状态" + t.getState());
    }
}

class T extends Thread {
    public void run() {
        while (true) {
            for (int i = 0; i < 20; i++) {
                System.out.println("hi~~~");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            break;
        }
    }
}
```

10. Synchronized线程同步可以放在方法上和同步代码块，==成员方法上默认是锁this，非静态方法时默认锁的是类.class==
11. 同步代码块锁的对象要求是同一个对象，且==对象不会修改变化，对于基本数据类型来说会进行自动拆箱和装箱，如果修改了值就是不同的对象了，引用数据类型来说就是不能修改引用的指向，但是可以修改指向里面的内容，所以一般定义成final==

```java
int i=1;
i=i++; //1 temp=i -->  i=i+1; i=temp;
System.out.println(i)//1
    
    
int j=1;
j=++j; // i=i+1 --> temp=i; i=temp;
System.out.println(j) //2
```

```java
package com.kyrie.exception_;

public class IntegerTest {
    @SuppressWarnings({"all"})
    public static void main(String[] args) {
 
        Object obj = true ? new Integer(1) : new Double(2.0); //要把三元运算符看做一个整体，会有精度提升
        System.out.println(obj);//1.0
    }
}
```

释放锁的场景

![image-20211017220401214](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201403.png)

不释放锁的场景

![image-20211017220434618](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201404.png)

#### 反射机制

1. 反射机制允许程序在执行期接祖ReflectionApI获取任何类的内部信息，比如成员变量，构造器，成员方法等，并且能操作对象的属性和方法

2. 加载完类之后，在堆中产生了一个Class类型的对象，这个对象包含了类的完整结构信息，通过这个对象得到类的结构，像一面镜子一样，所以称为反射。 

3. 反射机制图![image-20211019104825517](https://kyrie-file.oss-cn-hangzhou.aliyuncs.com/notes202207301201405.png)

4. 反射主要的类有Class,Method 类的方法   Field:代表类的成员变量   Constructor代表类的构造方法

5. ==反射机制和面向对象中的封装性是否矛盾，如何看待？==

   1. 面向对象的封装是建不建议去调的问题
   2. 反射是解决了能不能调用的问题

6. 获取Class对象的三种方式

   1. 对象.getClass()方法
   2. 类.class
   3. class.forName("完整类路径")
   4. 使用类的加载器

   ```java
   public class Reflect01 {
       public static void main(String[] args) throws ClassNotFoundException {
           ClassLoader loader = Reflect01.class.getClassLoader();
           Class loadClass = loader.loadClass("com.atguigu.reflect_.Person");
           System.out.println(Person.class==loadClass);
       }
   }
   ```

7. 不同方式获取到的Class对象都是同一个对象

8. Class类的特点

   1. Class也是Object类的子类
   2. Class不是new出来的，而是系统创建的

   ```java
   public class Class01 {
       public static void main(String[] args) {
           /**
            * 通过反射加载类实际上是走的ClassLoader类的loadClass方法，
            *  public Class<?> loadClass(String name) throws ClassNotFoundException {
            *         return loadClass(name, false);
            *     }
            */
           Class<Person> cls = Person.class;
       }
   }
   ```

   3. 对于某个类的Class对象在内存中只有一份，因为类只加载一次

9. 哪些类型有Class对象

   1. 外部类，成员内部类，静态内部类，局部内部类，匿名内部类
   2. interface ，数组，enum ，annotation ，基本数据类型，void

##### 静态代理

```java
package com.atguigu.proxy;

/**
 * @author KyrieStudy
 * @version 1.0
 * @date 2021/10/24 16:33
 * 静态代理举例
 * 代理模式实现同一套接口，静态代理就是代理类和被代理类在编译期间就固定了
 */
interface Factory {
    void produce();
}
//代理类
class proxyFactory implements Factory {

    private Factory factory;//用被代理对象初始化

    //传入的是哪个对象，代理的就是哪个对象
    public proxyFactory(Factory factory) {
        this.factory = factory;
    }

    @Override
    public void produce() {
        System.out.println("中介做准备工作");
        this.factory.produce();
        System.out.println("中介做收尾工作");
    }
}

//被代理类
class Me implements Factory {

    @Override
    public void produce() {
        System.out.println("我需要找一个工厂");
    }
}

public class StaticProxy {
    public static void main(String[] args) {
        Me me = new Me();
        proxyFactory factory = new proxyFactory(me);
        factory.produce();
    }
}
```

##### 动态代理

```java
package com.atguigu.proxy_;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

/**
 * @author KyrieStudy
 * @version 1.0
 * @date 2021/10/24 16:41
 */
//接口规范
interface Human {
    String getBelief();

    void eat(String food);
}

//被代理类
class SuperMan implements Human {

    @Override
    public String getBelief() {
        return "I believe I can Fly";
    }

    @Override
    public void eat(String food) {
        System.out.println("我喜欢吃" + food);
    }
}

/**
 * 要想实现动态代理，需要如何解决的问题
 * 1. 如何根据加载到内存中的被代理类，动态的创建一个代理类及其对象
 * 2. 当通过代理类的对象调用方法时，如何动态的去调用被代理类中的同名方法
 */
class ProxyFactory1 {
    //调用此方法，返回一个代理类的对象
    public static Object getProxyInstance(Object obj) { //被代理的对象
        //创建一个InvocationHandler对象
        MyHandler handler = new MyHandler();
        handler.bind(obj);
        Object proxyInstance = Proxy.newProxyInstance(obj.getClass().getClassLoader(), obj.getClass().getInterfaces(), handler);
        return proxyInstance;
    }
}

class MyHandler implements InvocationHandler {
 	
    private Object obj;

    //绑定被代理类对象
    public void bind(Object obj) {
        this.obj = obj;
    }

    //当我们通过代理类的对象调用方法a时，就会自动的调用如下的方法：invoke();
    //将被代理类要执行的方法a的功能就声明在invoke中
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println(proxy.getClass());
        //执行被代理类的方法
        Object returnValue = method.invoke(obj, args);
        //被代理类的对象的方法执行的方法的返回值
        return returnValue;
    }
}

public class ProxyTest {
    public static void main(String[] args) {
        SuperMan superMan = new SuperMan(); //创建被代理对象
        Human proxyInstance = (Human) (ProxyFactory1.getProxyInstance(superMan)); //获取到代理类对象
        proxyInstance.eat("方便面");
        System.out.println(proxyInstance.getBelief());
    }
}
```

个人总结：代理模式主要的三个角色：接口，代理类，被代理类，静态代理的三个角色都是明确的，缺点是代理类代理的对象是固定的，如果每多了一个代理类，那么就需要多添加一个被代理类。 动态代理的优势是没有明显的定义代理类，不需要创建多个代理类对象，每次传入不同的被代理对象就会获取到不同的代理对象。

#### Java8新特性

```java
package com.atguigu;
class Employee{
    private int id;
    private String name;
    private Integer age=10;

    public int myCompare(Employee e){
        return this.getAge().compareTo(e.getAge());
    }
    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public Employee() {
        System.out.println("无参数构造方法执行了!");
    }

    public Employee(int id) {
        this.id = id;
        System.out.println("Employee(int) 构造方法执行了");
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }
}
```

```java
package com.atguigu;

import org.junit.Test;

import java.util.Comparator;
import java.util.function.Consumer;

/**
 * @author KyrieStudy
 * @version 1.0
 * @date 2021/10/25 8:18
 */
@SuppressWarnings({"all"})
public class LambdaTest01 {
    public static void main(String[] args) {
        Comparator<Integer> integerComparator = new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
               return Integer.compare(o1,o2);
            }
        };
        Comparator<Integer> com=(x,y)->Integer.compare(x,y);
        System.out.println(com.compare(10, 20));
    }

    //无参无返回值
    //lambda表达式的本质就是作为接口的实例
    @Test
    public void test1(){
        Runnable runnable=new Runnable() {
            @Override
            public void run() {

            }
        };
        Runnable runnable1=()-> {System.out.println("lambda表达式");};
        runnable.run();
    }
    //有参数但是没有返回值
    @Test
    public void test2(){
        Consumer<String> consumer=new Consumer<String>() {
            @Override
            public void accept(String s) {
                System.out.println(s);
            }
        };
        //数据类型可以省略，可以由编译器推断得出，称为类型推断
        Consumer<String> con=(str)->{System.out.println(str);};
        con.accept("hello world");
    }
    //Lambda如果只需要一个参数，参数的小括号可以省略
    @Test
    public void test3(){
        Consumer<String> consumer=new Consumer<String>() {
            @Override
            public void accept(String s) {
                System.out.println(s);
            }
        };
        //数据类型可以省略，可以由编译器推断得出，称为类型推断
        Consumer<String> con=str->{System.out.println(str);};
        con.accept("hello world");
    }
    //多个参数或者多条执行语句，参数小括号和方法体括号不能省略
    @Test
    public void show(){
        Comparator<Integer> integerComparator = new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return Integer.compare(o1,o2);
            }
        };
        Comparator<Integer> com=(x,y)->{
            System.out.println(x);
            System.out.println(y);
           return Integer.compare(x,y);
        };
        System.out.println(com.compare(10, 20));
    }

    //方法体只有一条语句，return和大括号都可以省略
    @Test
    public void test5(){
        Comparator<Integer> com=(x,y)->Integer.compare(x,y);
    }

}
```

Java的四大内置函数式接口

```java
package com.atguigu;

import org.junit.Test;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.function.Consumer;
import java.util.function.Predicate;
import java.util.List;

/**
 * @author KyrieStudy
 * @version 1.0
 * @date 2021/10/25 9:39
 * java内置的四大核心函数式接口
 * Consumer<T> void accept(T t)
 * Supplier<T> T get()
 * Function<T,R> R apply(T t)
 * Predicate<T> boolean test(T t)
 */
public class LambdaTest02 {
    public static void main(String[] args) {

    }
    @Test
    public void test1(){
        happyTime(1000,(x)-> System.out.println("本次花费了"+x+"块钱"));
    }
    public static void happyTime(double money, Consumer<Double> con){
        con.accept(money);
    }

    @Test
    public void test2(){
        String[] strs = {"abc", "bcd", "Abc", "abcsdf", "zdfasdfasdf"};
        List<String> asList = Arrays.asList(strs);
        List<String> d = filterString(asList, x -> x.contains("d"));
        System.out.println(d);
    }
    public List<String> filterString(List<String> stringList, Predicate<String> predicate){
        List<String> list=new ArrayList<>();
        for (String s : stringList) {
            if(predicate.test(s)){
                list.add(s);
            }
        }
        return list;
    }
}
```

**对象引用的几种方式**（重要）

```java
package com.atguigu;

import org.junit.Test;

import java.util.Comparator;
import java.util.function.BiPredicate;
import java.util.function.Consumer;
import java.util.function.Function;

/**
 * @author KyrieStudy
 * @version 1.0
 * @date 2021/10/25 10:04
 * 方法引用和构造器引用
 * 1.使用情景：当要传递给Lambda体的操作，已经有了实现的方法了，可以使用方法引用
 * 2.方法引用本质上就是lambda表达式，而lambda表达式作为函数式接口的实例，所以方法引用，也是函数式接口的实例
 */
public class LambdaTest03 {

    /**
     * 情况1:对象::实例方法
     * Consumer中的 void accpet(T t);
     * PrintStream中的 void println(T t);
     * 具体分为如下三种情况
     * 对象::实例方法
     * 类::实例方法
     * 类::静态方法
     *
     * 方法引用使用的要求：要求接口中的抽象方法的形参列表和返回值类型与方法引用的方法的形参列表和返回值相同
     */
    //以下就是第一种情况 ，对象::非静态方法
    @Test
    public void test1(){
        Consumer<String> con=(x)-> System.out.println(x);
        con.accept("hello");

        Consumer<String> consumer=System.out::println;
        consumer.accept("nihao");
    }

    /**
     * 类::静态方法
     * Comparator中的  int compare(T t1,T t2);
     * Integer中的 int compare(T t1,T t2)
     *
     *  t2.compareTo(T t1)
     */
    @Test
    public void test2(){
        Comparator<Integer> com1=(x,y)->Integer.compare(x,y);
        Comparator<Integer> com2=Integer::compare;

        Comparator<Employee> com3=(x,y)->x.getAge().compareTo(y.getAge());
        Comparator<Employee> com4=Employee::myCompare;
        System.out.println(com4.compare(new Employee(), new Employee()));
        
    }

    /**
     * 类::非静态方法  难理解
     * Comparator中的int compare(T t1,T t2)
     * String中的 int t1.compareTo(t2)
     */
    @Test
    public void test3(){
        Comparator<String> com=(x,y)->x.compareTo(y);
        //因为是 t1.compareTo(t2) 而泛型声明为String类型且t1的类型是String，所以写成 String::compareTo;
        Comparator<String> com1=String::compareTo;
        System.out.println(com1.compare("hello", "Hello"));
    }

    /**
     * 类::非静态方法
     * BiPredicate中的boolean test(T t1,T t2)
     * String中的boolean t1.equals(T2)
     * 相当于把t1参数拿出来了，里面就剩了个t2类型
     */
    @Test
    public void test4(){
        BiPredicate<String,String> bp=(a,b)->a.equals(b);
        BiPredicate<String,String>bp1=String::equals;
        System.out.println(bp1.test("hello", new String("hello")));
    }

    /**
     * 类::非静态方法
     * Function<T,R>中的R apply(T t)         R apply(T t)
     * Employee中的String getName();  相当于  R t.getName();
     * 相当于把T t这个参数拿出到外面了，然后里面就没有参数了，
     */
    @Test
    public void test5(){
        Function<Employee,String> fun=(e)->e.getName();
        Function<Employee,String> fun1=Employee::getName;
    }
}
```

构造器引用

```java
package com.atguigu;

/**
 * @author KyrieStudy
 * @version 1.0
 * @date 2021/10/25 10:56
 */

import jdk.nashorn.internal.ir.FunctionNode;
import org.junit.Test;

import java.util.function.Function;
import java.util.function.Supplier;

/**
 * 构造器引用
 * 类名::new的方式，根据函数式接口中的方法的参数个数，调用不同的构造方法
 */
@SuppressWarnings({"all"})
public class LambdaTest04 {
    //构造器引用
    //Supplier中的 T get()
    @Test
    public void test01() {
        Supplier<Employee> sup = new Supplier<Employee>() {
            @Override
            public Employee get() {
                return new Employee(1001, "Tom");
            };
        };
        Supplier sup1 = () -> new Employee(1002, "Jerry");

        Supplier<Employee> sup2 = Employee::new; //选择无参构造方法创建一个对象
        Employee employee = sup2.get();
    }

    /**
     * Function<T,R>  R apply<T t>
     * Employee的一个参数的构造方法
     * pubic Employee(int id); //和上面相同，相当于传入一个int，返回一个Employee
     */
    @Test
    public void test02() {
        Function<Integer, Employee> fun = (x) -> new Employee(x);

        Function<Integer, Employee> fun1 = Employee::new;
        fun1.apply(1001);
    }


    //数组引用
    //Function<T,R>中的R apply(T t)
    @Test
    public void test03() {
        Function<Integer, String[]> fun = len -> new String[len];

        Function<Integer, String[]> fun1 = String[]::new;
    }
}
```

##### Java8中的Collection接口被扩展，提供了两个获取流的方法

1. default Stream<E> stream() 返回一个顺序流
2. default Stream<E>parallelStream() 返回一个并行流

#### StreamAPI流

```java
//创建用来测试的Employee类
class Employee implements Comparable<Employee>{
    private int id;
    private String name;
    private Integer age=10;

    public int myCompare(Employee e){
        return this.getAge().compareTo(e.getAge());
    }
    public Integer getAge() {
        return age;
    }

    public Employee(int id, String name, Integer age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public Employee() {
        System.out.println("无参数构造方法执行了!");
    }

    public Employee(int id) {
        this.id = id;
        System.out.println("Employee(int) 构造方法执行了");
    }
    public static List<Employee> getEmployeeList(){
        List<Employee> list=new ArrayList<>();
        list.add(new Employee(1001,"马化腾",19));
        list.add(new Employee(1002,"马云",28));
        list.add(new Employee(1003,"刘强东",36));
        list.add(new Employee(1004,"雷军",18));
        list.add(new Employee(1005,"比尔盖茨",21));
        list.add(new Employee(1006,"任正非",34));
        list.add(new Employee(1007,"巴尔扎",45));
        list.add(new Employee(1008,"巴尔扎",45));
        list.add(new Employee(1007,"巴尔扎",45));
        return list;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Employee employee = (Employee) o;
        return id == employee.id && name.equals(employee.name) && age.equals(employee.age);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id, name, age);
    }

    @Override
    public String toString() {
        return "Employee{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public int compareTo(Employee o) {
        return o.getAge()-this.getAge();
    }
}
```

```java
import org.junit.Test;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.function.UnaryOperator;
import java.util.stream.IntStream;
import java.util.stream.Stream;

/**
 * @author KyrieStudy
 * @version 1.0
 * @date 2021/10/25 11:34
 * 1.Stream关注的是对数据的运算，与CPU打交道
 * 2.集合关注的是数据的存储，与内存打交道
 * <p>
 * 2.Stream自己不会存储元素
 * Stream不会改变源对象，而是返回一个持有结果的新Stream
 * Stream操作时延迟执行的，这意味着他们会等到需要结果的时候才执行
 * 3.Stream执行流程
 * Stream的实例化
 * 一系列的中间操作(过滤，映射等)
 * 终止操作
 * 一个中间操作连，对数据源的数据进行处理
 * 一旦执行终止操作，就执行中间操作连，并产生结果，之后，不会再被调用
 */
public class StreamAPITest {
    public static void main(String[] args) {

    }

    //1.通过集合创建Stream流
    @Test
    public void test() {
        List<String> list = new ArrayList<>();
//        default Stream<E> stream(); 返回一个顺序流
        Stream<String> stream = list.stream();
//        default Stream<E> stream(); 返回一个并行流
        Stream<String> stream1 = list.parallelStream();
    }

    //2.通过数组创建Stream流
    @Test
    public void test1() {
        int[] arr = new int[]{1, 2, 3, 4, 5, 6};
        //调用Arrays类的static<T> Stream<T> stream(T[] array) :返回一个流
        IntStream stream = Arrays.stream(arr); //泛型不同返回流的类型不同
    }

    //3.Stream的of方法
    @Test
    public void test2() {
        Stream<Integer> integerStream = Stream.of(1, 2, 3, 4, 5, 6);
    }

    //4.创建无限流，顾名思义能够一直运行下去，需要通过limit操作获取一部分数据
    @Test
    public void test3() {
		//迭代 public static<T> iterate(final T seed,final UnaryOperator<T> f)
        //遍历前10个偶数
        Stream.iterate(0, t -> t + 2).limit(10).forEach(System.out::println);
	
        //生成 public static<T> Stream<T> generate(Supplier<T> s)
        Stream.generate(Math::random).limit(10).forEach(System.out::println);
    }

}
```

```java
/**
 * @author KyrieStudy
 * @version 1.0
 * @date 2021/10/25 14:06
 */

import org.junit.Test;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.stream.Stream;

/**
 * Stream流的中间操作  筛选和切片
 */
public class StreamAPITest1 {
    public static void main(String[] args) {

    }

    //筛选和切片
    @Test
    public void test1() {
        //filter(Predicate p) 接收Lambda，从流中排除某些元素
        List<Employee> list = Employee.getEmployeeList();
        Stream<Employee> stream = list.stream();

        //过滤选择年龄大于20岁的员工
        stream.filter(e -> e.getAge() > 20).forEach(System.out::println);
        System.out.println("========================================");


        //limit(n) 截断流，让其元素不超过给定数量
        //stream.limit(3).forEach(System.out::println); //报错：前面stream已经使用了截断操作，无法再次使用，需要重新获取
        list.stream().limit(3).forEach(System.out::println);

        //skip(n)-跳过元素,返回一个扔掉了前n个元素的流，若流中元素不足n个，则返回一个空流
        list.stream().skip(1).forEach(System.out::println);

        System.out.println("=========================");
        //distinct() 筛选，通过流所生成的元素的hashcode和equals去除重复数据
        list.stream().distinct().forEach(System.out::println);
    }


    //映射
    @Test
    public void test2() {
        //map(Function f)-接收一个函数作为参数，将元素转换成其他形式或提取信息，该函数会被应用到每个元素上，并将其映射成一个新的元素
        List<String> list = Arrays.asList("abc", "efg", "dfa", "erg");
        list.stream().map(str -> str.toUpperCase()).forEach(System.out::println);

        System.out.println("=======");
        //练习：获取员工姓名长度大于2的员工的姓名
        List<Employee> emp = Employee.getEmployeeList();
        //自己写的方式
        //emp.stream().filter(person->person.getName().length()>2).forEach((x)-> System.out.println(x.getName()));

        //使用map方式
        emp.stream().map(Employee::getName).filter(name -> name.length() > 2).forEach(System.out::println);


        //flatMap(Function f)-接收一个函数作为参数，将流中的每一个值都换成另一个流，然后把所有的流合成一个大流
    }

    //展示flatMap和map的区别,flatMap用于操作集合中存储的还是集合的情况
    @Test
    public void test3() {
        List<String> list = Arrays.asList("abc", "efg", "dfa", "erg");
        //map做映射的时候如果里面还是一个流，那么不会把流拆开，类似于list.add(Connection con);
        Stream<Stream<Character>> streamStream = list.stream().map(StreamAPITest1::fromStringToStream);
        //stream里面还是一个流，需要再次遍历，拿到里面的元素
        streamStream.forEach(myStream -> myStream.forEach(System.out::println));
        System.out.println("======================");
        //而flatMap不一样，它类似于lis.addAll(Connection con);它会将集合中的元素拆分开，添加到集合中
        Stream<Character> stream = list.stream().flatMap(StreamAPITest1::fromStringToStream);
        stream.forEach(System.out::println);
    }

    public static Stream<Character> fromStringToStream(String str) {
        List<Character> list = new ArrayList<>();
        for (Character ch : str.toCharArray()) {
            list.add(ch);
        }
        Stream<Character> stream = list.stream();
        return stream;
    }

    //排序
    //sorted() 产生一个新流，按照自然顺序排序
    //sorted(Comparator com)产生一个新流，其中按比较器顺序排序
    @Test
    public void test() {
        //自然排序，按照Integer的默认排序方法
        List<Integer> list = Arrays.asList(5, 3, 2, 1, 7, 4, 9, 0);
        list.stream().sorted().forEach(System.out::println);

        List<Employee> employees = Employee.getEmployeeList();
        employees.stream().sorted().forEach(System.out::println); //如果没有实现Comparator接口的话会报错,或者临时指定一种排序方式

        System.out.println("====================================");
        //sorted(Comparator com)产生一个新流，其中按比较器顺序排序
        employees.stream().sorted(Employee::compareTo).forEach(System.out::println);
    }
}
```

```java
import org.junit.Test;
	
import java.util.*;
import java.util.function.BinaryOperator;
import java.util.stream.Collectors;

/**
 * @author KyrieStudy
 * @version 1.0
 * @date 2021/10/25 16:39
 */
//Stream流的终止操作
public class StreamAPITest2 {
    public static void main(String[] args) {

    }

    //1. 匹配与查找
    // allMatch(Predicate p)检查是否匹配所有元素: 练习：是否所有的员工的年龄都大于18岁
    // anyMatch(Predicate p) 检查是否至少匹配一个元素，练习：是否存在一个年龄大于30岁的
    // noneMatch(Predicate p) 检查是否没有匹配的元素：练习：是否没有员工姓马
    // findFirst 返回第一个元素
    // findAny 返回当前流中的任意元素
    // count 返回流中的元素的总个数
    // max(Comparator c) 返回流中的最大值
    // min(Comparator c) 返回流中的最小值
    // forEach(Consumer c) 内部迭代
    @Test
    public void test1() {
        // allMatch(Predicate p)检查是否匹配所有元素: 练习：是否所有的员工的年龄都大于18岁
        List<Employee> list = Employee.getEmployeeList();
        boolean isTrue = list.stream().allMatch(emp -> emp.getAge() > 18);
        System.out.println(isTrue);

        // anyMatch(Predicate p) 检查是否至少匹配一个元素，练习：是否存在一个年龄大于30岁的
        boolean match = list.stream().anyMatch(emp -> emp.getAge() > 30);
        System.out.println(match);

        // noneMatch(Predicate p) 检查是否没有匹配的元素：练习：是否存在员工姓马
        boolean isHave = list.stream().noneMatch(emp -> emp.getName().contains("詹"));
        System.out.println(isHave);

        // findFirst 返回第一个元素
        Optional<Employee> first = list.stream().findFirst();
        System.out.println(first);

        // findAny 返回当前流中的任意元素
        Optional<Employee> any = list.parallelStream().findAny();
        System.out.println(any);

    }


    @Test
    public void test2() {
        List<Employee> list = Employee.getEmployeeList();
        // count 返回流中的元素的总个数
        long count = list.stream().filter(e -> e.getAge() > 20).count();
        System.out.println(count);

        // max(Comparator c) 返回流中的最大值
        Optional<Employee> max = list.stream().max(Employee::myCompare);
        System.out.println(max);

        // min(Comparator c) 返回流中的最小值
        Optional<Employee> min = list.stream().min(Employee::myCompare);
        System.out.println(min);

        // forEach(Consumer c) 内部迭代,使用迭代器的方式称为外部迭代
        list.stream().forEach(System.out::println);
    }


    //规约
    // T reduce(T identity, BinaryOperator<T> accumulator); 可以将流中元素反复结合起来，得到一个值，返回一个 T identity表示初始值
    // T reduce(BinaryOperator<T> accumulator):可以将流中元素反复结合起来，得到一个值，返回optional<T> 类型的值
    @Test
    public void test3() {
        //练习：计算1-10的自然数的和
        List<Employee> list = Employee.getEmployeeList();
        List<Integer> sum = Arrays.asList(new Integer[]{1, 2, 3, 4, 5, 6, 7, 8, 9, 10});
        Integer sumAll = sum.stream().reduce(0, Integer::sum);
        System.out.println(sumAll);

        //练习：计算公司所有员工年龄的总和
        Optional<Integer> reduce = list.stream().map(Employee::getAge).reduce(Integer::sum);
        System.out.println(reduce);

    }


    //收集
    //collect(Collector c) 将流转换成其他形式，接收一个Collector接口的实现，用于给stream中元素做汇总的方法，
    //Collectors中有很多的静态方法可以返回一个Collector对象
    @Test
    public void test4() {
        //练习：查找年龄大于25岁的员工，结果返回一个List或者Set
        List<Employee> list = Employee.getEmployeeList();
        List<Employee> collect = list.stream().filter(e -> e.getAge() > 20).collect(Collectors.toList());
        collect.forEach(System.out::println);

        List<Employee> list1 = Employee.getEmployeeList();
        Set<Employee> collect1 = list.stream().filter(e -> e.getAge() > 20).collect(Collectors.toSet());
        collect1.forEach(System.out::println);
        System.out.println("===============");

    }

}
```

### Integer相关的面试题

```java
package com.kyrie.exception_;

public class IntegerTest {
    @SuppressWarnings({"all"})
    public static void main(String[] args) {
        Integer i = new Integer(1);
        Integer j = new Integer(1);
        System.out.println(i == j);//false 比较地址
        Integer m = 1;
        Integer n = 1;
        System.out.println(m == n);//true 自动装箱，调用Integer，valueOf方法，-128-127不会创建新的对象
        Integer x = 128;
        Integer y = 128;
        System.out.println(x == y);//false
//          valueOf的源码
//        public static Integer valueOf(int i) {
//            if (i >= Integer.IntegerCache.low && i <= Integer.IntegerCache.high)
//                return Integer.IntegerCache.cache[i + (-Integer.IntegerCache.low)];
//            return new Integer(i);
//        }

    }
}
```



```java
package com.kyrie.exception_;

public class IntegerTest {
    @SuppressWarnings({"all"})
    public static void main(String[] args) {
//示例一
        Integer i1 = new Integer(127);
        Integer i2 = new Integer(127);
        System.out.println(i1 == i2);//F 比较地址，false
//示例二
        Integer i3 = new Integer(128);
        Integer i4 = new Integer(128);
        System.out.println(i3 == i4);//F 只要是new的两个都是false
//示例三
        Integer i5 = 127;//底层 Integer.valueOf(127)
        Integer i6 = 127;//-128~127
        System.out.println(i5 == i6); //T
//示例四
        Integer i7 = 128;
        Integer i8 = 128;
        System.out.println(i7 == i8);//F 不在缓存表中，都调用Integer.valueOf方法，重写new对象
//示例五
        Integer i9 = 127; //Integer.valueOf(127)
        Integer i10 = new Integer(127);
        System.out.println(i9 == i10);//F 一个是缓存表中的，一个是重新new的
//示例六
        Integer i11 = new Integer(128);
        int i12 = 128;
//只要有基本数据类型参与比较， 判断的是
//值是否相同
        System.out.println(i11 == i12); //T
//示例七
        Integer i13 = 128;
        int i14 = 128;
        System.out.println(i13 == i14);//T
    }
}
```

String 面试题（很重要）

```java
String a="hello"+"abc" 创建了几个对象？ 1个，编译器优化等价于String a="helloabc"
可以分析出来，如果创建了hello和abc的话也没有变量指向他们，很合理
    
String a="hello";
String b="abc";
String c=a+b;创建了几个对象？三个对象，c也是指向堆的内存
String c=a+b;首先创建了一个StringBuilder对象，然后调用了append方法，将hello和abc分别添加进去，最后调用toString方法
第一步
public StringBuilder() {
        super(16);
}
第二步
@Override
    public StringBuilder append(String str) {
        super.append(str);
        return this;
    }
第三步
 @Override
    public String toString() {
        // Create a copy, don't share the array
        return new String(value, 0, count);
    }

最总结论：String c="ab"+"cd";常量相加，看的是常量池   String c1=a+b;变量相加是在堆中
```

 
