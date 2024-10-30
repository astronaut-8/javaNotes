参照https://blog.csdn.net/qsmiley10/article/details/115000474?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522171516959616800222898691%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=171516959616800222898691&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-2-115000474-null-null.142%5Ev100%5Epc_search_result_base3&utm_term=java%20kafka&spm=1018.2226.3001.4187

**部分参数配置省略，可以在文章中查阅**

# Kafka安装

**手动更改zookeeper(集群地址)**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-27%2016.58.32.png" alt="截屏2024-10-27 16.58.32" style="zoom:50%;" />

**log**文件地址

![截屏2024-10-27 17.00.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-27%2017.00.15.png)

`chmod +x /usr/local/kafka/bin/*.sh` // 为kafka bin目录下所有脚本增加启动权限

**配置zooKeeper 的环境变量**

**配置kafka环境变量**

**修改server.properties的log目录**

![截屏2024-10-28 09.50.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-28%2009.50.39.png)

`$KAFKA_HOME/bin/kafka-server-start.sh -daemon $KAFKA_HOME/config/server.properties` //**启动**

`$KAFKA_HOME/bin/kafka-server-stop.sh`//关闭

# Kafka架构

## Broker

- 把kafka的服务叫做**Broker**，默认是**9092**的端口
- 生产者和消费者都需要跟这个Broker**建立一个连接**，才可以实现消息的收发
- 一个部署的kafka**实例**就是一个broker 在集群环境下要配置**broker id**

## 消息

- 客户端之间传输的数据叫做消息，或者叫做**记录**（Record）
- 生产者的封装类是**ProducerRecord**，消费者的封装类是**ConsumerRecord**
- 消息在传输过程中需要**序列化**，所以在代码中要指定**序列化工具**

## 生产者

- 为了提升发送效率，生产者不是逐条发送消息给Broker，而是批量发送的

**参数**(数据大小和时间间隔)：

```
batch.size=16384
linger.ms=0
```

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-28%2016.39.40.png" alt="截屏2024-10-28 16.39.40" style="zoom:50%;" />

## 消费者

—般来说消费者获取消息有两种模式，一种是**pull**模式，一种是**push**模式。

Pull模式就是**消费放在Broker**,消费者**自己决定**什么时候去获取。

Push模式是消息放在**Consumer**，**只要**有消息到达Broker,都直接**推给**消费者。

Kafka只支持pull模式,在pull模式下消费者一次pull获取多少条消息由以下参数决定

```
max.poll.records=500
```

(这个配置命令行中无法使用，只能在java客户端中)

## topic

一组消息的集合（用以区分不同业务用途的消息）

注意，生产者发送消息时，如果Topic不存在，会自动创建。由以下参数决定：

```java
auto.create.topics.enable=true
```

## partition

把一个Topic进行拆分。 Kafka引入了一个分区(Partition)的概念。一个Topic可以划分成多个分区。 分区在创建topic的时候指定，每个topic至少有一个分区。
  在创建topic时需要指定分区数由以下参数决定：

```java
num.partitions=1
```

## replica

如果partition的数据只存储一份，在发生网络或者硬件故障的时候，该分区的数据就无法访问或者无法恢复了。
  Kafka在0.8的版本之后增加了副本机制。每个partition可以有若干个副本(Replica)，副本必须在不同的Broker 上面。—般我们说的副本包括其中的主节点。由 replication-factor指定一个Topic 的副本数。默认副本数由以下参数决定

```
offsets.topic.replication.factor=1
```

![截屏2024-10-28 16.43.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-28%2016.43.54.png)

## segment

Kafka的数据是放在**后缀.log**的文件里面的。如果一个partition只有一个log文件，**消息不断地追加**，这个log文件也会变得越来越大，这个时候要**检索数据效率**就很低了

把**partition**再做一个切分，切分岀来的单位就叫做段（**segment**）

每个**segment**都有至少有**1个数据文件和2个索引文件**，这3个文件是**成套**出现的。

## consumer group

Kafka引入了消费者组Consumer Group的概念，通过group.id来配置，group.id相同的消费者属于同一个消费者组，Kafka对不同消费者组进行广播，而同一个消费者组中的消费者只消费一次消息。
  具体点说，**同一个消费者组的消费者不能订阅相同的分区**。当消费者数量小于分区数时，一个消费者消费多个分区，当消费者数量大于分区数时，多余的消费者将不进行工作。

## consumer offset

我们已经知道Kafka消息是写在日志文件中，并且**不会在消费过后就被删除**。那么当一个消费者组在消费一半时重启了，该如何继续上一次的位置读取消息呢？为此，Kafka引入Consumer Offset的概念。
  Consumer Offset是标记一个消费者组在一个partition即将消费的下一条记录，这个信息直接保存在Kafka本身一个特殊的topic中，叫__consumer_offsets，默认创建50个分区。

# Kafka使用

## 脚本

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-28%2015.08.50.png" alt="截屏2024-10-28 15.08.50" style="zoom:67%;" />

## --zookeeper

![截屏2024-10-28 15.47.24](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-28%2015.47.24.png)

![截屏2024-10-28 15.54.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-28%2015.54.39.png)

## 创建topic

```
#--replication-factor 创建的副本数,这个使用来备份的.副本数不能大于broker数
#--partitions 2 创建的分区数.根据实际情况创建
#--topic testDemo topic-name
#--bootstrap-server 参数直接与 Kafka Broker 通信 创建主题
./kafka-topics.sh --create --bootstrap-server 116.198.203.111:9092 --replication-factor 1 --partitions 2 --topic testDemo
```

![截屏2024-10-28 15.58.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-28%2015.58.02.png)

## 查看topic

```
./kafka-topics.sh --list --bootstrap-server 116.198.203.111:9092
```

![截屏2024-10-28 15.58.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-28%2015.58.56.png)

## 查看topic详细信息

```
./kafka-topics.sh --describe --bootstrap-server 116.198.203.111:9092
```

![截屏2024-10-28 16.00.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-28%2016.00.05.png)

topic下具体的partition的信息 数字代表此**partition所在的borker(Leader),副本所在的broker(Replicas)**

**ISR**（In-Sync Replicas）是指当前与 Leader 副本保持同步的副本集合。ISR 机制帮助 Kafka 实现数据可靠性和一致性，是 Kafka 副本管理和容错的关键部分

## 发送消息

```
# 发送消息到指定 topic
./kafka-console-producer.sh --broker-list 116.198.203.111:9092 --topic testDemo
```

![截屏2024-10-28 16.10.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-28%2016.10.17.png)

**按control + D 关闭输入**

## 消费消息

```
#加上--from-beginning 从头开始消费 不然是从最新消息开始
./kafka-console-consumer.sh --bootstrap-server 116.198.203.111:9092 --topic testDemo
```

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-28%2016.16.10.png" alt="截屏2024-10-28 16.16.10" style="zoom:150%;" />

**control + C 退出监听消息**

## **消费者群组**

**分区与消费者组中各消费者实例的绑定**

通过 Kafka 内部的**消费者组协调器**和**分区分配策略**来实现的。当一个消费者加入消费者组后，Kafka 会通过协调器自动分配分区给每个消费者，从而实现消息的均衡消费

![截屏2024-10-28 16.49.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-28%2016.49.16.png)

```
# 启动 Kafka 控制台消费者并指定消费者组
./kafka-console-consumer.sh --bootstrap-server <Kafka服务器地址> --topic <主题名称> --group <消费者组名称> --from-beginning
```

```
# 查看所有消费者组
./kafka-consumer-groups.sh --bootstrap-server <Kafka服务器地址> --list

# 查看特定消费者组的信息
#包括消费者实例的 Consumer ID 和分区分配情况
./kafka-consumer-groups.sh --bootstrap-server <Kafka服务器地址> --describe --group <消费者组名称>
```

在创建消费者的时候就指定group 和 自己的id 下次重新用这个group + id 就可以实现使用同一个消费者消费

```
./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic myTopic --group myGroup --consumer-property group.instance.id=myConsumerInstance --from-beginning
```

# Java客户端

**依赖**

```xml
<dependency>
    <groupId>org.apache.kafka</groupId>
    <artifactId>kafka-clients</artifactId>
    <version>3.0.0</version>
</dependency>
 <!--kafka内部需要使用到日志依赖-->
    <dependency>
      <groupId>ch.qos.logback</groupId>
      <artifactId>logback-classic</artifactId>
      <version>1.2.3</version>
    </dependency>
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-api</artifactId>
      <version>1.7.30</version>
    </dependency>
```

**消费者(单)**

```java
package com.sjc.kafka;

/**
 * @author abstractMoonAstronaut
 * {@code @date} 2024/10/28
 * {@code @msg} reserved
 */
import org.apache.kafka.clients.consumer.ConsumerRecord;
import org.apache.kafka.clients.consumer.ConsumerRecords;
import org.apache.kafka.clients.consumer.KafkaConsumer;


import java.util.Collections;
import java.util.Properties;

import static com.sjc.kafka.KafkaConstant.*;


public class Consumer {


    private static KafkaConsumer<String,String> consumer = null;

    static {
        Properties configs = initConfig();
        consumer = new KafkaConsumer<String, String>(configs);
        consumer.subscribe(Collections.singletonList(TOPIC));
    }

    private static Properties initConfig(){
        Properties properties = new Properties();
        properties.put("bootstrap.servers",BROKER_LIST);
        properties.put("group.id","0");
        properties.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
        properties.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
        properties.setProperty("enable.auto.commit", "true");
        properties.setProperty("auto.offset.reset", "earliest");
        return properties;
    }


    public static void main(String[] args) {
        while (true) {
            ConsumerRecords<String, String> records = consumer.poll(10);
            for (ConsumerRecord<String, String> record : records) {
                System.out.println("------------------------------------");
                System.out.println(record);
            }
        }
    }
}
```

**生产者**

```java
package com.sjc.kafka;
import org.apache.kafka.clients.producer.*;
import org.apache.kafka.common.serialization.StringSerializer;
import java.util.Properties;

import static com.sjc.kafka.KafkaConstant.BROKER_LIST;
import static com.sjc.kafka.KafkaConstant.TOPIC;

/**
 * @author abstractMoonAstronaut
 * {@code @date} 2024/10/28
 * {@code @msg} reserved
 */

public class Producer {

    private static KafkaProducer<String,String> producer = null; // Kafka 生产者的实例，用于发送消息到 Kafka
    /*
    初始化生产者
     */
    static {
        Properties configs = initConfig();
        producer = new KafkaProducer<String, String>(configs);//表示生产者将字符串类型的键和值发送到 Kafka
    }

    /*
    初始化配置
     */
    private static Properties initConfig(){
        Properties properties = new Properties();
        properties.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG,BROKER_LIST); //kafka地址
        //指定键的序列化类，这里使用 StringSerializer，表示键将被序列化为字符串
        properties.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());
        //指定值的序列化类，同样使用 StringSerializer，表示值将被序列化为字符串
        properties.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG,StringSerializer.class.getName());
        return properties;
    }

    public static void main(String[] args) throws InterruptedException {
        //消息实体
        ProducerRecord<String , String> record = null;
        for (int i = 0; i < 2000; i++) {
            record = new ProducerRecord<String, String>(TOPIC, "value"+(int)(10*(Math.random())));
            //发送消息
            producer.send(record, new Callback() {
                @Override
                public void onCompletion(RecordMetadata recordMetadata, Exception e) {
                    //recordMetadata封装返回信息
                    if (null != e){
                        System.out.println("send error" + e.getMessage());
                    }else {
                        System.out.println("offset - " + recordMetadata.offset());
                        System.out.println("partition - " + recordMetadata.partition());
                    }
                }
            });
            Thread.sleep(1000);
        }
        producer.close();
    }
}
```

**消费者群**

```java
package com.sjc.kafka;
import org.apache.kafka.clients.producer.*;
import org.apache.kafka.common.serialization.StringSerializer;
import java.util.Properties;

import static com.sjc.kafka.KafkaConstant.BROKER_LIST;
import static com.sjc.kafka.KafkaConstant.TOPIC;

/**
 * @author abstractMoonAstronaut
 * {@code @date} 2024/10/28
 * {@code @msg} reserved
 */

public class Producer {

    private static KafkaProducer<String,String> producer = null; // Kafka 生产者的实例，用于发送消息到 Kafka
    /*
    初始化生产者
     */
    static {
        Properties configs = initConfig();
        producer = new KafkaProducer<String, String>(configs);//表示生产者将字符串类型的键和值发送到 Kafka
    }

    /*
    初始化配置
     */
    private static Properties initConfig(){
        Properties properties = new Properties();
        properties.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG,BROKER_LIST); //kafka地址
        //指定键的序列化类，这里使用 StringSerializer，表示键将被序列化为字符串
        properties.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());
        //指定值的序列化类，同样使用 StringSerializer，表示值将被序列化为字符串
        properties.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG,StringSerializer.class.getName());
        return properties;
    }

    public static void main(String[] args) throws InterruptedException {
        //消息实体
        ProducerRecord<String , String> record = null;
        for (int i = 0; i < 2000; i++) {
            record = new ProducerRecord<String, String>(TOPIC, "value"+(int)(10*(Math.random())));
            //发送消息
            producer.send(record, new Callback() {
                @Override
                public void onCompletion(RecordMetadata recordMetadata, Exception e) {
                    //recordMetadata封装返回信息
                    if (null != e){
                        System.out.println("send error" + e.getMessage());
                    }else {
                        System.out.println("offset - " + recordMetadata.offset());
                        System.out.println("partition - " + recordMetadata.partition());
                    }
                }
            });
            Thread.sleep(1000);
        }
        producer.close();
    }
}
```

# producer

## 发送流程

消息发送整体流程，生产端由两个线程协调运行

分别为**main**线程和**sender**线程（发送线程

**1 -** 在创建**KafkaProducer**的时候，创建了一个**Sender**对象，并且**启动了—个IO线程**





**2 -** **执行拦截器的逻辑(实现消息的定制化)**

可以在producer的构造方法中的config中自定义拦截器，不然自动创建

![截屏2024-10-28 21.09.37](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-28%2021.09.37.png)

```java
public Future<RecordMetadata> send(ProducerRecord<K, V> record, Callback callback) {
    ProducerRecord<K, V> interceptedRecord = this.interceptors.onSend(record);
    return this.doSend(interceptedRecord, callback); //在发送消息后调用拦截器方法
}
```

**自定义拦截器实例**

```java
public class Charginginterceptor implements ProducerInterceptor<String, String> {
	//发送消息的时候触发
	@Override
	public ProducerRecord<String, String> onSend(ProducerRecord<String, String> record) {
	 	System.out.println("1分钱1条消息，不管那么多反正先扣钱”);
		return record;
	}

	//收到服务端的ACK的时候触发
	@Override
	public void onAcknowledgement(RecordMetadata metadata, Exception exception) {
		System.out.pnntln(n消息被服务端接收啦”);
	}
	
	@Override 
	public void close() {
		System.out.printin(”生产者关闭了");
	}
	
	//用键值对配置的时候触发
	@Override
	public void configure(Map<String, ?> configs) {
		System. out.piintln(n configure...H);
	}
}

```



**3 -**接下来是利用指定的工具对key和value进行序列化

**配置的config - properties**

```java
//指定键的序列化类，这里使用 StringSerializer，表示键将被序列化为字符串
properties.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());
//指定值的序列化类，同样使用 StringSerializer，表示值将被序列化为字符串
properties.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG,StringSerializer.class.getName());
```

**提供了一系列的序列化器**

![截屏2024-10-28 21.14.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-28%2021.14.27.png)



**4 -**分区路由

**决定消息发送到哪个分区partition**

```java
/**
*	record 是生产者发送的消息，包括主题（topic）、键（key）、值（value）、以及时间戳等信息。
* 序列化后的key和value
* cluster 是 Kafka 集群的元数据对象，包含主题、分区、副本等信息
*/
int partition = partition(record, serializedKey, serialized Value, cluster);
```

**key-value 结构的作用**

​	1.	**分区路由**：如果指定了 key，Kafka 默认对 key 进行哈希，然后对分区数量取模，决定将消息分配到哪个分区。这样，具有相同 key 的消息总是路由到同一个分区，保证了分区内的消息顺序性。

​	2.	**数据关联**：在 key-value 结构中，key 可以用作业务上的标识符，比如用户 ID、订单 ID 等，让消费者在消费消息时可以识别和区分不同的消息类型或来源。

​	3.	**消息的分布**：如果 key 是空的，Kafka 会随机分配消息的分区（通常使用轮询算法），以达到均衡负载的目的。

**partition决定机制**

- **指定了partition**——直接将指定的值直接作为partiton值。

- 没有指定partition,**自定义了分区器**——将使用自定义的分区器算法选择分区。

- 没有指定partition,没有自定义分区器，**但是key不为空**——使用**默认分区器**DefaultPartitioner,将key的hash值与topic的partition数进行**取余**得到partition值；

- 没有指定partition,没有自定义分区器，但是key是空的——**第一次调用时随机生成一个整数**(后面每次调用在这个整数上**自增**)，将这个值与topic**可用的partition总数取余得到 partition值**，也就是常说的**round-robin** 轮询算法。

  

**5 -** 消息类加器

**选择分区以后并没有直接发送消息，而是把消息放入了消息累加器**

```java
RecordAccumulator.RecordAppendResult result = accumulator.append(tp, timestamp, serializedKey, serializedValue, headers, interceptcallback, remainingWaitMs);

ConcurrentMap<TopicPailition Deque<ProducerBatc>> batches; //RecordAccumulator 本质上是一个 ConcurrentMap:
```

**RecordAccumulator**(负责**管理待发送的消息的缓冲和批量处理**) 使用 **ConcurrentMap<TopicPartition, Deque<ProducerBatch>>** **batches** 来维护这个关系，其中 **TopicPartition** 是主题和分区的组合，而 **Deque<ProducerBatch>** 则是一个双端队列，用于存储待发送的消息批次。

**对于 Kafka 中的每个分区，会有一个独立的消息批次（batch）。每个分区有其独立的消息队列和批次管理机制**



**发送条件包括：**

​	•	**批次大小（**batch.size**）**：如果积累的消息**字节数达到这个大小**，批次会被发送。

​	•	**最大等待时间（**linger.ms**）**：即使未达到批次大小，如果在此时间内（从发送请求开始到消息被发送）有新的消息加入，生产者会等待新的消息加入到当前批次，直到时间到达后再发送。如果 linger.ms 的时间到了，即使批次大小没有达到，也会发送当前的批次。这确保了消息不会长时间滞留在缓冲区



允许生产者**立即将所有积累的消息发送到 Kafka 服务器**，而不等待批次大小或时间条件（手动保证**时效性**）

```java
KafkaProducer<String, String> producer = new KafkaProducer<>(props);

try {
    for (int i = 0; i < 10; i++) {
        String key = "key-" + i;
        String value = "message-" + i;
        ProducerRecord<String, String> record = new ProducerRecord<>("test-topic", key, value);
        producer.send(record);
    }
    // 强制发送当前所有待发送的消息
    producer.flush();
} finally {
    producer.close();
}
```

## 应答ack

kafka服务端有一种响应客户端的方式，只有在服务端确认以后，生产者才发送下一轮的消息，否则重新发送数据

Kafka为客户端提供了三种可靠性级别，用户根据对**可靠性和延迟**的要求进行权衡，选择相应的配置

- **acks=0**： producer**不等待broker的ack**,这一操作提供了一个最低的延迟，broker一接收到还没有写入磁盘就已经返回，当broker故障时有可能丢失数据；

- **acks=1**（默认）： producer 等待 broker 的 ack, **partition 的 leader 落盘成功后** 返回ack,如果在follower同步成功之前leader故障，那么将会丢失数据；

- **acks=-1** (**all**) ： producer等待 broker 的 ack, **partition 的 Ieader和 follower全部落盘成功后**才返回ack。

    三种机制，**性能依次递减（producer吞吐量降低**），**数据健壮性则依次递增**。我们可 以根据业务场景使用不同的参数。

```java
 Properties props = new Properties();
        props.put("bootstrap.servers", "localhost:9092"); // Kafka 服务器地址
        props.put("key.serializer", StringSerializer.class.getName()); // 键序列化器
        props.put("value.serializer", StringSerializer.class.getName()); // 值序列化器

        // 设置 acks 参数
        props.put("acks", "all"); // 可以设置为 0, 1 或 all
```

**ISR**

假设leader收到数据，所有follower都开始同步数据，但是有一个follower出了问题，没有办法从leader同步数据。按照这个规则，leader就要一致等待，无法发送 ack

**只有那些正常工作的follower同步数据的时候才会等待**

**把那些正常和leader保持同步的replica维护起来，放到一个动态set里面**，这个就叫做in-sync replica set (**ISR**) 同步副本set

ISR里面的follower同步完数据之后，我就给客户端发送ACK

如果一个follower长时间不同步数据，就要从ISR**剔除**。同样，如果后面这个follower重新与leader保持同步，就会**重新加入ISR**。

时间间隔由以下参数决定：

```java
replica.lag.time.max.ms=10000
```

如果跟**follower**在这个时间内**未能从leader获取到新消息**，**leader**就会认为该副本处于“**落后**”状态。超过这个时间，**leader**可能会将该副本从 **ISR（In-Sync Replicas）**中移除，这意味着该副本不再被视为“**同步副本**”。

**在config/server.properties中去配置这个值**

# 消息存储原理

## 数据存储目录

**日志存储目录即为数据区域**

**（不是哥们，怎么忘记改了，最好把log目录配置到usr/local/kafka下）**

`logs.dir=/tmp/kafka-logs`

![截屏2024-10-29 19.29.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-29%2019.29.50.png)

## partition分区

- topic 分成多个 partition
- 一个partition中消息有序，顺序写入，但不一定全局有序

**每个partition都有一个物理目录，topic名字后面的数字标号即代表分区**

**服务器上log目录内部**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-29%2019.33.14.png" alt="截屏2024-10-29 19.33.14" style="zoom:50%;" />

## replica副本

- 设计**partition**的副本**replica**提高可靠性
- 创建topic的时候，通过指定replication-factor确定topic的副本数
- **副本数量要小于Broker的数量**
- 副本分为**leader**(对外提供读写服务)和**follower**(从**leader**异步拉取同步数据)

## 副本分布规则

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-29%2019.37.02.png" alt="截屏2024-10-29 19.37.02" style="zoom:50%;" />

**第一个分区**（编号为0）的第一个副本放置位置是**随机从 brokerList 选择的**

**其他分区的第一个副本位置**相对于第0个分区依次往后移 



一组分区副本 除第一个副本外，其余副本会由一个公式计算出来，公式一开始简单普通取余

后面的公式有点复杂，其中使用**nextReplicaShift**实现均衡的副本分布

**nextReplicaShift 是 Kafka 内部副本分配算法自动决定的一个值，通常不由用户显式指定。Kafka 会根据集群代理的数量、分区数量、副本因子等参数自动计算和分配**

## Segment

**一个partition又被划分成多个segment来组织数据**

在磁盘上，每个segment由一个log文件和2个index文件组成

![截屏2024-10-29 19.51.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-29%2019.51.43.png)

**leader-epoch-checkpoint**文件中保存了每一任leader开始写入消息时的offset

**partition.metadata**存储分区元数据的文件，用于记录该分区的状态和一些控制信息

**.index偏移量(offset)索引文件**，将 Kafka 消息的偏移量映射到日志段文件中的实际物理位置(**快速定位消息**)

**.timeindex 时间戳(timestamp)索引文件**，根据时间戳快速定位消息

**.log日志文件（日志就是数据）**



**一个segment中，日志是追加写入的，满足一定条件就会切分日志：**

- 根据文件大小，写满后，创建新的segment，**用最新的offset作为名称**
- 根据消息的**最大时间戳**，和当前系统时间戳的**差值**，超过设定就会新建
- offset索引文件或者timestamp索引文件达到了一定的大小，超过阈值就会新建



**leader-epoch-checkpoint 文件记录了每个 leader epoch 与其对应的起始偏移量**

​	•	**保持数据一致性**：当副本重新加入 ISR 时，可以根据 leader-epoch-checkpoint 文件中的信息检查并确保该副本的日志在特定 epoch 内是**完全一致**的。

​	•	**支持日志截断**：当 Kafka 需要截断日志（比如在 **leader 切换后**），可以通过 leader-epoch-checkpoint 文件找到特定 epoch 的起始偏移量，从而知道日志截断的**准确位置**。

​	•	**恢复和数据对齐**：当新的 leader 接管分区时，其他跟随者可以利用 leader-epoch-checkpoint 文件来**对齐数据**，从而快速恢复一致的日志。

## 索引

根据offset获取消息，必须要有一种**快速检索消息的机制** - index

**偏移量索引文件** - offset和消息物理地址（在log文件中的位置）的映射关系

**时间戳索引文件** - 时间戳和offset的关系



Kafka的索引并不是每一条消息都会建立索引，而是一种稀疏索引**sparse index**

`log.index.interval.bytes=4096` 这个值设定的是消息大小的阈值，只要写入的消息超过了 这个阈值,偏移量索引文件index和时间戳索引文件.timeindex就会増加一条索引记录(索引项)。

这个值设置越小，索引越密集。值设置越大，索引越稀疏。相对来说，越稠密的索引检索数据更快，但是会消耗更多的存储空间。越的稀疏索引占用存储空间小，但是插入和删除时所需的维护开销也小



时间戳索引有两种，一种是**消息创建的时间戳**，一种是**消息在Broker追加写入的时间**

```handlebars
log.message.timestamp.type=CreateTime
```

  默认是**创建时间**。如果要改成日志追加时间，则修改为**LogAppendTime**

## 索引检索消息

1 - 根据**分区路由的机制**，可以算出消息属于哪个**partition**

2 - segment文件是由base offset命名的，可以用**二分发快速找到消息属于哪个段文件**

3 - 根据segment的**索引文件**，查找offset对应的**position**位置(不是每个消息都记录，所以这是大概位置)

4 - 根据**position**在**log**中一一比对，找到和**offset**重合的消息

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-29%2020.31.54.png" alt="截屏2024-10-29 20.31.54" style="zoom:50%;" />

## 日志清理策略

由于Kafka的日志**并没有在消费后马上删除**，随着时间的推移，日志越来越多，需要有**相应的清理策略**来保证系统的健康，日志清理的**开关**由以下参数控制：

`log.cleaner.enable=true`

提供了**两种清理方式**，分别是删除**delete**和压缩**compact**

`log.cleanup.policy=delete`

删除的功能是由**定时任务**完成的，定时任务运行的**时间间隔由以下参数控制**

`log.retention.check.interval.ms=300000`

删除多久以前的数据由以下参数控制（**精度越小优先级越高**)

`log.retention.hours=168`
`log.retention.minutes`
`log.retention.ms`

根据保留**日志的大小**来删除数据，**超出该大小则删除最早的数据**（默认-1代表不限制）

`log.retention.bytes=-1` 

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-29%2020.36.10.png" alt="截屏2024-10-29 20.36.10" style="zoom:50%;" />

## Controller选举

早期分片副本选取leader，使用投票机制，只用ZK选举。

**利用ZK怎么实现选举**？ZK的什么功能可以感知到节点的变化（增加或者减少）? 或者说，ZK为什么能实现加锁和释放锁？

- **watch机制**
- **节点不允许重复写入**
- **临时节点**

**存在问题：**

如果分区和副本**数量过多**，所有的副本都**直接进行选举**的话，一旦某个出现节点的增减，就会**造成大量的watch事件被触发**，ZK的**负载**就会过重



不是所有的repalica都参与leader选举，而是**由其中的一个Broker统一来指挥**， 这个Broker的角色就叫做**Controller**

所有的Broker会**尝试**在zookeeper中**创建临时节点/controller**,只有一个能创建成功，成为**controller**

如果Controller挂掉了或者网络出现了问题，ZK上的**临时节点会消失**。其他的**Broker**通过**watch**监听到**Controller**下线的消息后，开始竞选新的Controller。方法跟之前还是一样的,**谁先在ZK里面写入一个/controller节点,谁就成为新的Controller**
**controller的职责**

- 监听Broker变化。
- 监听Topic变化。
- 监听Partition变化。
- 获取和管理Broker、Topic、Partition的信息。
- 管理Partiontion的主从信息。

## 分区副本Leader选举

**副本有三个概念**

- **Assigned-Replicas (AR)**：一个分区的所有副本
- **In-Sync Replicas (ISR)**：一个分区中跟leader数据保持一定程度的同步的副本
- **Out-Sync-Replicas (OSR)**：跟leader副本同步滞后过多的副本

**只有ISR有资格参加leader选举**。而且这个ISR**不是固定不变的**，它是一个动态的列表。
如果同步延迟超过30秒，就踢出ISR，进入OSR。如果赶上来了， 就加入ISR。

**默认情况下**，当leader副本发生故障时，只有在ISR集合中的副本才有资格被选举为新的leader。

**极端情况下**，如果ISR为空，则可以允许ISR之外的副本参与选举，由以下参数控制：

`unclean.leader.election.enable=true`



​	1.	**故障检测**：Controller 定期检查各个副本的状态，如果检测到 Leader 副本不可用，会触发选举流程。

 2. **从 ISR 列表中选择新 Leader**：如果 ISR 中有可用副本，Controller 会在 ISR 列表中选择一个新的 Leader，并更新元数据。

    使用一种**类似微软的PacificA算法**，在这种算法中，**默认是让ISR中第一个replica**变成leader。比如ISR是1、5、9, 优先让1成为leader

​	3.	**更新 ZooKeeper 和通知 Broker**：Controller 更新 ZooKeeper 中的 Leader 信息，并通知所有 Broker 更新 Leader 副本，客户端可以立即与新的 Leader 副本交互。

​	4.	**同步 Follower**：新的 Leader 开始接受写入请求，其他 Follower 开始同步新的 Leader 副本的数据。

## 主从同步

**不同的raplica的offset是不一样的**

**LEO (Log End Offset)：** 下一条等待写入的消息的offset (最新的offset + 1),图中分别是9, 8, 6
**HW (Hign Watermark):** ISR中最小的LEO。Leader会管理所有ISR中最小的 LEO作为HW,目前是6

Follower 副本的 HW 是从 Leader 的 HW 同步过来的，而不是独立计算的。因此，Follower 的 HW 只能等于或低于 Leader 的 HW。

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-29%2020.53.21.png" alt="截屏2024-10-29 20.53.21" style="zoom:50%;" />

**consumer最多只能消费到HW之前的位置(消费到offset 5的消息)。也就是说: 其他的副本没有同步过去的消息，是不能被消费的**

- **follower**节点会向**Leader**发送一个**fetch**请求，leader向follower发送数据
- **follower**接收到数据响应后，**依次写入消息**并且更新LEO。
- leader 更新 **HW** （ISR 最小的 LEO）。



**follower的数据反馈**

​	•	**发送确认**：在成功处理 Leader 返回的消息并更新自己的 LEO 后，Follower 可能会向 Leader 发送确认（**Acknowledgment**），表明自己已成功处理这些消息。**这有助于 Leader 了解 Follower 的同步状态。**

​	•	**更新 ISR 列表**：如果 Follower 的 LEO 达到某个条件（**例如与 Leader 的 LEO 保持在一定范围内**），Leader 可以将该 Follower 保持在 **ISR**（In-Sync Replicas，同步副本集合）列表中。

## replica故障处理

### follower故障

![截屏2024-10-29 21.00.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-29%2021.00.01.png)

恢复以后，首先根据之前记录的**HW** （6）,把高于HW的消息截掉（6、7）。然 后**向leader同步消息**(从自己的hw?)。追上leader之后（**30**秒），重新加入**ISR**





### leader故障

假设图中leader发生故障。首先选一个leader。因为replica 1 （中间这个）优先，它成为leader。为了**保证数据一致**，其他的follower需要**把高于HW的消息截取掉**（这里没有消息需要截取）。然后replica2同步数据。注意：这种机制只能**保证副本之间的数据一致性**，**并不能保证数据不丢失或者不重复**。

### 故障导致的数据丢失

 假设ISR中只有一个leader副本，且成功写入数据。此时leader挂掉，**若参数设置允许OSR中的副本抢占leader的话，可能导致数据丢失。**
  这种场景系统也无法处理，所以Kafka交给用户自行选择，**若unclean.leader.election.enable=true则无法保证消息的可靠性，若unclean.leader.election.enable=false则无法保证系统的效率和可用性**



***这部分太复杂 总结一下 写个文章 注册一个watch事件，有时间再整理***

：Kafka 0.11 版本之前没有 Leader Epoch 概念，导致 Follower 副本在追随 Leader 时可能出现不正确的消息截断（即，HW 不一致）。

​	•	**消息截断问题**：在 Follower 副本追随 Leader 时，如果 Leader 发生故障或更换，可能会出现不一致的情况。比如，Leader 副本在处理一条消息后成功更新了自己的 HW，但在这条消息被同步到所有 Follower 之前，Leader 就发生了故障。这时候，新的 Leader 可能会重用旧的消息记录，而不是已经被确认的最新消息，从而造成消息的截断。

​	•	**HW 不一致**：由于没有 Leader Epoch 的机制，Follower 副本无法准确知道自己应该追随哪个 Leader 的数据。在某些情况下，Follower 可能会从一个不同的、过时的 Leader 副本获取数据，这会导致 Follower 副本的数据与新的 Leader 副本的数据不一致。



# comsumer

## Offset维护

### offset的存储

**partition** 和其对应的上次消费位置(**offset**)的对应关系被保存下来

消费者组**gp-assign-group-1**和 **ass5part** （5个分区）的partition的偏移量关系用以下代码访问

`./kafka-consumer-groups.sh “bootstrap-server 192.168.44.161:9093,192.168.44.161:9094,192.168.44.161:9095 —describe —group gp-assign-group-1`

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-30%2018.57.17.png" alt="截屏2024-10-30 18.57.17" style="zoom:50%;" />

- **CURRENT-OFFSET**指的是下一个未使用的offset。
- **LEO , Log End Offset**:下一条等待写入的消息的offset （最新的offset + 1）
- **LAG**是延迟量

这是一个**consumer group**和**topic**中的一个**partition**的关系（**offset在partition中连续编号而不是全局连续编号**）。



**Kafka**早期的版本把消费者组和**partition**的**offset**直接维护在**ZK**中，但是读写的性能消耗太大了。

后来就放在一个特殊的**topic**中，名字叫**—consumer_offsets**，默认有 50 个分区**(offsets.topic.num.partitions 默认是 50)**，每个分区默认一个**replication**



**topic**中，经过**序列化和反序列化**可以存放**对象类型的value** - 

- **GroupMetadata**:保存了消费者组中各个消费者的信息（**每个消费者有编号**）。

- **OffsetAndMetadata**:保存了消费者组和各个**partition**的**offset**位移信息元数据。

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-30%2019.05.59.png" alt="截屏2024-10-30 19.05.59" style="zoom:50%;" />



通过**hash取模**，得到一个 consumer group的offset **存放在topic的哪一个分区**

`Math.abs("gp-assign-group-1".hashCode()) % 50;`



### 如果找不到offset

 如果增加了一个**新的消费者组**去消费—个**topic**的某个**partion**,没有offset的记录，这个时候应该从哪里开始消费呢？ 由以下参数控制：

`auto.offset.reset=latest`

- **latest**： 从最新的消息（最后发送的）开始消费。
- **earliest**： 从最早的（最先发送的）消息开始消费。可以消费到历史消息。
- **none**： 如果consumer group在服务端找不到offset会报错。

### offset的跟新

消费者需要**手动commit**才能变更offset，并非在消费完消息就会跟新



`enable.auto.commit=true` 设置为自动提交

`auto.commit.interval.ms=5000`设置自动提交的频率



如果是手动提交，需要调用方法跟新offset

-  `consumer.commitSync()`手动**同步**提交。

-  `consumer.commitAsync()`手动**异步**提交。

**如果不提交或者提交失败,Broker的Offset不会更新，消费者下次消费的时候会受到重复消息**

## 消费者和消费策略	

### 分区分配策略

一个分区只能被一个consumer group中的一个consumer消费

当分区数量大于consumer数量，为了保证系统最大的利用率，分配遵循一个基本原则，平均分配，不会有任意两个消费者消费的分区数差大于1

**1）Range（范围）**
  C1：P0 P1 P2
  C2：P3 P4 P5
  C3：P6 P7
**2）RoundRobin（轮询）**
  C1：P0 P3 P6
  C2：P1 P4 P7
  C3：P2 P5

**3）sticky（粘滞）**

粘滞的分配策略较为复杂，它的核心思想是在分区**重新分配时保证最小的移动**（类似Redis的一致性hash思想，实现方式不同）。

第一次分配**类似轮询**，结果如下：
  C1：P0 P3 P6
  C2：P1 P4 P7
  C3：P2 P5

**假设此时C2挂掉**：
若按照**RoundRobin**，结果如下：
  C1：P0 P2 P4 P6
  C3：P1 P3 P5 P7

sticky结果如下（尽量保证P0 P3 P6 P2 P5不动）：
  C1：P0 P3 P6 P1
  C3：P2 P5 P4 P7

保证数据的**最小迁移量**，减少分区分配时候的**性能损耗**

### 分区重分配Rebalance

**重新分配分区与消费者之间的映射关系的过程**

消费者和分区数量发生变化都会发生reblance

- 确定协调者**coordinator**，通常由集群中**负载最小的Broker**承担

- 所有**consumer**向**coordinator**发送**join group**请求，确认自己是该组成员
- **coordinator**在所有**consumer**中确定**leader**（通常是**第一个**）并由**leader确定分区分配结果**
- **coordinator**向所有**consumer**发送**分区分配结果**

## Kafka快的原因

### 顺序读写

**磁盘结构**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-30%2019.32.46.png" alt="截屏2024-10-30 19.32.46" style="zoom:50%;" />

磁盘的盘片不停地旋转，磁头会在磁盘表面画出一个**圆形轨迹**，这个就叫**磁道**。从内到位半径不同有很多磁道。然后又用半径线，把磁道分割成了**扇区**（两根射线之内的扇区组成扇面）。如果要**读写数据, 必须找到数据对应的扇区**，这个过程就叫**寻址**



- **随机I/O**：读写的多条数据在**磁盘上是分散的**，寻址会很耗时。

- **顺序I/O**：读写的数据在磁盘上是**集中**的，**不需要重复寻址**的过程。



 **Kafka**的**message**是不断**追加**到本地磁盘文件末尾的，而**不是随机的写入**，这使得 Kafka写入吞吐量得到了显著提升

**一定条件下，速度大于内存的随机读写**

### 索引

我们在写入日志的时候会建立关于**Offset**和**时间的稀疏索引**，提升了查找效率

### 批量读写

Kafka无论是生产者发送消息还是消费者消费消息都是**批量操作**的，大大提高读写性能

### 零拷贝

​        操作系统**虚拟内存**的**内核空间**和**用户空间**。操作系统的虚拟内存分成了两块，一部分是内核空间，一部分是用户空间。这样就可以**避免用户进程直接操作内核，保证内核安全**

进程在内核空间可以**执行任意命令**，调用系统的一切资源

在用户空间必须要通过**—些系统接口**才能向内核发出指令

如果用户要从磁盘读取数据(比如kafka消费消息)，必须先**把数据从磁盘拷贝到内核缓冲区**，然后在从**内核缓冲区到用户缓冲区**，最后才能**返回给用户**


<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-30%2019.37.26.png" alt="截屏2024-10-30 19.37.26" style="zoom:50%;" />



第二个是DMA拷贝。没有DMA技术的时候，拷贝数据的事情需要CPU亲自去做, 这个时候它没法干其他的事情，如果传输的数据量大那就有问题了。**DMA技术叫做直接内存访问(Direct Memory Access)**,其实可以理解为CPU给 自己找了一个小弟帮它做数据搬运的事情。在进行**I/O设备和内存的数据传输**的时候， 数据搬运的工作全部交给**DMA控制器**，解放了 CPU的双手。


**传统I/O模型**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-30%2019.39.53.png" alt="截屏2024-10-30 19.39.53" style="zoom:50%;" />



 比如kafka要消费消息，比如要先把数据从磁盘拷贝到内核缓冲区，然后拷贝到用户缓冲区，再拷贝到socket缓冲区，再拷贝到网卡设备。这里面发生了 4次用户态和内核态的切换和4次数据拷贝，2次系统函数的调用（read、write）,这个过程是非常耗费时间的。怎么优化呢?
  在Linux操作系统里面提供了一个**sendfile**函数（并不是所有操作系统都支持sendfile），可以实现"零拷贝"。这个时候就**不需要经过用户缓冲区了，直接把数据拷贝到网卡**（这里画的是支持**SG-DMA**拷贝的情况）。因为这个只有DMA拷贝，没有CPU拷贝，所以叫做”零拷贝”。零拷贝至少可以提高一倍的性能。





**磁盘文件通过DMA拷贝，到达内核缓存区，通过SG-DMA直接拷贝到网卡，跳过CPU拷贝**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-30%2019.41.33.png" alt="截屏2024-10-30 19.41.33" style="zoom:50%;" />

# MQ选型

## Kafka特性

- **高吞吐、低延迟**： kakfa最大的特点就是收发消息非常快，kafka每秒可以处理几十万条消息，它的最低延迟只有几毫秒；
- **高伸缩性**： 如果可以通过增加分区**partition**来实现扩容。不同的分区可以在不同的Broker中。通过ZK来管理Broker实现扩展，ZK管理Consumer可以实现负载
- **持久性、可靠性**： Kafka能够允许数据的**持久化存储**，消息被持久化到磁盘，并支持数据备份防止数据丢失；
- **容错性**： 允许集群中的节点失败，某个节点宕机，Kafka集群能够正常工作；
- **高并发**： 支持数千个客户端同时读写。

## Kafka对比RabbitMQ

- **产品侧重**：kafka:**流式**消息处理、**消息引擎**；RabbitMQ**:消息代理。**
- **性能**：kafka有更高的**吞吐量**。RabbitMQ主要是**push**, kafka只有**pull**。
- **消息顺序**：分区里面的消息是有序的，**同一个consumer group里面的一个消费者只能消费一个partition**,能保证消息的**顺序性**
- **消息的路由和分发**：RabbitMQ更加灵活。
- **延迟消息、死信队列**：RabbitMQ支持。
- **消息的留存**：kafka消费完之后消息会留存，RabbitMQ消费完就会删除。Kafka可以设置retention,清理消息。



优先选择**RabbitMQ**的情况：

- 高级灵活的**路由**规则；

- 消息时序控制（控制消息**过期**或者消息**延迟**）；
- 高级的**容错处理**能力，在消费者**更有可能处理消息不成功**的情景中（瞬时或者持久）;
- 更简单的消费者实现。



优先选择**Kafka**的情况：

- 严格的消息**顺序**；

- 延长消息**留存时间**，包括过去**消息重放**的可能；
- 传统解决方案无法满足的**高伸缩**能力。

## MQ总体选型

![](https://i-blog.csdnimg.cn/blog_migrate/a75c30267767d691a5a55ef41488b6e7.png)

# Kafka参数配置

## 生产者参数

![](https://i-blog.csdnimg.cn/blog_migrate/1a9217ebabd3ddfd068b8b39a12023c8.png)

## 服务端参数

![](https://i-blog.csdnimg.cn/blog_migrate/d77ea9d36219047da73a17ebc9ac6333.png)

## 消费者参数

![](https://i-blog.csdnimg.cn/blog_migrate/ab8cf3bc08ca1f090474cbe2848b5141.png)

## 增加数据可靠性配置

- **设置acks = al**l。acks是Producer的一个参数，代表已提交消息的定义。如果设置成all,则表明所有Broker都要接收到消息，该消息才算是”已提交”。

- 设置retries为一个较大的值。同样是Producer的参数。当出现网络抖动时，消息发送可能会失败，此时配置了retries的Producer能够自动重试发送消息，尽量避免消息丢失。配置在Properties中

- 设置 **unclean.leader.election.enable**=false。

- 设置replication.factor >= 3。需要三个以上的replication。

- 设置min.insync.replicas > 1。Broker端参数，控制消息至少要被写入到多少个副本才算是"已提交"。设置成大于1可以提升消息持久性。在生产环境中不要使用默认值1。确保replication.factor(副本数量) > min.insync.replicas。如果两者相等，那么只要有 —个副本离线整个分区就无法正常工作了。推荐设置成replication.factor = min.insync.replicas + 1。

- **确保消息消费完成再提交**。Consumer端有个参数enable.auto.commit,最好设置成false,并自己来处理offset的提交更新。

  