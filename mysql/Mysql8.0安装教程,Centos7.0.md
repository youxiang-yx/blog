## Mysql8.0安装教程,Centos7.0

#### 安装

本文使用yum方式安装mysql

首先更新一下yum  mysql-server的源至8.0

下载地址 https://repo.mysql.com//mysql80-community-release-el7-1.noarch.rpm

```shell
[root@VM-0-4-centos tool]wget https://repo.mysql.com//mysql80-community-release-el7-1.noarch.rpm //下载mysql8.0的源
[root@VM-0-4-centos tool]rpm -ivh mysql80-community-release-el7-1.noarch.rpm //安装mysql8.0的源
[root@VM-0-4-centos tool]yum install mysql-server //使用yum安装mysql
[root@VM-0-4-centos tool]/bin/systemctl start mysqld.service //启动mysql,不支持这个命令的机器用:service mysqld start
```

#### 连接Mysql

mysql8不再支持无密码登陆，会有一个默认密码

````
[root@VM-0-4-centos tool]grep 'temporary password' /var/log/mysqld.log // 查看默认密码, myi=默认密码
[Note] [MY-010454] [Server] A temporary password is generated for root@localhost: myi=kS+*o1Wr
````

首次登陆无法进行数据库操作，需要先修改默认密码

```
ALTER USER USER() IDENTIFIED BY '新密码' //修改密码命令
```

如果连接数据库用默认密码也报错，那就只能用skip-grant-table

打开my.cnf配置文件，加上:skip-grant-table,就可以不需要密码直接连接数据库了(要停掉数据库后再启动，是用restart会报错)。

````mysql
The MySQL server is running with the --skip-grant-tables option so it cannot execute this statement
//如果碰到这个错误，先停掉数据库再启动
执行以下命令
flush privileges; 
alter user 'root'@'localhost'IDENTIFIED BY '密码';
修改成功后删掉配置文件里的skip-grant-table，再重启
````

#### 添加权限

一般在使用数据库的时候是不会直接使用root账号连接的，把root开放给外部ip访问会有安全因素。所以我们需要创建一个对应库的账号来管理数据库。

```mysql
create user '账号'@'%' identified by '密码'; //创建一个mysql账号，%表示任意ip访问，我们可以修改指定ip进行访问。
GRANT ALL PRIVILEGES ON 库名.* TO '账号'@'%' WITH GRANT OPTION; //设置账号访问权限,all表示对指定库的所有权限。
flush privileges;//刷新mysql库缓存
```

#### Mysql管理常用命令

```mysql
desc 表名; //查看表结构
show create table 表名; //查看建表语句
create database 库名 charset utf8; // 创建数据库 charset 设置表字符集
create table 表名 (字段名 字段类型); // 创建数据库表
alter table 表名 add 字段名 字段类型; // 添加字段
alter table 表名 alter column 字段名 字段类型; // 修改字段信息
alter table 表名 drop column 字段名; // 删除字段
```



