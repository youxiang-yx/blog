#### 下载

```shell
// 去官网下载jdk,有些时候需要登录后下载，可以先下载到本地，然后通过scp命令上传到服务器
https://www.oracle.com/java/technologies/javase-downloads.html
```

##### 安装

````shell
// 解压
tar -zxvf jdk-8u271-linux-x64.tar.gz

// 直接把解压的程序移动到/usr/local目录下
mv jdk1.8.0_271 /usr/local/java

// 添加系统变量
vim /etc/profile

// 添加内容
export JAVA_HOME=/usr/local/java
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH

// 刷新系统变量
source /etc/profile

//添加软连接
ln -s /usr/local/java/bin/java /usr/bin/java

//正常显示版本就安装成功了
java -version 
````







