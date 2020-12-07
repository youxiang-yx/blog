spring中事物的管理和数据库基本一致，都是基于ACID来处理事物。使用@Transactional开启事物。但是在spring中新增加了事物的传播级别，比如a方法中调用了b方法，a方法开启了事物，b方法也开启了事物，到底是使用a方法的事物，还是b方法的事物。

#### 事物传播级别

```java
// 如果当前没有事物就开启一个新的事物，如果当前有事物就继续使用该事物
PROPAGATION_REQUIRED

// 如果当前有事物就使用当前事物，如果没有就不开启事物执行
PROPAGATION_SUPPORTS

// 如果当前有事物就使用当前事物，如果没有就抛出异常
PROPAGATION_MANDATORY

// 如果当前有事物就挂起来，创建一个新的事物执行
PROPAGATION_REQUIRES_NEW

// 以非事物方式运行，如果当前有事物就挂起来
PROPAGATION_NOT_SUPPORTED

// 以非事物方式运行，如果当前有事物就抛出异常
PROPAGATION_NEVER

```


