#### 什么分布式锁
在java多线程中，如果不对共享数据进行加锁就会导致数据不一致。我们就会使用synchroiezed来加同步锁。单机情况下可能没有问题，如果是在集群情况下，我们的代码放在不同的服务器，那么同步锁就会没用了。

#### 场景
1.在秒杀系统中我们将库存预热到redis中，redis是分布式。假设有三个线程同时去不同的redis节点去读数据，就都会拿到一样的库存，就会出现超卖。
2.在商城的下订单过程中，如果我重复提交表单，就会出现重复下单。


#### zookeeper 

使用zookeeper节点名称唯一的特性，可以实现分布式锁。我们在进行库存扣减的时候创建一个节点，如果没有报错就继续执行扣减库存的过程，反之就将线程阻塞，然后通过一个自旋锁（死循环）的方式对线程唤醒。

##### 死锁

如果使用普通节点，当服务器挂了或者网络波动的时候，就会造成死锁。可以使用zookeeper临时节点的特性来解决死锁的问题。

##### 监听

我们在自旋锁中使用zookeeper的监听删除节点来唤醒其他线程，但是如果当前等待被唤醒的线程有很多，那么就会一起来重新竞争这个锁，就会导致资源浪费。我们可以使用zookeeper的临时顺序节点来解决这个问题。
在设置监听节点的时候只监听最新的节点，而不是一个固定的节点，这样每次就只会消费一个节点。就是我们在拿锁失败后创建一个临时有序节点，下一个再来拿锁就拿的是上一个拿锁失败的节点。


#### redis

redis分布式锁时使用key的唯一性和设置过期时间来完成，setnx