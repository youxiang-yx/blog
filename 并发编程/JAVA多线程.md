## java多线程

#### 基本概念	
​	进程是应用程序运行的单位，一个系统可以同时打开多个进程，多进程之间独立运行，比如同时打开IDEA和Postman就是两个进程，而一个进程可以拥有多个线程，它们之间可以相互通信，java线程使用共享内存+线程私有内存来实现对内存的管理。

#### java创建线程	

Thread类是java创建线程最基本的类，接收实现Runnable接口的对象。

```` java
new Thread(new Runnable() {
  public void run() {
    // 代码实现
  }
}).start();
````

#### 常用方法

```` java
//运行线程，如果创建线程后不调用该方法，线程将不会执行。
public synchronized void start()
  
//等待该线程结束后，再执行。
public synchronized void join()
  
//获取线程执行ID。
public long getId()

//获取线程名称
public final String getName()
  
//获取当前线程的对象
Thread.currentThread()
````

