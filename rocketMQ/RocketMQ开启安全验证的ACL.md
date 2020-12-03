## RocketMQ开启ACL安全验证

1.首先在broker.conf增加参数：

```shell
aclEnable=true
```

2.将源码目录下的distribution/conf/plain_acl.yml复制到rocketMQ conf目录下(新版本不需要复制)

3.打开plain_acl.yml文件修改:

```shell
- accessKey: RocketMQ
  secretKey: 12345678
```

