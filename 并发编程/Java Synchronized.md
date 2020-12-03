##### 基础概念

Synchronized是java的同步锁，保证方法或者代码块在运行时只有一个线程进入临界区。保证共享资源的内存可见性。同时还保证原子性和一致性。一致性是保证方法和代码块的最终一致性。但是不能解决指令重排的问题。

##### 代码实现

```java
// 锁方法,锁定当前方法
public synchronized void setId(Long id) {
  this.id = id;
}
// 锁对象,锁定对象
public synchronized void setId(Long id) {
  this.id = id;
}
// 类锁,绥定该类的实例
synchronized (TestSync.class) {
  this.id = id;
}
```

##### 使用javap工具对TestSync.class文件进行反编译

```java
Compiled from "test.java"
class com.aidc.TestSync {
  com.aidc.TestSync();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: return

  public void setId(java.lang.Long);
    Code:
       0: aload_0
       1: dup
       2: astore_2
       3: monitorenter //获取锁
       4: aload_0
       5: aload_1
       6: putfield      #2                  // Field id:Ljava/lang/Long;
       9: aload_2
      10: monitorexit // 释放锁
      11: goto          19
      14: astore_3
      15: aload_2
      16: monitorexit
      17: aload_3
      18: athrow
      19: return
    Exception table:
       from    to  target type
           4    11    14   any
          14    17    14   any
}
```

java中的对象都有一个monitor与之关联，当代码执行到monitorenter的时候会持有该对象的monitor，其它线程执行到monitorenter的时候回去尝试获取monitor，如果monitor被别的线程持有将无法获取。 在上面的反编译文件中只有使用代码块才会出现monitorenter和monitorexit,同步方法使用的是ACC_SYNCHRONIZED标记，在调用方法前会检查该方法的ACC_SYNCHRONIZED是否被标记。

#### java对象头

对象是存储在jvm的堆内存中，而对象分为三部分，对象头，实例数据，对其填充三部分。

对象头又分为Mark Word，klass pointer（指向类指针）。

##### klass pinter

记录了对象的类型，指向对象的类，虚拟机通过这个知道对象的类型。

##### Mark Word

记录了对象运行时的信息，比如hashcode，gc分代信息，锁的状态（偏向锁，轻量级锁，重量级锁）等信息。