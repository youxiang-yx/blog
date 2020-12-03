#### 基本概念	

Spring是一个采用分层设计的java框架，解决了业务逻辑和数据层等其它层面的耦合关系，面向接口进行编程。

提供了AOP,IOC,事物管理，JMS等技术支持。

同时spring也有很多子项目，包括数据层，权限管理，微服务（Spring boot/ Spring Cloud）。

#### 官方架构图

[![D4TV6x.png](https://s3.ax1x.com/2020/12/02/D4TV6x.png)](https://imgchr.com/i/D4TV6x)

#### 源码下载

下载地址: <https://github.com/spring-projects/spring-framework/tree/master>

使用IDEA打卡spring框架目录下的build.gradle文件，等他自己安装就行。（最好使用IDEA的最新版本，不然会有各种问题）

创建demo用来测试spring的功能，这样就可以跟踪spring的代码了

[![DTReKK.jpg](https://s3.ax1x.com/2020/12/03/DTReKK.jpg)](https://imgchr.com/i/DTReKK)
[![DTg2lV.md.jpg](https://s3.ax1x.com/2020/12/03/DTg2lV.md.jpg)](https://imgchr.com/i/DTg2lV)

打开build.gradle文件，添加内容(这里)

```
plugins {
    id 'java'
}

group 'org.springframework'
version '5.3.2-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.6.0'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine'
}

test {
    useJUnitPlatform()
}
```

#### 创建一个bean(跟我们的项目一样在src/main/java目录下创建就行)

创建service.impl.CustomerServiceImpl

```java
package service.impl;

import service.CustomerService;

public class CustomerServiceImpl{
	public void create() {
		System.out.println("创建用户");
	}
}

```

resource目录下创建spring-config.xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	   xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean id="customerService" class="service.impl.CustomerServiceImpl" />

</beans>
```

test的目录下创建测试代码

```java
import org.springframework.beans.factory.BeanFactory;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import service.CustomerService;

public class CustomerTest {

	public static void main(String[] args) {
		BeanFactory beanFactory = new ClassPathXmlApplicationContext("spring-config.xml");
		CustomerService customerService = (CustomerService) beanFactory.getBean("customerService");
		System.out.println(beanFactory.containsBean("customerService"));
	}
}
```

运行结果

```
> Task :demo:CustomerTest.main()
创建用户
```