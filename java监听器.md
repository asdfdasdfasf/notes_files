#                                                ·监听器

### 什么是观察者模式：

1. 从现实的角度来说，我们每一个人都是一个观察者，同时也是一个监听者，作为被观察者，我们可以发出一些信息，观察者在接收到这些信息后，会做出相应的反应；作为观察者，我们是可以被“被观察者”所发送出来的信息影响的。**一个被观察者可能存在多个观察者**，也就是说，**一个被观察者发出的信息可能会影响多个观察者**。

2. 观察者设计模式，定义了一种一对多的关联关系，一个对象A与多个对象B，C，D之间建立“被观察与观察关系”，当A对象的状态发生变化时，通知所有观察者对象B,C,D在接收到A的通知后，根据自身实际情况，做出对应改变。

3. 定义观察者接口：

   ```java
   public interface IObserver {
       // 处理被观察者发送来的信息
       void handleMessage(String message);
   }
   ```

   4.定义被观察者接口：

```java
// 定义被观察者
public interface Ioberserables {
    // 添加观察者
    void addObserver(IObserver observer);
    // 删除观察者
    void removeObserver(IObserver observer);
    // 向观察者发送信息
    void notifyObservers(String message);
}
```

5. 定义观察者和被观察者的实现类

   ```java
   // 定义一号观察者
   public class firstObserver implements IObserver {
       @Override
       public void handleMessage(String message) {
           System.out.println("一号观察者接受到消息"+message);
       }
   }
   
   // 定义二号观察者
   public class secondObserver implements IObserver {
       @Override
       public void handleMessage(String message) {
           System.out.println("第二号接受到"+message);
       }
   }
   
   // 定义被观察者
   public class Obseverable implements Ioberserables{
       // 因为有多个观察者，所以创建一个集合来维护所有的观察者
       private List<IObserver> Observers;
       public Obseverable(){
           //在被观察者创建的时候就创建观察者集合；
           Observers =new ArrayList<>();
       }
       // 添加观察者到集合中
       @Override
       public void addObserver(IObserver observer) {
           Observers.add(observer);
       }
       // 从集合中删除指定的观察者
       @Override
       public void removeObserver(IObserver observer) {
           Observers.remove(observer);
       }
   
       @Override
       public void notifyObservers(String message) {
           // 通知每一个观察者
           for(IObserver observer:Observers){
               observer.handleMessage(message);
           }
       }
   }
   
   ```

   6. 观察者模式的测试类

      ```java
      public class MyTest {
          public static void main(String[] args) {
               // 创建多个观察者
              IObserver firstObserver=new firstObserver();
              IObserver secondObserver=new secondObserver();
              // 创建被观察者
              Ioberserables observerable=new Obseverable();
              // 被观察者添加观察者
              observerable.addObserver(firstObserver);
              observerable.addObserver(secondObserver);
              observerable.notifyObservers("kyrie永远的神哟");
              // 被观察者删除观察者
              observerable.removeObserver(firstObserver);
              observerable.notifyObservers("hello world");
          }
      }
      ```


   ## 什么是监听器模式？

   1. 监听器设计模式，是观察者设计模式的一种实现，它并不是23中设计模式之一。这里的**监听器实际对应的就是观察者**，当被监听对象的状态发生改变时，也需要通知监听器，监听器在收到通知后会做出相应改变。

   2. 与观察者设计模式不同的是，**被监听者的状态改变，被定义成为了一个对象，称为事件**：**被监听对象被叫做事件源**，对监听器的通知叫做触发监听器。

   3. 定义和实现事件接口：

      ```java
      / 定义增删改查事件
      //c:create 增加
      //u:update 修改
      //R:retrieve 检索
      //D:delete 删除
      // 对于事件对象，我们一般是需要从事件对象中获取到事件源对象，即被观察者对象，
      // 当我们定义事件对象的时候，很显然应该和一个被监听对象（事件源）进行绑定，以及这个事件源做了什么事情。
      public interface ICURDEvent {
          String CRE_EVENT="create event";
          String UPD_EVENT="update event";
          String RET_EVENT="retrieve event";
          String DEL_EVENT="delete event";
          // 获取事件源
          IListenable getEventSource();
          // 获取事件类型
          String getEventType();
      }
      
      // 实现类
      public class CurdEvent implements ICURDEvent {
           private IListenable eventSource; // 事件源
           private String methodName; //事件源执行的方法名称
          public CurdEvent(IListenable eventSource,String methodName) {
              this.eventSource = eventSource;
              this.methodName=methodName;
          }
      
          @Override
          public IListenable getEventSource() {
              return eventSource;
          }
          // 根据事件源所执行的不同的方法返回不同的事件类型
          @Override
          public String getEventType() {
             String eventType=null;
             if(methodName.startsWith("save")){
                 eventType=CRE_EVENT;
             }else if(methodName.startsWith("delete")){
                 eventType=DEL_EVENT;
             }else if(methodName.startsWith("update")){
                 eventType=UPD_EVENT;
             }else if(methodName.startsWith("find")){
                 eventType=RET_EVENT;
             }return eventType;
          }
      }
      ```

   4. 定义和实现事件源接口：

      由于监听器模式只有一个观察者（即监听器）

      ```java
      // 事件源接口（被观察者）
      public interface IListenable {
          // 设置观察者，即监听器,注册监听器
          void setListener(IListener listener);
          // 触发监听器
          void triggerListener(ICURDEvent event);
      }
      
      
      public class Listenable implements IListenable {
          // 事件源需要注册监听器
          private IListener listener;
          @Override
          // 注册监听器
          public void setListener(IListener listener) {
              this.listener=listener;
          }
          // 触发监听器
          @Override
          public void triggerListener(ICURDEvent event) {
              listener.handleEvent(event);
          }
      }
      
      ```

   5. 定义和实现监听器：

      ```java
      // 监听器接口
      public interface IListener {
          // 处理事件
          void handleEvent(ICURDEvent event);
      }
      
      
      public class Listener implements IListener{
          @Override
          public void handleEvent(ICURDEvent event) {
             String eventType=event.getEventType();
             if(ICURDEvent.CRE_EVENT.equals(eventType)){
                 System.out.println("事件源执行了添加操作");
             }else if(ICURDEvent.UPD_EVENT.equals(eventType)){
                  System.out.println("事件源执行了修改操作");
              }else if(ICURDEvent.DEL_EVENT.equals(eventType)){
                  System.out.println("事件源执行了删除操作");
              }else if(ICURDEvent.RET_EVENT.equals(eventType)){
                  System.out.println("事件源执行了查询操作");
              }
          }
      }
      ```

      

   

