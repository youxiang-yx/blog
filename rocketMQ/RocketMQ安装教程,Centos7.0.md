## RocketMQ安装教程,Centos7.0

#### 安装

下载地址 https://mirror.bit.edu.cn/apache/rocketmq/4.7.1/rocketmq-all-4.7.1-source-release.zip

```shell
//下载rocketMQ
wget https://mirror.bit.edu.cn/apache/rocketmq/4.7.1/rocketmq-all-4.7.1-source-release.zip 
//解压文件
unzip rocketmq-all-4.7.1-source-release.zip 
cd rocketmq-all-4.7.1-source-release
//编译安装rocketMQ
mvn -Prelease-all -DskipTests clean install -U //编译rocket
//将编译好的rocketMQ移动道/usr/local目录下
mv distribution/target/rocketmq-4.7.1/rocketmq-4.7.1 /usr/local/rocketMQ
```

#### 启动rocketMQ

在启动rocketMQ之前需要修改配置文件，不然有可能会内存不足。

```shell
1.打开bin目录下runbroker.sh文件找到:
JAVA_OPT="${JAVA_OPT} -server -Xms8g -Xmx8g -Xmn4g"
2.打开bin目录下runserver.sh文件找到:
JAVA_OPT="${JAVA_OPT} -server -Xms4g -Xmx4g -Xmn2g -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=320m"
将上面两个参数修改成你自己机器内存能接受的范围。
```

```shell
sh bin/mqnamesrv & //启动nameserver
sh bin/mqbroker -n 127.0.0.1:9876 & //启动broker
```

### 停止rocketMQ
```shell
bin/mqshutdown broker //停止运行broker
bin/mqshutdown namesrv //停止运行namesrc
```

