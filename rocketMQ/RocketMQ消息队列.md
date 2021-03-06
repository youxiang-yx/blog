#### 基本概念

消息队列是解决软件开发过程中代码耦合，高并发场景下削峰和异步执行的解决方案。

##### 异步	

我们在用户注册之后需要发送邮件和手机短信，和一系列站内信和活动赠送等操作。如果全部写在用户注册流程里，那么代码的执行时间就会增加。如果注册只负责主流程，把用户信息同步到数据库和验证，将短信和邮件等功能，放入到另外一个代码块，开启另外一个线程异步执行，就会减少响应时间。如果用开启新线程的方式异步执行（线程池），异步执行失败了就会导致严重的数据一致性问题，这时候就需要消息队列来解决了。

##### 解耦

在复杂的订单下单流程中有很多主流程和子流程，主流程有下单,支付，扣减库存然后完成订单，子流程有优惠卷，积分，短信等操作。如果放在一个代码流程里执行，有一天子流程规则变更了，那么就需要去修改订单相关的代码，如果别的地方也用到了也要跟着改。流程越来越多，如果出现bug，维护起来就会很困难。如果使用消息队列，定位问题和发布就会轻松很多。不需要担心主流程会因为本次修改出现问题。

##### 削峰

在秒杀活动中，瞬间很高的并发，如果不对流量进行限制，那么服务器很可能就会因此挂掉。如果使用消息队列那么我们就可以设置每秒钟消费的消息数量，把更多的消息先堆积起来，慢慢消费。

#### 问题

使用消息队列，也需要处理各种问题：重复消费，消息丢失，分布式事务