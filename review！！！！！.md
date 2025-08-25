# 1 - 1.7HashMap的头插法考虑

# 2 - 1.7循环链表hashMap出现原因

# 3 - 脏读，不可重复读，幻读

读到人家没有提交的东西

两次查询的结果不一样(针对数据)

两次查询的结果集不一样(针对数据行)

幻读无法完全避免



普通读配合MVCC不会有幻读问题

如果使用select in shard mode/for update 就会是当前读 不是 快找读 读取数据的真实内容

虽然加了邻键锁，但是只能保证邻键锁之后的事物插入操作被阻塞，但是一旦加了邻键锁，其之前的数据就会被跟新上去



隔离级别一般还是读已提交

读取事物锁定当前数据行，读取完成就会释放锁（每次select 都会创建mvcc）

repeatableRead 每次读取都在事物结束后释放锁，并发度降低，并且有大量邻键锁，效率太低



# 4 - Spring单例bean的发现

- BeanDefinition 进行getScope
- 去singletonObjects当中找
- doublecheck + synchronized 去创建singletonBean
- 发现scope是singleton，不会在bean实例完之后加入到单例池管理

# 5 - 消除stw

- 合理配置堆内存 - **调整堆大小**，防止**新生代太小**，对象创建频繁，频繁gc。调整**新生代和老年代的比例**
- **复用对象** - 线程池 spring
- 避免**大对象**和**长生命周期对象** 减少垃圾回收压力 增加效率
- 减少**静态变量**集合的使用，占用内存，可分配对象内存降低，gc频繁



**工程上**尝试减少**平衡stw**

stw **长到短** 

**单线程到多线程到Shenandoah和ZGC**

stw从**不可控到可控**

**从CMS到G1**



**AzulGC**理论上的 没有stw的垃圾回收器

**C4（Continuously Concurrent Compacting Collector）和GPGC（Generational Pauseless Garbage Collector）**

分代并发 分代压缩

# 6 - 4个拒绝策略和场景

- **AbortPolicy** - 抛出异常，停止系统工作 (任务丢失非常敏感)
- **CallerRunsPolicy** - 任务提交线程运行
- **DiscardPolicy** - 直接丢弃任务
- **DiscardOldestPolicy** - 丢弃任务队列中最老的任务

1 - 任务丢失敏感，并且不会主动对新任务做操作，只会报错，适合金融交易系统

2 - 对任务执行有延迟的容忍度，并且调用方有能力执行，可以达到延缓任务拒绝速度，降级，负载均衡流量调节，数据备份，日志记录(非核心业务)	

3 - 任务丢失可以容忍，低优先级、非核心、可以丢弃的任务，如自动去补齐缓存的工作

4 - 任务随着时效性会失去价值 - 监控工作 实时数据

# 7 - SpringBean装配

- xml 定义 id name class property
- 注解 @ComponentScan去开启注解扫描范围 @Component <context:component-scan>
- @Configuration + @Bean

# 8 - Spring常用注解

- @Component
- @Service
- @Repository
- @Controller
- @Component
- @Bean
- @Autowired
- @Qualifer
- @Primary
- @Resouce
- @Configuration
- @PropertySource - 加载外部配置文件
- @value
- @Aspect
- @Before
- @After
- @Around
- @RestController - @Controller + @ResponseBody restFul风格，返回值直接作为http响应体
- @。。Maping
- @RequestBody - 请求体绑定到参数
- @PathVariable

# 9 - @Tranctional注解

- 异常未被处理或者抛出了非受检异常(RuntimeException的子类) 直接回滚
- 事物嵌套并且传播属性配置不正确 - 方法调用事物方法(是否加入事物还是新创建一个事物，还是不使用事物)
- 多事物源，事物管理没有正确配置(不懂)
- 跨方法调用，事物内部调用没有@Transcational注解的方法，外层事物可能失效
- 事物标注在非public方法上

# 10 - 负载均衡具体使用场景

- 通过nginx的负载均衡将大量请求分散到多台web服务器上
- 微服务架构(应用被拆分成很多独立服务)通过注册中心，将请求分散到不同服务实例上去
- mysql集群通过mycat实现无感的请求负载均衡
- rabbitMQ que被多消费者监听，通过默认的轮询负载均衡数据的传输
- kafka 消费者群数量小于分片的数量，负载均衡





