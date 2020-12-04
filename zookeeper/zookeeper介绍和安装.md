#### zookeeper是什么

zookeeper是一种特殊的数据库，存储节点，并有监听通知机制。

#### 节点

zookeeper有4种节点

1.持久化节点 (永远不会消失)
2.持久化有序节点
3.临时节点（在客户端关闭的时候消失）
4.临时有序节点

#### 可以实现的功能

1.服务注册与发现
2.分布式锁
3.分布式管理
4.分布式配置中心

注：zookeeper就是为分布式而生

#### 安装

```shell
//下载
wget https://downloads.apache.org/zookeeper/zookeeper-3.6.2/apache-zookeeper-3.6.2-bin.tar.gz

//解压
tar -zxvf apache-zookeeper-3.6.2-bin.tar.gz

//移动到自己的应用安装目录
mv apache-zookeeper-3.6.2-bin.tar.gz /usr/local

//启动zookeeper
bin/zkServer.sh start

//使用自带客户端工具连接到zookeeper
./bin/zkCli.sh

//查看所有节点
ls /
```