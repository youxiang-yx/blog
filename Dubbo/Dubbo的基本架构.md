duboo是阿里巴巴开源，然后交给apache管理的一个分布式服务框架。

### 框架

provider 服务提供者

consumer 服务消费者,消费者通过dubbo协议调用服务提供者的服务

register 注册中心，服务提供者向注册中心注册自己提供的服务，消费者向注册中心拉取需要的服务
 
mointer 监控dubbo之间的调用次数


### dubbo的服务暴露过程

#### 服务暴露方式
```xml
// 本地暴露 injvm
<dubbo:service scope="local" />
// 远程暴露 默认使用dubbo协议
<dubbo:service scope="remote" />
```
默认情况下两种是一起暴露的


#### 服务的引入流程
服务的引入流程有两种，饿汉氏和懒汉氏。饿汉就是在启动的时候就引入，懒汉就是在服务调用的时候引入，默认是懒汉氏。


#### 服务的调用过程
在调用方法后，会生成代理类，然后由cluster经过负载均衡，选择一个invoker发起远程调用。
远程服务在接收到调用之后去之前暴露的map里找到exporter，然后调用真正的实例类。