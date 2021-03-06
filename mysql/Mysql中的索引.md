我们一提mysql的优化，首先就会想到索引，但是索引是什么，他的具体结构和原理就不定清楚了，我简单谈一下我自己对mysql索引的理解。

#### 什么是索引

我们在看书的时候，如果想对某一具体的章节进行查看，就会去目录中找到对应页码就可以了。还有就是我们在查看技术文档的时候右边会有一排目录。目录就是我们日常生活中使用的索引。

#### mysql中的锁索引

mysql中的索引就是帮助我们快速找到需要的数据，我们假设一个表没有索引，那就需要一行一行去匹配，这就是全表扫描。

#### B+tree

b+tree是mysql中的一种索引结构，结构图如下

[![DbcY0U.jpg](https://s3.ax1x.com/2020/12/04/DbcY0U.jpg)](https://imgchr.com/i/DbcY0U)

根节点下面的节点称为子节点，最底层一排节点我们称之为叶子结点。

子节点中存放着我们的索引值，比如主键id。

叶子结点在innodb存储引擎中，存储了主键id外还存储了整行的值。在myisam中存放着主键id和行号。（聚簇索引和非聚族索引）

#### 聚簇索引
聚簇索引是将索引行的数据都存在叶子结点中，在innodb引擎中所有表在创建的时候必须要有一个主键，如果主动指定了就用指定的（primary key），没有则会寻找一个unique唯一索引作为主键，如果没有唯一索引就创建一个rowid来当索引。
所以innodb表天生自带聚簇索引。

#### 二级索引
在我们将表中的字段设置为索引的时候，又会根据字段值新键一个索引结构，子节点中存储索引的值，叶子结点中存储了索引的id（就是主键id）

#### 组合索引
创建组合索引后，也会创建一个新的索引结构。子节点中存储了该组合的所有值，子节点中存储索引id

#### 回表
当我们将id+邮箱+手机号设置成组合索引的时候，在查询的时候如果正好只要这三个字段的信息，那就会直接从组合索引中查出来，而不需要去聚簇索引再查找一次, 反之就叫回表。
这就是我们说为什么不要用 select * 去查询数据。

#### B+tree 在线可视化工具 
<https://www.cs.usfca.edu/~galles/visualization/BPlusTree.html>