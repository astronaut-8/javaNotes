# 分布式系统相关概念

## 互联网项目架构目标

### 特点

![截屏2024-10-23 09.18.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-23%2009.18.50.png)

**互联网项目特点**

- 用户多(用户0容忍)
- 流量大，并发高
- 海量数据
- 易受攻击
- 功能繁琐
- 变更快

### 目标

- **高性能** - 提供快速的访问体验

- 响应时间 - 请求从开始到最后收到相应数据所花费的总体时间
- 并发数 - 系统同时能处理的请求数量
- 并发连接数 - 客户端向服务器发起请求，建立TCP连接，每秒钟服务器连接的总TCP数量
- 请求数 - QPS (Query Per Second) 每秒多少请求(请求建立可以发送多次请求)
- 并发用户数 - 单位时间有多少用户
- 吞吐量 - 单位时间内系统能处理的请求数量

- QPS -  (Query Per Second)  每秒查询数
- TPS - （Transactions Per Second） 每秒事物数(一个客户机向服务器发送请求然后服务器作出反应的过程，客户机在发送请求时开始计时，收到服务器响应后结束计时，计算使用的时间和完成的事物个数)

![截屏2024-10-23 09.30.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-23%2009.30.41.png)

- 高可用 - 网站服务一直可以正常访问
- 可伸缩 - 通过硬件增加减少， 提高降低处理能力
- 高可拓展 - 系统间耦合度低，方便通过新增移除的方式，增加减少新的功能模块
- 安全性 - 提供网站安全访问和数据加密，安全存储等策略
- 敏捷性 - 随需应变，快速响应



## 集群和分布式

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-23%2009.40.17.png" alt="截屏2024-10-23 09.40.17" style="zoom: 33%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-23%2009.42.14.png" alt="截屏2024-10-23 09.42.14" style="zoom: 50%;" />

![截屏2024-10-23 09.42.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-23%2009.42.43.png)

## 架构演进

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-23%2009.45.16.png" alt="截屏2024-10-23 09.45.16" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-23%2009.46.49.png" alt="截屏2024-10-23 09.46.49" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-23%2009.49.20.png" alt="截屏2024-10-23 09.49.20" style="zoom:50%;" />

![截屏2024-10-23 09.52.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-23%2009.52.25.png)

![截屏2024-10-23 09.54.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-23%2009.54.35.png)

![截屏2024-10-23 09.56.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-23%2009.56.23.png)

**Dubbo - SOA**

**SpringCloud - 微服务**

# Dubbo概述

## 概念 

属于SOA

![截屏2024-10-23 16.34.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-23%2016.34.28.png)



## 架构 

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-23%2016.35.53.png" alt="截屏2024-10-23 16.35.53" style="zoom: 50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-23%2016.37.39.png" alt="截屏2024-10-23 16.37.39" style="zoom:50%;" />



# Dubbo快速入门

 **安装zookeeper**(参见zookeeper的笔记)

## Spring和SpringMVC整合

![截屏2024-10-23 19.00.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-23%2019.00.06.png)

项目名称dubbo - pro

记录：

service层 dubbo有个注解名字他妈跟spring的一毛一样 - @Service 死活找不到报错 无法把bean加入到容器，服了

## 服务提供者

首先项目一直报错，这个dubbo zookeeper的版本于jdk的兼容问题还是挺烦的

在maven中手动指定了jdk版本就可以解决版本问题?

```java
@Service //dubbo的@Service 将这个类提供的方法(服务) 对外发布，将访问的地址，ip 端口，路径注册到注册中心
public class UserServiceImpl implements UserService {

    @Override
    public String sayHello() {
        return "hello world";
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo" xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

<!--    dubbo配置-->
<!--    配置项目名称，唯一-->
    <dubbo:application name="dubbo-service"/>
<!--    配置注册中心(zookeeper)地址-->
    <dubbo:registry address="zookeeper://116.198.203.111:2181"/>
<!--       配置dubbo包扫描-->
    <dubbo:annotation package="com.itheima.service.impl"/>
</beans>
```



## 服务消费者

启动 服务提供者 和 服务消费者两个 tomcat应用 其dubbo 的 **监控**(qos)都会占用22222端口，导致一个应用产生端口占用的报错

实际环境下，应用部署在不同的服务器上，就不会发生端口报错，也可以通过**手动指定端口位置**

![截屏2024-10-24 10.27.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-24%2010.27.11.png)

```xml
<mvc:annotation-driven/>
<context:component-scan base-package="com.itheima.controller"/>
<!--    dubbo配置-->
<!--    配置项目名称，唯一-->
<dubbo:application name="dubbo-web">
    <dubbo:parameter key="qos.port" value="33333"/>
</dubbo:application>
<!--    配置注册中心(zookeeper)地址-->
<dubbo:registry address="zookeeper://116.198.203.111:2181"/>
<!--       配置dubbo包扫描-->
<dubbo:annotation package="com.itheima.controller"/>
```

```java
@RestController
@RequestMapping("/user")
public class UserController {


    // 注入service
    //@Autowired 本地注入
    /**
     * 1 ,从zookeeper注册中心获取userService的访问Url
     * 2 ，进行远程调用rpc
     * 3 ，将结果封装为一个代理对象，给变量赋值
     */
    @Reference
    private UserService userService;

    @RequestMapping("/sayHello")
    public String sayHello() {
        return userService.sayHello();
    }
    @RequestMapping("/say")
    public String say() {
        return "hello";
    }

}
```





 

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-24%2010.34.27.png" alt="截屏2024-10-24 10.34.27" style="zoom:50%;" />



**写一个interface的接口模块，消费者和服务提供者都统一使用它的接口，接口统一定义**

# Dubbo高级特性

## dubbo admin

 <img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-24%2010.48.59.png" alt="截屏2024-10-24 10.48.59" style="zoom:50%;" />

**放弃了，这吊前端根本打不开**

**启动前端工程**

![截屏2024-10-24 11.33.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-24%2011.33.31.png)

**查询服务**

![截屏2024-10-24 16.25.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-24%2016.25.40.png)

**除了tomcat占用的端口，服务本身还占用一个端口**

![截屏2024-10-24 16.26.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-24%2016.26.49.png)

加入配置开启元数据

![截屏2024-10-24 16.28.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-24%2016.28.16.png)



## dubbo常用高级配置

### 序列化

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-24%2016.47.55.png" alt="截屏2024-10-24 16.47.55" style="zoom:33%;" />

- dubbo内部封装了序列化和反序列化
- 需要在pojo类实现**Serializable**接口

- 定义一个公共pojo模块，让生产者和消费者都依赖这个模块



### 地址缓存

![截屏2024-10-24 16.58.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-24%2016.58.06.png)



### 超时

![截屏2024-10-24 17.00.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-24%2017.00.19.png)

**dubbo利用超时机制来解决这个问题，设置一个超时时间，在这个时间段内，无法完成服务访问，则自动断开连接**

**使用timeout设置超时时间，默认值1000，单位ms**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-24%2017.15.37.png" alt="截屏2024-10-24 17.15.37" style="zoom:50%;" />

**reference也可以设置超时时间，而且优先级更高，但是最好配置在服务的提供者**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-24%2017.18.46.png" alt="截屏2024-10-24 17.18.46" style="zoom:50%;" />

### 重试

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-24%2017.21.33.png" alt="截屏2024-10-24 17.21.33" style="zoom:50%;" />

![截屏2024-10-24 17.22.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-24%2017.22.06.png)

### 多版本

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-24%2017.24.14.png" alt="截屏2024-10-24 17.24.14" style="zoom:50%;" />

```java
@Service(version="v1.0")
```

```java
@Reference(version="v1.0")
```

**在同一个类的不同版本中的service中加入不同的版本区分，在reference中指定调用的版本**

### 负载均衡

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-24%2017.29.00.png" alt="截屏2024-10-24 17.29.00" style="zoom:33%;" />

- **Random:按照权重随机，默认值，按照权重设置随机概率**

`@Service(weight = 100)`

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-24%2017.29.51.png" alt="截屏2024-10-24 17.29.51" style="zoom:33%;" />

集群中实例，变换端口(两个dubbo端口 tomcat端口)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-24%2017.31.53.png" alt="截屏2024-10-24 17.31.53" style="zoom:50%;" />

最后在服务调用方指定负载均衡策略

`@Reference(loadbalance = "random")`





**其他3种**

![截屏2024-10-24 17.39.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-24%2017.39.06.png)

### 集群容错

访问集群中的一个服务器，可能出现错误，访问其他的集群中的服务器的策略

- **Failover Cluster** - 失败重试，默认，当出现失败，重试其他服务器，默认重试2次，使用retries配置，用于读操作

对于写操作，如果访问一个服务器，由于数据库原因访问速度慢，虽然成功插入了数据，但是还是达到了重试标准，就会在其他服务器重新发请求，导致数据重复



`@Reference(cluster = "failover" , retries = 3)`



**广播模式用于数据同步，同步跟新比如bcd数据，使得其数据保持同步**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-24%2017.51.24.png" alt="截屏2024-10-24 17.51.24" style="zoom:50%;" />



### 服务降级

关停一些服务确保核心业务的稳定

![截屏2024-10-24 17.54.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-24%2017.54.50.png)

![截屏2024-10-25 08.47.59](/Users/sunnyday/Library/Application Support/typora-user-images/截屏2024-10-25 08.47.59.png)

# dubbo 总结

**Dubbo 的通信模型分为几个主要组件：**



​	1.	**服务消费者（Consumer）**

​	2.	**服务提供者（Provider）**

​	3.	**注册中心（Registry）**

​	4.	**监控中心（Monitor）**

	5.	**集群容错机制（Cluster）**





**3. 网络通信**



​	•	**传输协议**：Dubbo 支持多种协议，如 Dubbo 协议、RMI、HTTP、Thrift 等。默认情况下，Dubbo 协议使用高效的二进制数据序列化和反序列化方式。

​	•	**Netty 框架**：Dubbo 默认使用 Netty 作为通信框架，Netty 是一个高性能的异步网络通信框架，支持高并发，低延迟。

​	•	**异步通信**：Dubbo 支持异步通信模式，消费者发送请求后不必等待响应，可以立即返回，从而提高系统的响应速度。

Dubbo 默认的协议是基于 TCP 传输的高效二进制协议，支持长连接和异步调用，特别适合高并发的场景





@Spi() 指定默认的接口实现

![截屏2024-10-25 09.16.32](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-25%2009.16.32.png)

![截屏2024-10-25 09.21.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-25%2009.21.12.png)

 ![ ](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-25%2009.22.19.png)
