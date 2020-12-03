mysql中的事物保证一组sql对表的修改或者删除操作要么全部功能，要么全部失败。

#### 事物开启方式

```mysql
begin;

  update ...

  delete ...

  select ...

commit; // 成功就提交(commit)，失败就回滚(rollback);
```

#### 事物特性ACID

##### 原子性
一组sql要么全部成功，要么全部失败，不会有成功一半的情况。

##### 一致性
事物操作成功后，数据状态和业务规则必须保持一致。比如购买商品可以使用优惠卷，而优惠卷只能使用一次，下次购买商品该优惠卷就不应该可以继续使用了。

##### 隔离性
当有多个事物对数据进行操作的时候，对数据进行隔离。

##### 持久性
事物提交成功后，必须将数据成功保存到数据库，就算是提交后立马崩溃。这个使用的是数据库的日志功能进行恢复。

#### 事物的隔离级别

##### 读未提交
可以去读到未提交的事物对数据库的修改，一般不用。

##### 读提交
读取事物已经提交的数据，可以优化数据库的并发，对数据一致性不高的业务可以使用这个。

##### 可重复度
读取事物创建时候的数据，只有查询才会读创建时候的数据。在update的时候会是最新的值。

##### 序列化
当事物级别是序列化的时候，不管是读还是写都会被加锁，如果是innodb依然是行级锁。

``` mysql

//设置读未提交级别：
set session transaction isolation level read uncommitted;

//设置读提交级别：
set session transaction isolation level read committed;

//设置可重复读级别：
set session transaction isolation level repeatable read;

//设置serializable级别：
set session transaction isolation level serializable;
```

#### 幻读
1.假设我们开启了两个可重复读事物，对同一条记录的两个字段进行+1操作，当前值是2，两个事物都提交后你会发现变成了4。
2.假设我们开启事物A添加一条id=15的记录，A事物不提交开启事物B,在事物B中修改id=15的记录会被挂起来，此时提交事物A释放了事物B,此时事物B再执行查询操作，就会看到id=15的记录。