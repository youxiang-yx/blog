#### 什么是Docker

Docker是一种容器技术，主要功能是将应用和环境进行打包，然后在另外一台机器上解压就可以运行了。

比如我们有几千台服务器，如果一台一台安装环境和部署，将会要付出很大的人力资源，期间还有可能各种错误，导致最后的结果不尽人意，所以就出现了容器技术来解决这些问题，一次打包，别的机器直接解压运行就可以了。

#### Docker的三大核心概念

镜像(Image)

容器(Container)

仓库(Repositor)

#### Docker安装

##### Centos安装

```shell
//卸载老版本的docker
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine

//使用官方脚本进行安装
curl -sSL https://get.daocloud.io/docker | sh
```

##### Mac安装

```shell
//安装命令
brew cask install docker

//配置镜像, 所有系统通用
{
  "registry-mirrors": [
    "http://hub-mirror.c.163.com/",
    "https://registry.docker-cn.com"
  ],
  "insecure-registries":[],
  "experimental": false,
  "debug": true
}

```

