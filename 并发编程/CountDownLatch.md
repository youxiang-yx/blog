#### 功能
CountDownLatch可以使多个正在执行的线程进入等待（await）,直到给定的构造参数变成0时就会释放使用CountDownLatch阻塞的线程。


#### 代码实现

```java
CountDownLatch coun = new CountDownLatch(10);

// 线程进入等待
coun.await();

// count - 1
coun.countDown()

```