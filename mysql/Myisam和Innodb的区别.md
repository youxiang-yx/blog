#### Myisam和Innodb的区别

- Myisam在磁盘上索引和数据是分开存储，索引的叶子结点存储的是数据的行号。而Innodb存储的索引和数据存放在一次，叶子结点上存放的是具体的字段数据。所以说myisam是非聚簇索引，innodb是聚簇索引

- Myisam不支持事物,Innodb支持

- Myisam是表级锁,Innodb是行级锁。

- Innodb支持外键，Myisam不支持

- Innodb必须要有一个主键，如果指定就会使用唯一索引做主键，如果没有唯一索引，就会自动创建一个rowid作为主键。