## RocketMQ配置远程访问

打开broker配置文件

```shell
vim conf/broker.conf
//增加配置参数
namesrvAddr=xxx.xxx.xxx.xxx:9876
brokerIP1=xxx.xxx.xxx.xxx
```

```shell
sh bin/mqnamesrv & //启动nameserver
sh bin/mqbroker -n 127.0.0.1:9876 & //启动broker
```

##### No route info of this topic, TopicTest 错误

这个错误需要在启动broker的时候添加autoCreateTopicEnable=true参数，该参数声明topic不存在就自动创建

```shell
sh bin/mqbroker -n localhost:9876 autoCreateTopicEnable=true &
```

