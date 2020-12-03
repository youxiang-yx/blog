## Linux常用命令

##### 同步文件命令

```shell
//同步本地文件至远程服务器
[root@VM-0-4-centos tool]rsync -avz /var/tool/apache-maven-3.6.3-bin.tar.gz root@服务器IP:/var/tool/
//同步远程文件至本地
[root@VM-0-4-centos tool]rsync -avz --progress --partial root@服务器IP:/var/tool/apache-maven-3.6.3-bin.tar.gz ./

```

##### 系统常用命令

````shell
//查看端口是否被使用
[root@VM-0-4-centos tool]lsof -i:端口
//搜索进程是否启动
[root@VM-0-4-centos tool]ps -aux |grep 进程名字
````







