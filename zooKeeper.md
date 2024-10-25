# Zookeeper概念

-  Zookeeper是Apache Hadoop项目下的一个子项目，是一个树形目录服务
- Zookeeper是一个分布式的，开源的**分布式应用程序协调**服务
- **配置管理，分布式锁，集群管理(注册中心)**

# Zookeeper安装

**官网下载页**

https://zookeeper.apache.org/releases.html#download

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-23%2018.43.43.png" alt="截屏2024-10-23 18.43.43" style="zoom:50%;" />

ZooKeeper conf配置目录下，有一个zoo-sample的示例配置文件，只有复制成一个zoo(名字)的文件配置才会生效

![截屏2024-10-23 18.55.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-23%2018.55.47.png)

**修改配置文件内容 手动指定一个存储目录**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-23%2018.56.27.png" alt="截屏2024-10-23 18.56.27" style="zoom:50%;" />

![截屏2024-10-23 18.58.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-23%2018.58.18.png)

# ZooKeeper命令操作

## 数据模型

![截屏2024-10-25 09.47.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-25%2009.47.58.png)

## 服务端命令

![截屏2024-10-25 09.49.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-25%2009.49.53.png)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-25%2009.48.46.png" alt="截屏2024-10-25 09.48.46" style="zoom:50%;" />

## 客户端命令

### 基本CRUD

**在zookeeper bin 目录下**

`/opt/zooKeeper/apache-zookeeper-3.7.2-bin/bin`

 `./zkCli.sh -server 116.198.203.111:2181` 连接zookeeper客户端

`quit`退出客户端

**节点模型**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-25%2017.05.23.png" alt="截屏2024-10-25 17.05.23" style="zoom:33%;" />

**查看节点** - ls 查看节点下的子节点

```linux
 ls /
[dubbo, services, zookeeper]
[zk: 116.198.203.111:2181(CONNECTED) 2] ls /dubbo
[com.itheima.service.UserService, config, mapping, metadata, org.apache.dubbo.metadata.MetadataService, org.apache.dubbo.mock.api.MockService]
```

**创建节点** - create 路径 附带数据

```
[zk: 116.198.203.111:2181(CONNECTED) 3] create /nodeDemo helloWold
Created /nodeDemo
[zk: 116.198.203.111:2181(CONNECTED) 4] ls /
[dubbo, nodeDemo, services, zookeeper]
```

**查看数据** - get 路径

```
[zk: 116.198.203.111:2181(CONNECTED) 5] get /nodeDemo
helloWold
```

**添加数据** - set 路径 内容

```
[zk: 116.198.203.111:2181(CONNECTED) 6] create /nodeDemo2
Created /nodeDemo2
[zk: 116.198.203.111:2181(CONNECTED) 7] set /nodeDemo2 helloWorld
[zk: 116.198.203.111:2181(CONNECTED) 8] get /nodeDemo2
helloWorld
```

**删除节点 ** - delete 路径 （如果一个节点下面有子节点，无法删除，只能使用deleteall 路径 删除全部）

```
[zk: 116.198.203.111:2181(CONNECTED) 9] ls /
[dubbo, nodeDemo, nodeDemo2, services, zookeeper]
[zk: 116.198.203.111:2181(CONNECTED) 10] delete /nodeDemo2
[zk: 116.198.203.111:2181(CONNECTED) 11] ls /
[dubbo, nodeDemo, services, zookeeper]
```

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-25%2017.14.43.png" alt="截屏2024-10-25 17.14.43" style="zoom:50%;" />

### 创建临时循环节点

**create -e** 创建临时节点 当这次会话结束后，临时节点就消失了

![截屏2024-10-25 17.15.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-25%2017.15.49.png)

**create -s** 创建顺序节点(自动加编号)

![截屏2024-10-25 17.17.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-25%2017.17.12.png)

**create -es** 创建临时顺序节点(所有顺序节点的编号都是共同顺序的)

![截屏2024-10-25 17.18.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-25%2017.18.28.png)

**ls2 ls -s**  查看节点的详细信息

![截屏2024-10-25 17.20.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-25%2017.20.12.png)

![截屏2024-10-25 17.22.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-25%2017.22.36.png)

![截屏2024-10-25 17.22.13](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-25%2017.22.13.png)

# ZooKeeper JavaAPI

## Curator

- Apache ZooKeeper 的java客户端库
- 常见ZooKeeper Java API **原生Java Api   ZkClint    Curator**
- Curator 项目目标是简化zookeeper的客户端使用

**Curator 的版本向下兼容 ZooKeeper**

## 建立连接

**依赖和maven插件**

```xml
<dependencies>

  <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.10</version>
    <scope>test</scope>
  </dependency>

  <!--curator-->
  <dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-framework</artifactId>
    <version>4.0.0</version>
  </dependency>

  <dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-recipes</artifactId>
    <version>4.0.0</version>
  </dependency>
  <!--日志-->
  <dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>1.7.21</version>
  </dependency>

  <dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId>
    <version>1.7.21</version>
  </dependency>

</dependencies>


<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.1</version>
      <configuration>
        <source>1.8</source>
        <target>1.8</target>
      </configuration>
    </plugin>
  </plugins>
</build>
```

```java
@Test
public void testCuratorConnect1() {
    // RetryPolicy的实现类提供多种失败连接的重试策略
    RetryPolicy retry = new ExponentialBackoffRetry(3000, 10);
    //ip端口(集群用，分隔) 会话超时时间 连接超时时间 连接重试策略
    CuratorFramework client = CuratorFrameworkFactory.newClient("116.198.203.111:2181",
            30 * 1000, 15 * 1000, retry);
    client.start(); // 开启连接
}
@Test
public void testCuratorConnect2 (){
    //名称空间namespace 创建这个连接后 默认根节点为 /namespace
    CuratorFramework client = CuratorFrameworkFactory.builder()
            .connectString("116.198.203.111:2181")
            .sessionTimeoutMs(30 * 1000)
            .connectionTimeoutMs(15 * 1000)
            .retryPolicy(new ExponentialBackoffRetry(3000, 10))
            .namespace("sjc")
            .build();
    client.start();
}
```

## 创立节点

```java
/**
 * 创建节点，create 持久 临时 顺序 数据
 * 基本创建
 * 创建带有数据节点
 * 设置节点类型
 * 创建多级节点 /app/node
 */
@Test
public void testCreateNode() throws Exception {
    //基本创建
    //创建节点不指定数据，默认把客户端的ip作为数据存储
    String path1 = client.create().forPath("/node1");//返回路径 没啥用
    System.out.println(path1);
}
@Test
public void testCreateNode2 () throws Exception {
    //带有数据节点
    String path2 = client.create().forPath("/node2" , "Hello World".getBytes());//返回路径 没啥用
    System.out.println(path2);
}
@Test
public void testCreateNode3 () throws Exception {
    //设置节点类型
    //默认 持久化节点
    //此时创建临时节点(传参为枚举对象) 这一次对zookeeper的操作也是一次会话，所以这个程序结束后，临时节点就不存在了
    String path3 = client.create().withMode(CreateMode.EPHEMERAL).forPath("/node3");//返回路径 没啥用
    System.out.println(path3);
}
@Test
public void testCreateNode4 () throws Exception {
    //创建多级节点
    //如果父节点不存在就创建父节点
    String path4 = client.create().creatingParentsIfNeeded().forPath("/node4/node1");//返回路径 没啥用
    System.out.println(path4);
}
```

## 查询节点

```java
/**
 * 查询节点
 * 子节点ls  数据get  状态ls -s
 */
@Test
public void testGet() throws Exception {
    //查询数据
    byte[] bytes = client.getData().forPath("/node2");
    System.out.println(new String(bytes));
}
@Test
public void testGet2() throws Exception {
    //查询子节点
    List<String> strings = client.getChildren().forPath("/");//查询namespace
    System.out.println(strings);
}
@Test
public void testGet3() throws Exception {
    //查询节点信息
    Stat status = new Stat();//空的node状态对象
    //老版本中 get可以一次性获取message 和 status 后面功能降级 但是api没有跟新
    client.getData().storingStatIn(status).forPath("/node2");//填充空的status对象
    System.out.println(status);
}
```

## 修改节点

```java
/**
 * 修改数据
 * 根据版本修改
 */
@Test
public void testSet() throws Exception {
    client.setData().forPath("/node1" , "hello world".getBytes());
}

@Test
public void testSetInVersion () throws Exception {
    String nodePath = "/node1";
    //根据版本修改 类似多线程环境 防止一下子多次修改
    Stat status = new Stat();
    client.getData().storingStatIn(status).forPath(nodePath);

    int version = status.getVersion();
    client.setData().withVersion(version).forPath(nodePath , "hello world".getBytes());
    //保证更改时候的版本和更改时的version是一样的(更改期间没有其他线程操作)
  	//查询和修改是一个原子性操作	
}
```

## 删除节点

```java
/**
 * 删除节点
 * delete
 * deleteAll
 * 必须成功的删除
 * 回调操作
 */
@Test
public void testDelete () throws Exception {
    // delete
    client.delete().forPath("/node1");
}

@Test
public void testDelete2 () throws Exception {
    //delete All
    client.delete().deletingChildrenIfNeeded().forPath("/node4");
}

@Test
public void testDelete3() throws Exception {
    //必须成功的删除
    //如果在删除过程中遇到超时或者客户端短线，会一直采用重试操作，保证删除操作一定成功
    client.delete().guaranteed().forPath("/node2");
}

@Test
public void testDelete4 () throws Exception {
    //回调
    client.delete().guaranteed().inBackground(new BackgroundCallback() {
        @Override
        public void processResult(CuratorFramework curatorFramework, CuratorEvent curatorEvent) throws Exception {
            System.out.println("回调函数执行了");
            //CuratorEvent 中为删除对象的一系列信息
        }
    }).forPath("/node2");
}
```

## Watch监听

### 概述

![截屏2024-10-25 19.48.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-25%2019.48.31.png)

### NodeCache

```java
/**
 * 演示nodeCache
 * 指定一个节点的监听器
 */
@Test
public void testNodeCache() throws Exception {
    // 创建nodeCache对象
    NodeCache cache = new NodeCache(client, "/node1");
    //注册监听
    cache.getListenable().addListener(new NodeCacheListener() {
        @Override
        public void nodeChanged() throws Exception {
            System.out.println("节点发生变化");
            //获取节点修改后的数据
            byte[] data = cache.getCurrentData().getData();
            System.out.println(new String(data));
        }
    });
    //开启监听
    cache.start(true);//true 监听时 加载缓存数据
    System.in.read(); //阻塞线程 服务器中不需要
}
```

### PathChildrenCache

```java
@Test
public void testPathChildrenCache () throws Exception {
    PathChildrenCache pathChildrenCache = new PathChildrenCache(client, "/node1", true);//开启缓存数据

    // 每次一开始会有一次CONNECTION_RECONNECTED的事件
    pathChildrenCache.getListenable().addListener(
            new PathChildrenCacheListener() {
                @Override
                public void childEvent(CuratorFramework curatorFramework, PathChildrenCacheEvent pathChildrenCacheEvent) throws Exception {
                    System.out.println("子节点变化");
                    PathChildrenCacheEvent.Type type = pathChildrenCacheEvent.getType();
                    if (type.equals(PathChildrenCacheEvent.Type.CHILD_UPDATED)) {
                        System.out.println("子节点发生update");
                        String path = pathChildrenCacheEvent.getData().getPath();
                        byte[] data = pathChildrenCacheEvent.getData().getData();
                        System.out.println(path + " --> " + new String(data));
                    }
                }
            }
    );

    pathChildrenCache.start();
    System.in.read();
}
```

### TreeCache

```java
/**
 * 监听某个节点和它的子节点们
 */
@Test
public void testTreeCache () throws Exception {
    TreeCache treeCache = new TreeCache(client, "/node1");
    treeCache.getListenable().addListener(new TreeCacheListener() {

        @Override
        public void childEvent(CuratorFramework curatorFramework, TreeCacheEvent treeCacheEvent) throws Exception {
            System.out.println("节点发生变化");
            System.out.println(treeCacheEvent);
        }
    });
    treeCache.start();
    System.in.read();
}
```



# 分布式锁

## 概念

![截屏2024-10-25 20.25.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-25%2020.25.19.png)

## 原理

**临时 - 防止节点突然宕机，无法释放锁**

**顺序 - 每次需要比较序号判断自己是否获取到了锁**

![截屏2024-10-25 20.31.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-25%2020.31.25.png)

## 模拟12306售票

**可重入锁**（Reentrant Lock）是一种在同一个线程中可以多次获得的锁。在 Java 中，它允许线程在已经持有锁的情况下再次请求该锁而不会被阻塞。可重入锁可以避免**死锁**问题，因为它允许线程在持有锁的情况下递归调用被锁保护的方法或代码块。

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-25%2020.34.45.png" alt="截屏2024-10-25 20.34.45" style="zoom: 50%;" />



<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-25%2020.37.22.png" alt="截屏2024-10-25 20.37.22" style="zoom:50%;" />

```java
public class Ticket12306 implements Runnable{
    private int tickets = 10;
    InterProcessMutex lock;


    public Ticket12306() {
        CuratorFramework client = CuratorFrameworkFactory.builder()
                .connectString("116.198.203.111:2181")
                .sessionTimeoutMs(30 * 1000)
                .connectionTimeoutMs(15 * 1000)
                .retryPolicy(new ExponentialBackoffRetry(3000, 10))
                .namespace("sjc")
                .build();
        client.start();
        lock = new InterProcessMutex(client, "/lock");
    }

    @Override
    public void run() {

        while (true) {
            try {
                //获取锁
                lock.acquire(3 , TimeUnit.SECONDS);//抢锁的等待时间
                if (tickets > 0) {
                    System.out.println(Thread.currentThread() + " : " + tickets--);
                } else {
                    break;
                }
            }catch (Exception e){
                throw new RuntimeException(e);
            }finally {
                //释放锁
                try {
                    lock.release();
                } catch (Exception e) {
                    throw new RuntimeException(e);
                }
            }
        }
    }
}
```

```java
public class LockTest {
    public static void main(String[] args) {
        Ticket12306 ticket12306 = new Ticket12306();

        Thread name1 = new Thread(ticket12306, "name1");
        Thread name2 = new Thread(ticket12306, "name2");

        name2.start();
        name1.start();
    }
}
```

# ZooKeeper 集群

## 介绍

![截屏2024-10-25 20.56.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-25%2020.56.00.png)

## 搭建

`cd /Users/sunnyday/Library/Mobile\ Documents/com\~apple\~CloudDocs/dubbo\ zookeeper/Zookeeper/资料`

![截屏2024-10-25 20.58.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-25%2020.58.42.png)

**集群搭建.md**

## 故障测试

**内容于上述文档**

## 角色

follower observer 发送事物请求到leader leader处理完后 调度各个server 完成数据同步

![截屏2024-10-25 21.09.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-25%2021.09.35.png)





