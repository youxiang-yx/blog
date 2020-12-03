#### 搜索镜像

```shell
docker search 镜像名称
// 搜索nginx
docker search nginx
```

#### 下载镜像

```shell
docker pull 镜像名称
// 下载nginx镜像
docker pull nginx
```

#### 查看所有镜像

``` shell
docker images
```

#### 删除镜像

```shell
docker rmi 镜像名称
//删除nginx
docker rmi nginx
```

#### 保存镜像

```shel
docker save 镜像名称 > 保存的镜像名称
例
docker save nginx > nginx.tar
```

#### 加载镜像

```shell
docker load < 镜像名称
例
docker load < nginx.tar
```

#### 启动容器

```shell
//以启动nginx为例
docker run -d -p 8080:80 nginx
-d 表示后台运行
-p 表示将容器的80端口暴露给宿主机器的8080端口
http://ip:8080 //访问的就会是nginx服务器了
```

#### 查看当前运行中的容器

```shell
docker ps
```

#### 停止容器

```shell
docker stop 容器ID // 容器ID就是使用docker ps查看到的表格第一个字段的值。
docker kill 容器ID // 强制停止
```

```shell
docker start 容器ID //启动已经停止的容器
docker restart 容器ID  // 重启容器
```

#### 进入容器

```
docker exec -it 容器id /bin/bash
```

导入/导出容器

```she
docker export [OPTIONS] CONTAINER //导出容器
docker import [OPTIONS] file|URL|- [REPOSITORY[:TAG]] //导入容器
```

#### 构建镜像

```shell
docker build -t 自定义镜像名称:版本 文件目录
以打包java8jdk为例:
mkdir /usr/local/docker
mv jdk-8u271-linux-x64.tar.gz /usr/local/docker //将jdk放入docker目录
touch /usr/local/docker/Dockerfile //创建Dockerfile配置文件

//Dockerfile文件的内容
FROM centos:7
MAINTAINER zjk "xiangyou@codeyx.cn"
WORKDIR /usr/local/docker
ADD jdk-8u271-linux-x64.tar.gz /usr/local/docker
ENV JAVA_HOME=/usr/local/docker/jdk1.8.0_271
ENV CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV PATH=$JAVA_HOME/bin:$PATH
CMD ["java","-version"]

```