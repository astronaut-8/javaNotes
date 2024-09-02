**RabbitMQ - 高性能异步通信组件**



![截屏2024-08-12 22.44.26](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-12%2022.44.26.png)

# 同步调用

![截屏2024-08-13 11.01.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-13%2011.01.22.png)

**同步调用优势**

- **时效性强**，等待到结果后才返回

**同步调用劣势**

- **拓展性差**，每次增加新需求要不断修改源码
- **性能下降**，等待所有业务执行结束，耗时长
- **级联失败问题**，事物，一个业务执行失败，整个事物失败

# 异步调用

![截屏2024-08-13 11.06.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-13%2011.06.31.png)

![截屏2024-08-13 11.10.14](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-13%2011.10.14.png)

**不管怎么增加需求，原业务要做的只有发布消息通知   使得代码耦合度大大降低  拓展性强**



**异步调用的问题**

- **不能立即得到调用结果，时效性差**
- **不确定下游业务是否执行成功**
- **业务安全依赖于Broker(消息代理)的可靠性**

# MQ技术选型

**MQ - MessageQueue - 消息队列**

![截屏2024-08-13 11.17.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-13%2011.17.29.png)

# MQ基础

## rabbitMQ简介安装

安装文档

https://b11et3un53m.feishu.cn/wiki/OQH4weMbcimUSLkIzD6cCpN0nvc

![截屏2024-08-13 21.19.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-13%2021.19.06.png)

**publisher** 发送消息到 交换机

**交换机转发消息到队列**

消费者监听**queue**

整个**RabbitMQ**中有多个相互独立的**Virtual-host** (相当于数据库中的数据表) 多个业务共用的中间键

## rabbitMQ快速入门

- **交换机路由和转发消息，本身没有存储消息的能力**
- **用交换机发个消息，没有route到que ， 消息就丢失了** 
- **交换机去绑定队列**

## rabbitMQ数据隔离

- **有用户之分 每个用户可以有自己的虚拟主机 - 用某个身份登录 再做vitrual-host的增删操作 无法访问权限外的虚拟主机**

- **用户之间没有影响**

## Java客户端

### 快速入门

消息队列协议

![截屏2024-08-19 22.21.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-19%2022.21.09.png)

![截屏2024-08-19 22.24.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-19%2022.24.48.png)

![截屏2024-08-19 22.25.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-19%2022.25.35.png)

![截屏2024-08-19 22.25.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-19%2022.25.53.png)

![截屏2024-08-19 22.26.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-19%2022.26.44.png)

![截屏2024-08-21 22.00.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-21%2022.00.53.png)

![截屏2024-08-21 22.08.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-21%2022.08.12.png)

### WorkQueue

**让多个消费者绑定到同一个队列，共同消费队列中的信息**

每个消息被消费一次 轮询

**比如 不使用preFetch 一开始就轮询把任务都分配给了两个消费者，1357.。。 和 2468.。。。**

**但是由于不同消费者处理能力有限，会出现消息堆积的情况 -- 消费者一全部处理完了，消费者2比较慢，最后独自吧自己的消息统一处理完**

![截屏2024-08-21 23.04.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-21%2023.04.25.png)

![截屏2024-08-21 22.44.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-21%2022.44.09.png)

**Work模型的使用**

- 多个消费者绑定一个队列，加快消息处理速度
- 同一条消息只会被一个消费者处理
- 通过设置prefetch来控制消费者**预取**的消息数量，处理完一条再处理下一条，实现能者多劳



### Fanout交换机

**一个事件往往对应多个服务 如果直接把消息发送到队列，被消费完后就不存在了 无法实现多个对应多个服务 所以采用 交换机**

（多个消费者对应 项目的集群模式 增加处理效率 都是同一个东西）

**Fanout是广播模式**

![截屏2024-08-26 22.23.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-26%2022.23.16.png)



将队列和交换机绑定后，向交换机发送消息

```java
@RabbitListener(queues = "fanout.queue1")
public void listenFanoutQueue1(String msg) {  // 发送消息的type
    System.out.println("fanout.queue1 message -> : " + msg);
}
@RabbitListener(queues = "fanout.queue2")
public void listenFanoutQueue2(String msg) {  // 发送消息的type
    System.out.println("fanout.queue2 message -> : " + msg);
}
```

```java
@Test
void testSendFanout() {
    String exchangeName = "hmall.fanout";
    String msg = "hello everyone";
    rabbitTemplate.convertAndSend(exchangeName ,null , msg);
}
```



### Direct交换机

**定向路由**

![截屏2024-08-27 21.26.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-27%2021.26.25.png)

一个交换机可以和队列有多个不同routingKey的binding

![截屏2024-08-27 21.33.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-27%2021.33.43.png)

```java
@Test
void testSendDirect() {
    String exchangeName = "hmall.direct";
    String msg = "hello everyone";
    rabbitTemplate.convertAndSend(exchangeName ,"yellow" , msg);
}
```

发送消息要指定 routingkey

### Topic交换机

![截屏2024-08-27 21.39.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-27%2021.39.06.png)

![截屏2024-08-27 21.45.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-27%2021.45.53.png)

### 声明队列交换机

![截屏2024-08-27 21.48.07](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-27%2021.48.07.png)

```java
@Configuration
public class FanoutConfiguration {

    @Bean
    public FanoutExchange fanoutExchange() {
       // ExchangeBuilder.fanoutExchange("").build();
        return new FanoutExchange("hmall.fanout2");
    }
    @Bean
    public Queue queue() {
        return new Queue("fanout.queue3");
    }
    @Bean
    public Binding binding(Queue queue, FanoutExchange fanoutExchange) {
       //如果是direct模式指定routingKey 使用with 但是一次只能指定一个
        return BindingBuilder.bind(queue).to(fanoutExchange);
    }
}
```

**使用注解方式 简化代码 - 在监听器的位置**

```java
@RabbitListener(bindings = @QueueBinding(
        value = @Queue(name = "direct.queue3" , durable = "true"),
        exchange = @Exchange(name = "hmall.direct3" , type = ExchangeTypes.DIRECT),
        key = {"red" , "yellow"}
))
public void listenDirectQueue1(String msg) { 
    System.out.println("direct.queue1 message -> : " + msg);
}
```

### 消息转换器

![截屏2024-08-27 22.32.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-27%2022.32.54.png)

![截屏2024-08-27 22.33.21](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-27%2022.33.21.png)

 

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
</dependency>
```

```java
@Bean
public MessageConverter messageConverter() {
    return new Jackson2JsonMessageConverter();
}
```

## 改造业务代码

![截屏2024-08-28 22.58.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-28%2022.58.11.png)

**给需要做消息队列的模块加上AMQP依赖**

```xml 
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

**在总体的common模块中加入MessageConverter(amqp)的bean**

```java
@Configuration
public class MqConfig {
    @Bean
    public MessageConverter messageConverter() {
        return new Jackson2JsonMessageConverter();
    }
}
```

```java
// 5.修改订单状态
//tradeClient.markOrderPaySuccess(po.getBizOrderNo()); - 改为异步调用
try {
    rabbitTemplate.convertAndSend("pay.topic" , "pay.success" , po.getBizOrderNo());
} catch (AmqpException e) {
    log.error("支付成功，但是通知交易服务失败");
}
```

```java
@Component
@RequiredArgsConstructor
public class PayStatusListener {
    private final IOrderService orderService;
    @RabbitListener(bindings = @QueueBinding(
            value = @Queue(name = "mark.order.pay.queue" , durable = "true") ,
            exchange = @Exchange(name = "pay.topic" , type = ExchangeTypes.TOPIC ) ,
            key = "pay.success"
    ))
    public void listenOrderPay(Long orderId) {
        //标记订单状态已支付
        orderService.markOrderPaySuccess(orderId);
    }
}
```

# MQ高级

**保证消息可靠性**

## 生产者可靠性

### 生产者重连

这是**连接**的情况

![截屏2024-08-30 21.49.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-30%2021.49.50.png)

**注意是阻塞式的 有损性能**

### 生产者确认

![截屏2024-08-30 21.56.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-30%2021.56.05.png)

**出现Publisher Return 的原因大多是 交换机和队列 未能建立连接 或者routing key 出现问题**



具体使用 = 

![截屏2024-08-30 21.58.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-30%2021.58.50.png)

**每一个RabbitTemplate只能配置一个ReturnCallback 因此需要在项目启动过程中配置 （也可以使用autowire）**

一般不会出现这种情况

```java
@Slf4j
@Configuration
public class MqConfirmConfig implements ApplicationContextAware {
    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        RabbitTemplate rabbitTemplate = applicationContext.getBean(RabbitTemplate.class);
        rabbitTemplate.setReturnsCallback(new RabbitTemplate.ReturnsCallback() {
            @Override
            public void returnedMessage(ReturnedMessage returnedMessage) {
                log.debug("收到消息 return callback - {} " , returnedMessage.getMessage().toString());
            }
        });
    }
}
```

**每次发消息指定Confirm Callback** 每个消息流程逻辑的消息确认机制 遇到nack 需要重发消息

```java
void testConfirmCallback() {
    //创建cd
    CorrelationData cd = new CorrelationData(UUID.randomUUID().toString()); // 每一个confirm callback的识别标志
    //添加confirm callback
    cd.getFuture().addCallback(new ListenableFutureCallback<CorrelationData.Confirm>() {
        @Override
        public void onFailure(Throwable ex) {
            log.error("消息回调失败",ex); //spring中的错误 跟消息队列无关
        }

        @Override
        public void onSuccess(CorrelationData.Confirm result) {
            log.info("收到confirm callback 回执");
            if (result.isAck()) {
                //消息发送成功
                log.debug("消息发送成功，收到ack");
            }else {
                //消息发送失败
                log.error("消息发送失败，收到nack");
            }
        }
    });

    rabbitTemplate.convertAndSend("hmall.direct" , "red" , "hello" , cd);
}
```

![截屏2024-08-30 22.37.07](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-30%2022.37.07.png)

## MQ可靠性

![截屏2024-08-30 22.42.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-30%2022.42.41.png)

**第二种情况下，mq在把部分数据由内存转到磁盘的这个过程 会阻塞**

### 数据持久化

**交换机和队列在创建的时候 ，可以选择是否为持久化(mq重启后是否还会存在) spring声明队列交换机时默认为持久化创建**



**消息持久化** - 改变msg的**delivery mode**

![截屏2024-08-30 22.46.37](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-30%2022.46.37.png)

**选择消息持久化后 ，每一条消息都会现在内存中存储 再直接同步到磁盘中 当内存写满的瞬间，会清空一部分内存 这个过程会导致mq性能下降 但是不会出现 page out的阻塞情况**

### Lazy Queue

![截屏2024-08-30 22.56.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-30%2022.56.34.png)

![截屏2024-08-30 22.57.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-30%2022.57.09.png)

![截屏2024-08-30 22.57.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-30%2022.57.41.png)

![	](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-30%2022.59.55.png)

## 消费者可靠性

### 消费者确认机制

![截屏2024-08-31 23.07.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-31%2023.07.45.png)

**reject - 消息数据格式异常**



SpringAMQP的消息确认功能

![截屏2024-08-31 23.09.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-31%2023.09.48.png)

![截屏2024-08-31 23.10.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-31%2023.10.23.png)

**当出现异常 消息重新投递nack 会一直循环重复 直到ack**

### 消费失败处理

这里是在listener中 区别于**生产者重连**

![截屏2024-08-31 23.18.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-31%2023.18.06.png)

**重试失败后**

![截屏2024-08-31 23.22.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-31%2023.22.27.png)

```java
@Configuration
@ConditionalOnProperty(prefix = "spring.rabbitmq.listener.simple.retry" , name = "enable" , havingValue = "true") // 当满足条件
public class ErrorConfiguration {
    @Bean
    public DirectExchange errorExchange() {
        return new DirectExchange("error.direct");
    }
    @Bean
    public Queue errorQueue() {
        return new Queue("error.queue");
    }
    @Bean
    public Binding errorBinding() {
        return BindingBuilder.bind(errorQueue()).to(errorExchange()).with("error");
    }

    @Bean
    public MessageRecoverer messageRecoverer(RabbitTemplate rabbitTemplate) {
        return new RepublishMessageRecoverer(rabbitTemplate , "error.direct" , "error");
    }
}
```

![截屏2024-08-31 23.36.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-31%2023.36.39.png)

### 业务幂等性

**幂等概念**

![截屏2024-08-31 23.41.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-31%2023.41.46.png)

由于各种问题 同一条消息可能会被多次消费 如果业务不满足幂等 将会出现大问题

#### 唯一消息id

![截屏2024-08-31 23.44.33](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-31%2023.44.33.png)

增加了业务负担(根据id 保存 判断是否重复 性能下降)



#### 业务判断

![截屏2024-08-31 23.50.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-31%2023.50.58.png)

```java
@Component
@RequiredArgsConstructor
public class PayStatusListener {
    private final IOrderService orderService;
    @RabbitListener(bindings = @QueueBinding(
            value = @Queue(name = "mark.order.pay.queue" , durable = "true") ,
            exchange = @Exchange(name = "pay.topic" , type = ExchangeTypes.TOPIC ) ,
            key = "pay.success"
    ))
    public void listenOrderPay(Long orderId) {
        //1 查询订单
        Order order = orderService.getById(orderId);
        //2 判断订单是否为未支付
        if (order == null || order.getStatus()!= 1) {
            //订单不存在 或者状态异常
            return;
        }
        //标记订单状态已支付
        orderService.markOrderPaySuccess(orderId);
    }
}
```

**使用sql语句优化代码 ：**

![截屏2024-08-31 23.56.59](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-31%2023.56.59.png)

![截屏2024-09-01 00.01.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-01%2000.01.54.png)

## 延迟消息

**生产者发送消息时指定一个时间，消费者不会立刻收到消息，而是在指定时间后才收到消息**

定时任务 - 效率 定时时间不好把控

![截屏2024-09-01 15.59.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-01%2015.59.17.png)

### 死信交换机

**死信**

![截屏2024-09-01 16.17.26](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-01%2016.17.26.png)

如果**队列**通过**dead-letter-exchange**属性指定一个交换机

那么该队列中的死信就会投递到这个交换机中。这个交换机就是**死信交换机**   **Dead Letter Exchange，简称DLX**

![截屏2024-09-01 16.21.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-01%2016.21.16.png)

![截屏2024-09-01 16.23.22](/Users/sunnyday/Library/Application Support/typora-user-images/截屏2024-09-01 16.23.22.png)

```java
@Test
void testSendTTLMessage() {
    Message message = MessageBuilder
            .withBody("hello".getBytes(StandardCharsets.UTF_8))
            .setExpiration("1000")
            .build();
    rabbitTemplate.convertAndSend("simple.direct" , "hi" , message);
}
```

```java
@Test
void testSendTTLMessage() {
    rabbitTemplate.convertAndSend("simple.direct", "hi", "hello", new MessagePostProcessor() {
        @Override
        public Message postProcessMessage(Message message) throws AmqpException {
            message.getMessageProperties().setExpiration("10000");
            return message;
        }
    });
}
```

### 延迟消息插件

**插件安装教程**

https://b11et3un53m.feishu.cn/wiki/A9SawKUxsikJ6dk3icacVWb4n3g

把交换机指定成允许 delay

在msg中设定 delay时间

```Java
@RabbitListener(bindings = @QueueBinding(
        value = @Queue(name = "delay.queue", durable = "true"),
        exchange = @Exchange(name = "delay.direct", delayed = "true"),
        key = "delay"
))
public void listenDelayMessage(String msg){
    log.info("接收到delay.queue的延迟消息：{}", msg);
}
```

```Java
package com.itheima.consumer.config;

import lombok.extern.slf4j.Slf4j;
import org.springframework.amqp.core.*;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Slf4j
@Configuration
public class DelayExchangeConfig {

    @Bean
    public DirectExchange delayExchange(){
        return ExchangeBuilder
                .directExchange("delay.direct") // 指定交换机类型和名称
                .delayed() // 设置delay的属性为true
                .durable(true) // 持久化
                .build();
    }

    @Bean
    public Queue delayedQueue(){
        return new Queue("delay.queue");
    }
    
    @Bean
    public Binding delayQueueBinding(){
        return BindingBuilder.bind(delayedQueue()).to(delayExchange()).with("delay");
    }
}
```

```Java
@Test
void testPublisherDelayMessage() {
    // 1.创建消息
    String message = "hello, delayed message";
    // 2.发送消息，利用消息后置处理器添加消息头
    rabbitTemplate.convertAndSend("delay.direct", "delay", message, new MessagePostProcessor() {
        @Override
        public Message postProcessMessage(Message message) throws AmqpException {
            // 添加延迟消息属性
            message.getMessageProperties().setDelay(5000);
            return message;
        }
    });
}
```

**延迟消息插件内部会维护一个本地数据库表，同时使用Elang Timers功能实现计时。如果消息的延迟时间设置较长，可能会导致堆积的延迟消息非常多，会带来较大的CPU开销，同时延迟消息的时间会存在误差。**

**因此，不建议设置延迟时间过长的延迟消息。**

### 取消超时订单

![截屏2024-09-01 21.03.10](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-01%2021.03.10.png)

![截屏2024-09-01 21.04.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-01%2021.04.34.png)

### 发送延迟检测订单消息

将消息做封装 包含消息本身 和 剩下的过期时间

```java
public class MultiDelayMessage <T>{
    private T data; // 消息体
    private List<Long> delayMillis;

    public MultiDelayMessage(T data, List<Long> delayMillis) {
        this.data = data;
        this.delayMillis = delayMillis;
    }

    public static <T> MultiDelayMessage<T> of(T data, Long ... delayMillis) {
        return new MultiDelayMessage<>(data , CollUtils.newArrayList(delayMillis));
    }

    public Long removeNextDelay() {
        return delayMillis.remove(0); // 获取并移除下一个延迟时间
    }

    public boolean hashNextDelay() {
        return !delayMillis.isEmpty();
    }
}
```

```java
// 延迟检查交换机
MultiDelayMessage<Long> msg = MultiDelayMessage.of(order.getId(), 1000L, 1000L, 2000L);
rabbitTemplate.convertAndSend("trade.delay.topic", "delay", msg, new MessagePostProcessor() {
    @Override
    public Message postProcessMessage(Message message) throws AmqpException {
        message.getMessageProperties().setDelay(msg.removeNextDelay().intValue());
        return message;
    }
});
return order.getId();
```

### 监听延迟消息

 

```java
@Component
@RequiredArgsConstructor
public class OrderStatusCheckListener {
    private final IOrderService orderService;
    private final RabbitTemplate rabbitTemplate;
    @RabbitListener(bindings = @QueueBinding(
            value = @Queue(value = "delay.queue" , durable = "true" ) ,
            exchange = @Exchange(value = "delay.topic" , delayed = "true" , type = ExchangeTypes.TOPIC),
            key = "delay"
    ))
    public void listenOrderDelayMessage( MultiDelayMessage<Long> msg) {
        //查询订单状态
        Order order = orderService.getById(msg.getData());
        //判断是否已经支付
        if (order == null || order.getStatus() == 2) {
            //订单不存在或者已经被处理了
            return;
        }
        //去支付服务查询真正的支付状态(订单状态是mq异步修改，可能还在交换机队列中)
        //TODO 远程调用支付服务获取支付状态
        boolean isPay = false;
        //已支付，标记订单状态为已支付
        if (isPay) {
            orderService.markOrderPaySuccess(order.getId());
            return;
        }
        //判断是否存在延迟时间
        if (msg.hashNextDelay()) {
            //存在，重发延迟消息
            Long nextDelay = msg.removeNextDelay();
            rabbitTemplate.convertAndSend("trade.delay.topic", "delay", msg, new MessagePostProcessor() {
                @Override
                public Message postProcessMessage(Message message) throws AmqpException {
                    message.getMessageProperties().setDelay(nextDelay.intValue());
                    return message;
                }
            });
            return;
        }
        //TODO 将取消订单和恢复库存共同抽取到一个service方法中，使用分布式事务
        //不存在，取消订单
        orderService.lambdaUpdate()
                .set(Order::getStatus , 5)
                .set(Order::getCloseTime , LocalDateTime.now())
                .eq(Order::getId , order.getId())
                .update();
        //TODO恢复库存
    }
}
```
