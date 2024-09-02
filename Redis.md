# 1.redis基础

**NoSQL**

![截屏2024-04-23 19.46.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2019.46.53.png)

**非关系性数据库，结构相对不固定**

![截屏2024-04-23 19.49.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2019.49.58.png)

sql可以ACID  nosql - base

![截屏2024-04-23 19.52.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2019.52.02.png)

![截屏2024-04-23 19.58.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2019.58.43.png)

![截屏2024-04-23 20.28.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2020.28.47.png)

日志文件产生在工作目录下 

![截屏2024-04-23 20.36.26](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2020.36.26.png)



![截屏2024-04-23 20.36.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2020.36.46.png)

![截屏2024-04-23 20.37.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2020.37.08.png)

![截屏2024-04-23 20.37.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2020.37.20.png)

![截屏2024-04-25 10.41.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2010.41.16.png)

## 1.1数据类型

**字符串string** - 普通字符串,Redis中最简单的数据类型

**哈希hash**  - 散列，类似于java中的HashMap

**列表list**  - 按照插入顺序排序，可以有重复元素，类似于Java中的LinkedList

**集合set** - 无序集合，没有重复元素，类似于Java中的HashSet

**有序集合 sorted set / zset** - 集合中每个元素关联一个分数（score），根据分数升序排序，没有重复元素

![image-20240407151821908](https://typora---------image.oss-cn-beijing.aliyuncs.com/image-20240407151821908.jpg)

## 1.2.常用命令

### 1.2.1string

![截屏2024-04-25 11.27.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2011.27.22.png)

`SET key value //设置指定key的值`

`GET key //获取指定key的值`

`SETEX key seconds value 设置指定的key的值，并将key的过期时间设为seconds秒`

`SETNX key value //只有在key不存在的时候设置key的值`

![截屏2024-04-25 11.28.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2011.28.03.png)

![截屏2024-04-25 11.29.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2011.29.15.png)

![截屏2024-04-25 11.29.52](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2011.29.52.png)

![截屏2024-04-25 11.30.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2011.30.29.png)

![截屏2024-04-25 11.32.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2011.32.23.png)

![截屏2024-04-25 11.33.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2011.33.19.png)

#### 1.2.1.1key的层级格式

![截屏2024-04-25 11.36.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2011.36.06.png)

![截屏2024-04-25 11.37.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2011.37.00.png)

![截屏2024-04-25 11.37.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2011.37.25.png)



### 1.2.2hash

![截屏2024-04-25 11.39.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2011.39.50.png)

一个string类型的field和value的映射表，hash适合存储对象:

`HSET key field value `将哈希表key 的字符设为value

`HGET key field` 获取存储在哈希表中指定字段的值

`HDEL key field`删除存储在哈希表中的特定字段

`HKEYS key`获取哈希表中所有字段

`HVALS key`获取哈希表中所有值

![截屏2024-04-25 11.40.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2011.40.01.png)

![截屏2024-04-25 11.41.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2011.41.42.png)

![截屏2024-04-25 11.42.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2011.42.12.png)

![截屏2024-04-25 11.43.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2011.43.39.png)

![截屏2024-04-25 11.44.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2011.44.11.png)

### 1.2.3list

![截屏2024-04-25 11.46.37](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2011.46.37.png)

`LPUSH key value1 [value2]`将一个或者多个值插入到列表头部

`LRANGE key start stop`获取列表指定范围内的元素

`RPOP key`移除并获取列表最后一个元素

`LLEN key`获取列表长度

![截屏2024-04-25 11.46.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2011.46.49.png)

![截屏2024-04-25 11.50.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2011.50.17.png)

![截屏2024-04-25 11.51.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2011.51.11.png)



阻塞式，指定监听的时间

### 1.2.4set

![截屏2024-04-25 13.16.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2013.16.22.png)

`SADD key member1 [member2]`向集合添加一个或者多个成员

`SMEMBERS key`返回集合中的所有成员

`SCARD key`获取集合的成员数

`SINTER key1 [key2]`返回给定所有集合的交集

`SUNION key1 [key2]`返回所有给定集合的并集

`SREM key member1 [member2]`删除集合中的一个或者多个成员

![截屏2024-04-25 13.19.13](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2013.19.13.png)

![截屏2024-04-25 13.18.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2013.18.02.png)



### 1.2.5zset

![截屏2024-04-25 13.22.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2013.22.39.png)

`ZADD key score1 member1 [score2 member2]`向有序集合添加一个或者多个成员

`ZRANGE key start stop [WITHSCORES]`通过索引区间返回有序集合中指定区间内的成员(带分数)

`ZINCRBY key increment member`有序集合中对指定成员的分数加上增量increment

`ZREM key member [member ...]` 移除有序集合中的一个或者多个成员

![截屏2024-04-25 13.22.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2013.22.53.png)

![截屏2024-04-25 13.24.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2013.24.55.png)







### 1.2.6通用命令 

`KEYS pattern`查找所有符合给定模式(pattern)的key----> *  set*(set开头的key)

不建议在生产环境使用，redis单线程，查找过慢会阻塞线程

*所有字符 ？一个字符

![截屏2024-04-25 11.20.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2011.20.23.png)

`EXISTS key`检查给定key是否存在

![截屏2024-04-25 11.23.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2011.23.43.png)

`TYPE key`返回key所存储的值的类型

`DEL key`在key存在的时候删除key

返回值为删除的个数

![截屏2024-04-25 11.23.14](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2011.23.14.png)

`expire `

给一个key设置有效期，到期后会自定删除

由于基于内存存储，缓解内存压力

`ttl`

查看key的有效期

![截屏2024-04-25 11.25.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2011.25.36.png)

有效期为负树的时候

-1 --- 永久有效

其他的为过期了

## 1.3在java中操作redis

### 1.3.1Spring Data Redis

![截屏2024-04-25 15.26.07](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2015.26.07.png)

**导入依赖**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-pool2</artifactId>
</dependency>
```

**配置源**

```yml
spring:
  redis:
    host: 172.16.14.130
    port: 6379
    password: 137321
    lettuce:
      pool:
        max-active: 8
        max-idle: 8
        min-idle: 0
        max-wait: 100ms #虽然默认是这些值，但是不手动配置，连接池就不会生效
```

```yml
spring:
  redis:
    host: localhost
    port: 6379
    database: 0 //默认为0
```

**配置类**

```java
@Bean
public RedisTemplate redisTemplate(RedisConnectionFactory redisConnectionFactory){
    RedisTemplate redisTemplate = new RedisTemplate();
    //设置连接工厂
    redisTemplate.setConnectionFactory(redisConnectionFactory);
    //设置redis key序列化器
    redisTemplate.setKeySerializer(new StringRedisSerializer());
    return redisTemplate;
}
```

![截屏2024-04-25 15.28.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2015.28.34.png)

```java
@SpringBootTest
class RedisSpringbootApplicationTests {

    @Autowired
    private RedisTemplate redisTemplate;
    @Test
    void testString() {
        ValueOperations valueOperations = redisTemplate.opsForValue();

        valueOperations.set("name","google");
				//自动序列化，java操作的set是object对象，不是简单的String-String，底层默认宕对象处理了
        String name = (String)valueOperations.get("name");
        System.out.println(name);
    }

}
```

#### 1.3.1.1序列化问题

![截屏2024-04-25 15.58.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2015.58.28.png)

**自定义序列化方式**

![截屏2024-04-25 16.00.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2016.00.34.png)

```java
@Configuration
public class RedisConfig {
    @Bean
    public RedisTemplate<String,Object> redisTemplate(RedisConnectionFactory connectionFactory){
        //创建RedisTemplate对象
        RedisTemplate<String,Object> template = new RedisTemplate<>();
        //设置连接工厂
        template.setConnectionFactory(connectionFactory);
        //创建JSON序列化工具
        GenericJackson2JsonRedisSerializer jsonRedisSerializer = new GenericJackson2JsonRedisSerializer();
        //设置key的序列化
        template.setKeySerializer(RedisSerializer.string());
        //设置value的序列化
        template.setValueSerializer(jsonRedisSerializer);
        template.setHashKeySerializer(jsonRedisSerializer);
        //返回
        return template;
    }
}
```

```xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.15.2</version>
</dependency>
#若没有mvc要导入这个依赖
```

![截屏2024-04-25 17.57.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2017.57.20.png)

![截屏2024-04-25 18.01.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2018.01.49.png)

![截屏2024-04-25 18.03.33](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2018.03.33.png)

```java
@Test
void testObject() throws JsonProcessingException {
    ValueOperations<String, String> valueOperations = stringRedisTemplate.opsForValue();
    //创建对象
    Student student = new Student("LucyNick",18);
    //手动序列化
    String json = mapper.writeValueAsString(student);
    //写入数据
    valueOperations.set("stu2",json);
    //获取数据
    String resJson = valueOperations.get("stu2");
    //手动序列化
    Student student1 = mapper.readValue(resJson, Student.class);
    System.out.println(student1);
}
```

### 1.3.1string

```java
@Test
void testString(){
    ValueOperations valueOperations = redisTemplate.opsForValue();
    valueOperations.set("city","beijing");
    System.out.println((String)valueOperations.get("city"));
    valueOperations.set("code","1234",3,TimeUnit.MINUTES);
    valueOperations.setIfAbsent("lock","1");
}
```

### 1.3.2hash

```java
@Test
void testHash(){
    HashOperations hashOperations = redisTemplate.opsForHash();
    hashOperations.put("100","name","tom");
    System.out.println((String)hashOperations.get("100","name"));

    hashOperations.keys("100");//Set

    hashOperations.values("100");//Set
    hashOperations.delete("100","name");
}
```

### 1.3.3.list

```java
@Test
void testList(){
    ListOperations listOperations = redisTemplate.opsForList();
    
    listOperations.leftPushAll("myList","1","a");
    listOperations.leftPush("myList","s");
    
    List myList = listOperations.range("myList",0,-1);
    listOperations.rightPop("myList");
    
    Long size = listOperations.size("myList");
}
```

### 1.3.4set

```java
@Test
void testSet(){
    SetOperations setOperations = redisTemplate.opsForSet();
    setOperations.add("set1","a","b","1");
    setOperations.add("set2","a","v");
    
    Set members = setOperations.members("set1");
    
    Long size = setOperations.size("set1");
    
    Set intersect = setOperations.intersect("set1","set2");
    
    Set union = setOperations.union("set1","set2");
    
    setOperations.remove("set1","a","b");
}
```

### 1.3.5zset

```java
@Test
void testZset(){
    ZSetOperations zSetOperations = redisTemplate.opsForZSet();
    
    zSetOperations.add("zset1","a",10);
    zSetOperations.add("zset2",'b',12);
    
    Set zset1 = zSetOperations.range("zset1",0,-1);
    
    zSetOperations.incrementScore("zset1","a",10);
    
    zSetOperations.remove("zset1","a","b");
}
```

### 1.3.6通用命令

```java
 @Test
    void testCommon() {
        Set keys = redisTemplate.keys("*");

        Boolean flag = redisTemplate.hasKey("name");
        DataType type = redisTemplate.type("zet1");
        redisTemplate.delete("mylist");
    }
```

## 1.4redis的java客户端-jedis



![截屏2024-04-25 14.36.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2014.36.51.png)

![截屏2024-04-25 14.38.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2014.38.05.png)

```xml
 <!-- https://mvnrepository.com/artifact/redis.clients/jedis -->
    <dependency>
      <groupId>redis.clients</groupId>
      <artifactId>jedis</artifactId>
      <version>3.7.0</version>
    </dependency>

    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.13.1</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-api</artifactId>
      <version>1.7.30</version>
    </dependency>
    <dependency>
      <groupId>ch.qos.logback</groupId>
      <artifactId>logback-classic</artifactId>
      <version>1.2.3</version>
    </dependency>

  </dependencies>

```

```java
public class JedisTest {
    private Jedis jedis;

    @Before
    public void setUp() {
        //1.建立连接
        jedis = new Jedis("172.16.14.130",6379);
        //2.设置密码
        String PASSWORD = "137321";
        jedis.auth(PASSWORD);
        //3.选择库
        jedis.select(0);
    }

    @Test
    public void testString(){
        String res = jedis.set("name","sjc");
        System.out.println("res -> "+res);

        String name = jedis.get("name");
        System.out.println("name -> "+name);
    }

	 @Test
    public void testHash(){
        jedis.hset("user:1","name","jack");
        jedis.hset("user:1","age","11");


        Map<String, String> map = jedis.hgetAll("user:1");
        System.out.println(map);
    }
    @After
    public void tearDown(){
        if (jedis != null){
            jedis.close();
        }
    }
}
```

### 1.4.1**jedis 连接池**

**jedis本身是线程不安全的，并且频繁的创建和销毁连接有性能损耗，因此我们推荐大家使用jedis连接池代替jedis的直连方式**

```java
public class JedisConnectionFactory {
    private static final JedisPool jedisPool;
    static {
        //配置连接池
        JedisPoolConfig poolConfig = new JedisPoolConfig();
        poolConfig.setMaxTotal(8);
        poolConfig.setMaxIdle(8);
        poolConfig.setMinIdle(0);
        poolConfig.setMaxWaitMillis(1000);
        //创建连接池对象
        jedisPool = new JedisPool(poolConfig,"172.16.14.130",6379,1000,"137321");
    }
    public static Jedis getJedis(){
        return jedisPool.getResource();
    }
}
```

# 2.redis实战

## 2.1短信登录

### 2.1.1基于session的实现登录

![截屏2024-04-25 20.03.32](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2020.03.32.png)

```java
/**
 * 登录功能
 * @param loginForm 登录参数，包含手机号、验证码；或者手机号、密码
 */
@PostMapping("/login")
public Result login(@RequestBody LoginFormDTO loginForm, HttpSession session){
    //实现登录功能
    return userService.login(loginForm,session);
}

/**
 * 登出功能
 * @return 无
 */
@PostMapping("/logout")
public Result logout(){
    // TODO 实现登出功能
    return Result.fail("功能未完成");
}

@GetMapping("/me")
public Result me(){
    // 获取当前登录的用户并返回
    UserDTO user = UserHolder.getUser();
    return Result.ok(user);
}
```

```java
@Override
public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

    HttpSession session = request.getSession();
    Object user = session.getAttribute("user");
    if (user == null){
        response.setStatus(401);
        return false;
    }
    UserHolder.saveUser((UserDTO) user);
    return true;
}

@Override
public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
    UserHolder.removeUser();
}
```

```java
@Override
public Result sendCode(String phone, HttpSession session) {
    //1.校验手机号
    if (RegexUtils.isPhoneInvalid(phone)){
        //2.如果不符合，返回错误信息
        return Result.fail("手机号格式错误");
    }
    //3.符合，生成验证码
    String code = RandomUtil.randomNumbers(6);
    //4.保存验证码到session
    session.setAttribute("code",code);
    //5.发送验证码
    //TODO 暂时不做
    log.debug("send message successfully : {}",code);
    //6.返回ok
    return Result.ok();
}

@Override
public Result login(LoginFormDTO loginForm, HttpSession session) {
    String phone = loginForm.getPhone();
    //1.校验手机号
    if (RegexUtils.isPhoneInvalid(phone)){
        //如果不符合，返回错误信息
        return Result.fail("手机号格式错误");
    }
    //2.校验验证码
    String cacheCode = (String)session.getAttribute("code");
    String code = loginForm.getCode();
    if (cacheCode == null || !cacheCode.equals(code)){
        //3.不一致，报错
        return Result.fail("验证码错误");
    }
    //4.一致，根据手机号查询用户
    User user = query().eq("phone", phone).one();
    //5.判断用户是否存在
    if (user == null){
        //6.不存在，创建新用户并保存
        user = createUserWithPhone(phone);
    }
    //7.保存用户信息到session
    session.setAttribute("user", BeanUtil.copyProperties(user, UserDTO.class));
    return Result.ok();
}

private User createUserWithPhone(String phone) {
    User user = new User();
    user.setPhone(phone);
    //生成随机nickname
    user.setNickName(USER_NICK_NAME_PREFIX+RandomUtil.randomString(10));
    //保存
    save(user);
    return user;
}
```

**problem**

![截屏2024-04-25 21.09.21](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2021.09.21.png)

**代替方案的要求：**

**数据共享**

**内存存储**

**key-value结构**

### 2.1.2reids实现

![截屏2024-04-25 21.14.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2021.14.41.png)

![截屏2024-04-25 21.14.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2021.14.58.png)

![截屏2024-04-25 21.17.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2021.17.57.png)

对于这个令牌有效期的问题，用户登录后，有效期开始30分钟，但是如果傻比用户一直在访问不用拦截器的网页，那么有效期无法被刷新，这时候30分钟到了应该是身份过期了，有问题

![截屏2024-04-25 22.01.26](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-25%2022.01.26.png)

## 2.2商户缓存

### 2.2.1什么是缓存

![截屏2024-04-26 14.20.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-26%2014.20.03.png)

![截屏2024-04-26 14.25.13](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-26%2014.25.13.png)

### 2.2.2添加redis缓存

![截屏2024-04-26 14.28.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-26%2014.28.31.png)

一个对象：

```java
String shopJson = stringRedisTemplate.opsForValue().get(key);

//判断缓存是否存在
if (StrUtil.isNotBlank(shopJson)){
    Shop shop = JSONUtil.toBean(shopJson,Shop.class);
    return Result.ok(shop);
}
```

```java
stringRedisTemplate.opsForValue().set(key,JSONUtil.toJsonStr(shop));
```

多个对象：

```java
String typeCache = stringRedisTemplate.opsForValue().get(key);
if (StrUtil.isNotBlank(typeCache)){
    List<ShopType> res  = objectMapper.readValue(typeCache, new TypeReference<List<ShopType>>() {});
    return Result.ok(res);
}
```

```java
stringRedisTemplate.opsForValue().set(key,objectMapper.writeValueAsString(typeList));
return Result.ok(typeList);
```

### 2.2.3缓存跟新策略

![截屏2024-04-26 15.25.37](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-26%2015.25.37.png)

![截屏2024-04-26 15.25.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-26%2015.27.20.png)

![截屏2024-04-26 15.32.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-26%2015.32.19.png)

可能出现的线程问题





problem1 - 删除缓存后，跟新数据库之前，查询并跟新旧数据到了缓存中

![截屏2024-04-26 15.33.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-26%2015.33.53.png)



problem2 - 缓存中不存在一个数据，查询到后，写入缓存之前，数据库的内容被更改了

![截屏2024-04-26 15.35.32](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-26%2015.35.32.png)



**由于缓存的操作速度远大于数据库的crud，所以方案二发生的概率远低于方案一**

解释 = 在做完数据库的操作后，关于缓存的动作执行非常快，很难有这个间隙使得线程2执行完毕

### 2.2.4缓存穿透

缓存穿透 - 客户端请求的数据在缓存和数据库中都不存在，这样缓存永远不会生效，这些请求都会打到数据库

![截屏2024-04-29 02.05.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-29%2002.05.29.png)

![截屏2024-04-29 02.15.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-29%2002.15.27.png)

### 2.2.5缓存雪崩

![截屏2024-04-29 02.23.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-29%2002.23.57.png)





### 2.2.6缓存击穿

![截屏2024-04-29 02.27.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-29%2002.27.25.png)

![截屏2024-04-29 02.31.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-29%2002.31.25.png)

![截屏2024-04-29 02.31.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-29%2002.31.50.png)

#### 2.2.6.1互斥锁的案例

![截屏2024-04-29 02.36.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-29%2002.36.18.png)

**自带的锁无法完成功能，因为我们无法控制是否获取锁后的具体行为**

解决方案

![截屏2024-04-29 02.37.07](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-29%2002.37.07.png)

```java
private boolean tryLock(String key){
    //要设置默认的过期时间，防止由于特殊原因锁无法被主动释放
    Boolean b = stringRedisTemplate.opsForValue().setIfAbsent(key, "1", 10, TimeUnit.SECONDS);
    //不能直接返回，直接返回拆，可能会有空指针
    return BooleanUtil.isTrue(b);
}
private void unLock(String key){
    stringRedisTemplate.delete(key);
```

**jmeter模拟高并发环境**

![截屏2024-04-29 02.58.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-29%2002.58.34.png)

```java
public Shop queryByWithMutex(Long id){
    Shop shop;
    String key = "cache:shop:" + id;
    //尝试从缓存中获取数据
    String shopJson = stringRedisTemplate.opsForValue().get(key);

    //判断缓存是否存在
    if (StrUtil.isNotBlank(shopJson)){
        return JSONUtil.toBean(shopJson,Shop.class);
    }

    //当缓存为null，代表还没查过数据库
    if (shopJson != null){
        return null;
    }

    String lockKey = "lock:shop:" + id;
    //获取互斥锁
    try {

        boolean isLock = tryLock(lockKey);
        if (!isLock) {
            //获取锁失败，休眠并重试
            Thread.sleep(50);
            return queryByWithMutex(id);
        }
        //缓存不存在在数据库中尝试获取
        shop = getById(id);
        if (shop == null) {
            // 将空值写入redis
            stringRedisTemplate.opsForValue().set(key, "", 3L, TimeUnit.MINUTES);
            return null;
        }
        //成功获取的话需要存入缓存
        stringRedisTemplate.opsForValue().set(key, JSONUtil.toJsonStr(shop), 30L, TimeUnit.MINUTES);
    }catch (Exception e){
        throw new RuntimeException(e);
    }finally {
        unLock(lockKey);
    }
    return shop;
}
```

#### 2.2.6.2基于逻辑过期的方式

**基于逻辑过期的方式解决缓存击穿问题**

![截屏2024-05-05 23.41.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-05%2023.41.57.png)

额外写一个redisData，这样子不用在每个实体类中加入过期时间字段

```java
@Data
@Builder
public class    RedisData {
    private LocalDateTime expireTime;
    private Object data;
}
```

```java
private void saveShop2Redis(Long id,Long expireSeconds){
    String key = "cache:shop:" + id;
    //查询店铺数据
    Shop shop = getById(id);
    //封装逻辑过期时间
    RedisData redisData = RedisData.builder()
            .data(shop)
            .expireTime(LocalDateTime.now().plusSeconds(expireSeconds)).build();
    stringRedisTemplate.opsForValue().set(key,JSONUtil.toJsonStr(redisData));
}
```

```java
public Shop queryByWithLogicalExpire(Long id){
    String key = "cache:shop:" + id;
    //尝试从缓存中获取数据
    String shopJson = stringRedisTemplate.opsForValue().get(key);

    //判断缓存是否存在
    //这里的逻辑是基本都存在的，热点数据提前加入缓存了
    if (StrUtil.isBlank(shopJson)){
        return null;
    }

    //反序列化json为对象
    RedisData bean = JSONUtil.toBean(shopJson, RedisData.class);
    JSONObject jsonObject = (JSONObject) bean.getData();
    Shop shop = JSONUtil.toBean(jsonObject,Shop.class);
    LocalDateTime expireTime = bean.getExpireTime();
    //判断时间是否过期
    if (expireTime.isAfter(LocalDateTime.now())){
        //未过期
        return shop;
    }

    //缓存重建
    //获取互斥锁
    String lockKey = "lock:shop:" + id;
    boolean isLock = tryLock(lockKey);
    if (isLock){
        //开启独立线程，实现缓存重建
        CACHE_REBUILD_EXECUTOR.submit(()->{
            try {
                this.saveShop2Redis(id, 20L);
            }catch (Exception e){
                throw new RuntimeException(e);
            }finally {
                unLock(lockKey);
            }
        });
    }
    //最后都要返回店铺信息
    return shop;
}
```

```java
private static final ExecutorService CACHE_REBUILD_EXECUTOR = Executors.newFixedThreadPool(10);
```

### 2.2.7缓存工具封装

![截屏2024-05-06 00.13.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2000.13.57.png)

```java
public <R,ID> R queryByWithMutex(Long time, TimeUnit unit,String keyPrefix, ID id, Class<R> type, Function<ID,R> dbFallBack){
    R r;
    String key = keyPrefix + id;
    //尝试从缓存中获取数据
    String json = stringRedisTemplate.opsForValue().get(key);

    //判断缓存是否存在
    if (StrUtil.isNotBlank(json)){
        return JSONUtil.toBean(json,type);
    }

    //当缓存为null，代表还没查过数据库
    if (json != null){
        return null;
    }
    //缓存不存在在数据库中尝试获取
    r = dbFallBack.apply(id);
    if (r == null) {
        // 将空值写入redis
        stringRedisTemplate.opsForValue().set(key, "", 3L, TimeUnit.MINUTES);
        return null;
    }
   this.set(key,r,time,unit);
  
    return r;
}
```

![截屏2024-05-06 00.36.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2000.36.06.png)

id -> getById(id)也可以  函数式



```java
public <R,ID> R queryByWithLogicalExpire(String keyPrefix,ID id,Class<R> type,Function<ID,R> dbFallback,Long time, TimeUnit unit){
    String key = keyPrefix + id;
    //尝试从缓存中获取数据
    String shopJson = stringRedisTemplate.opsForValue().get(key);

    //判断缓存是否存在
    //这里的逻辑是基本都存在的，热点数据提前加入缓存了
    if (StrUtil.isBlank(shopJson)){
        return null;
    }

    //反序列化json为对象
    RedisData bean = JSONUtil.toBean(shopJson, RedisData.class);
    JSONObject jsonObject = (JSONObject) bean.getData();
    R r = JSONUtil.toBean(jsonObject,type);
    LocalDateTime expireTime = bean.getExpireTime();
    //判断时间是否过期
    if (expireTime.isAfter(LocalDateTime.now())){
        //未过期
        return r;
    }

    //缓存重建
    //获取互斥锁
    String lockKey = "lock:shop:" + id;
    boolean isLock = tryLock(lockKey);
    if (isLock){
        //开启独立线程，实现缓存重建
        CACHE_REBUILD_EXECUTOR.submit(()->{
            try {
                R r1 = dbFallback.apply(id);
                this.setWithLogicalExpire(key,r1,time,unit);
            }catch (Exception e){
                throw new RuntimeException(e);
            }finally {
                unLock(lockKey);
            }
        });
    }
    //最后都要返回店铺信息
    return r;
}
```

## 2.3优惠券秒杀

### 2.3.1全局唯一ID

![截屏2024-05-06 13.59.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2013.59.50.png)

存在问题 - id的规律太明显，

**受单表数据量的限制 - 因为单表数据量不大，需要存在多表中，如果每张表都是自增的id，那么订单的id可能会出现重复(没有唯一性)**

![截屏2024-05-06 14.04.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2014.04.42.png)

![截屏2024-05-06 14.07.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2014.07.19.png)



```java
private static final long BEGIN_TIMESTAMP = 1640995200L;
private static final int COUNT_BITS = 32;

@Resource
private StringRedisTemplate stringRedisTemplate;
//根据不同的前缀(表示不同业务)
public long nextId(String keyPrefix){
    //生成时间戳
    LocalDateTime now = LocalDateTime.now();
    long nowSecond = now.toEpochSecond(ZoneOffset.UTC);
    long timeStamp = nowSecond - BEGIN_TIMESTAMP;
    //生成序列号
    String date = now.format(DateTimeFormatter.ofPattern("yyyy:MM:dd"));
    //根据订单的日期创建同一个业务的多个id的key，保证单个键的数据不会溢出
    long count = stringRedisTemplate.opsForValue().increment("icr:" + keyPrefix + ":" + date);
    //拼接并返回
    return timeStamp << COUNT_BITS | count; // 位运算和或运算
}
```

```java
private final ExecutorService es = Executors.newFixedThreadPool(500);
@Test
public void testIdWorker() throws InterruptedException {
    CountDownLatch latch = new CountDownLatch(300);
    Runnable task = ()->{
        for (int i = 0;i < 100;i++){
            long id = redisIdWorker.nextId("order");
            System.out.println(id);
        }
        latch.countDown();
    };
    long begin = System.currentTimeMillis();
    for (int i = 0;i < 300;i++){
        es.submit(task);
    }
    latch.await();
    long end = System.currentTimeMillis();
    System.out.println("time = "+(end - begin));
}
```

![截屏2024-05-06 14.46.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2014.46.04.png)



### 2.3.2实现优惠券秒杀下单

![截屏2024-05-06 14.47.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2014.47.51.png)

![截屏2024-05-06 14.56.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2014.56.01.png)



```java
@Transactional
public Result seckillVoucher(Long voucherId) {
    //查询优惠券
    SeckillVoucher voucher = seckillVoucherService.getById(voucherId);

    //判断秒杀的时间是否符合
    if (voucher.getBeginTime().isAfter(LocalDateTime.now()) || voucher.getEndTime().isBefore(LocalDateTime.now())){
        return Result.fail("秒杀时间范围不合法");
    }

    if (voucher.getStock() < 1){
        return Result.fail("库存不足!");
    }

    boolean success = seckillVoucherService.update().setSql("stock = stock - 1").eq("voucher_id",voucherId).update();
    if (!success){
        return Result.fail("库存不足");
    }

    VoucherOrder voucherOrder = new VoucherOrder();
    long orderId = redisIdWorker.nextId("order");
    voucherOrder.setId(orderId);
    voucherOrder.setUserId(UserHolder.getUser().getId());
    voucherOrder.setVoucherId(voucherId);

    save(voucherOrder);
    return Result.ok(orderId);
}
```

### 2.3.3超卖问题

![截屏2024-05-06 15.22.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2015.22.45.png)

![截屏2024-05-06 15.29.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2015.29.31.png)

![截屏2024-05-06 15.33.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2015.33.18.png)

**CAS - compare and set** 

![截屏2024-05-06 15.34.14](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2015.34.14.png)

```java
boolean success = seckillVoucherService.update()
        .setSql("stock = stock - 1")
        .eq("voucher_id",voucherId)
        .eq("stock",voucher.getStock())//乐观锁
        .update();
```

```java
//由于很多线程争抢，当线程没抢到就结束了，抢到的成功率非常低,做一个改进
boolean success = seckillVoucherService.update()
        .setSql("stock = stock - 1")
        .eq("voucher_id",voucherId)
        .gt("stock",0)//库存>0都可以
        .update();
```

### 2.3.4一人一单

修改秒杀业务，要求同一个优惠券，一个用户只能下一单

```java
synchronized (userId.toString().intern()) {//加入或者获取字符串常量池中的对象
    IVoucherOrderService proxy = (IVoucherOrderService) AopContext.currentProxy();
    return proxy.createVoucherOrder(voucherId);
}
```

`AopContext.currentProxy()` 提供了一种在 Spring AOP 中解决同一个类中方法嵌套调用导致 AOP 增强失效问题的方案。

`AopContext.currentProxy()` 是 Spring 框架中的一个静态方法，它用于获取当前线程中正在执行的方法所属的代理对象。在 Spring 的面向切面编程（AOP）中，代理对象通常用于在方法调用的外部进行拦截和增强。**但是，当在同一个类内部的方法之间进行调用时，由于绕过了代理对象，AOP 增强可能会失效。**

例如，如果有一个事务方法 B 被另一个非事务方法 A 调用，**而事务管理是通过 AOP 实现的，那么直接调用 B 方法可能会导致事务失效**。通过使用 `AopContext.currentProxy()`，可以在 A 方法中通过代理对象调用 B 方法，从而确保事务管理仍然有效。

```java
@EnableAspectJAutoProxy(exposeProxy = true)//暴露代理对象
```

![截屏2024-05-06 16.50.26](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2016.50.26.png)

### 2.3.5分布式锁

#### 2.3.5.1简介

**满足分布式系统或集群模式下多进程可见并且互斥的锁**

![截屏2024-05-06 16.53.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2016.53.28.png)

![截屏2024-05-06 16.58.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2016.58.40.png)

#### 2.3.5.2基于redis的分布式锁

![截屏2024-05-06 17.02.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2017.02.58.png)

让获取锁具备原子性 - -  `set lock thread ex 10 nx`

**非阻塞 - 尝试一次，成功返回true，失败返回false**

```java
private final String name;
@Resource
private StringRedisTemplate stringRedisTemplate;
private static final String KEY_PREFIX = "lock:";

public SimpleRedisLock(String name) {
    this.name = name;
}

@Override
public boolean tryLock(long timeoutSec) {
    String key = KEY_PREFIX + name;
    long threadId = Thread.currentThread().getId();
    String value = String.valueOf(threadId);
    //获取锁
    Boolean success = stringRedisTemplate.opsForValue().setIfAbsent(key, value, timeoutSec, TimeUnit.SECONDS);
    return Boolean.TRUE.equals(success);//包装类的拆箱，可能存在包装类为空的情况，会拆蒙蔽
}

@Override
public void unlock() {
    String key = KEY_PREFIX + name;
    stringRedisTemplate.delete(key);
}
```

```java
Long userId = UserHolder.getUser().getId();

SimpleRedisLock lock = new SimpleRedisLock("order:"+userId);//每个用户一个锁
boolean isLock = lock.tryLock(115L);
if (!isLock){
    //获取锁失败
    return Result.fail("不允许重复下单!!!");
}
try {
    IVoucherOrderService proxy = (IVoucherOrderService) AopContext.currentProxy();
    return proxy.createVoucherOrder(voucherId);
}finally {
    lock.unlock();
}
```



**误删的情况：**

**一个线程业务阻塞的时间比锁设置的自动删除的时间还长，导致出现并发问题**

阻塞时间到了后可能把别的线程的锁给释放了

![截屏2024-05-06 17.26.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2017.26.42.png)

**业务阻塞线程在释放锁的时候应该检查这把锁是不是它上的**

![截屏2024-05-06 17.29.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2017.29.27.png)

```java
public class SimpleRedisLock implements ILock{

    private final String name;
    @Resource
    private StringRedisTemplate stringRedisTemplate;
    private static final String KEY_PREFIX = "lock:";
    private static final String ID_PREFIX = UUID.randomUUID().toString()+"-";

    public SimpleRedisLock(String name) {
        this.name = name;
    }

    @Override
    public boolean tryLock(long timeoutSec) {
        String key = KEY_PREFIX + name;
        long threadId = Thread.currentThread().getId();
        String value = ID_PREFIX+ threadId;
        //获取锁
        Boolean success = stringRedisTemplate.opsForValue().setIfAbsent(key, value, timeoutSec, TimeUnit.SECONDS);
        return Boolean.TRUE.equals(success);//包装类的拆箱，可能存在包装类为空的情况，会拆蒙蔽
    }

    @Override
    public void unlock() {
        String key = KEY_PREFIX + name;
        String id = stringRedisTemplate.opsForValue().get(key);
        String value = ID_PREFIX+ Thread.currentThread().getId();
        if (id.equals(value)){
            stringRedisTemplate.delete(key);
        }
    }
}
```



##### Lua脚本

**释放锁堵塞的时候产生的问题**

![截屏2024-05-06 18.42.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2018.42.49.png)

**ok后阻塞，会直接删除锁，但是也是误删的情况**



**lua脚本解决多条命令的原子性问题**

判断锁标识和释放锁应该是一个原子动作



**redis提供lua脚本功能，在一个脚本中编写多条redis命令，确保多条命令执行时的原子性，Lua脚本是一种编程语言**

![截屏2024-05-06 18.49.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2018.49.03.png)

**执行脚本**

![截屏2024-05-06 18.50.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2018.50.56.png)

**如果脚本中的key，value不想写死，可以作为参数传递，key类型参数会放入keys数组，其他参数会放入argv数组，在脚本中可以从keys和argv数组获取这些参数**

**索引从0开始**

![截屏2024-05-06 18.55.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2018.55.25.png)

![截屏2024-05-06 19.00.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2019.00.16.png)

![截屏2024-05-06 20.09.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2020.09.12.png)

```lua
if (redis.call('get',KEYS[1]) == ARGV[1]) then
    -- 释放锁
    return redis.call('del',KEYS[1])
end 
return 0
```

```java
private static final DefaultRedisScript<Long> UNLOCK_SCRIPT;
static {
    UNLOCK_SCRIPT = new DefaultRedisScript<>();
    UNLOCK_SCRIPT.setLocation(new ClassPathResource("unlock.lua"));
    UNLOCK_SCRIPT.setResultType(Long.class);
}
```



```java
@Override
public void unlock() {
    //调用lua脚本
    stringRedisTemplate.execute(UNLOCK_SCRIPT,
            Collections.singletonList(KEY_PREFIX + name),
            ID_PREFIX+ Thread.currentThread().getId());
}
```

##### Redisson

![截屏2024-05-06 20.29.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2020.29.51.png)

![截屏2024-05-06 20.31.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2020.31.29.png)

```xml
<dependency>
    <groupId>org.redisson</groupId>
    <artifactId>redisson</artifactId>
    <version>3.13.6</version>
</dependency>
```

```java
@Configuration
public class RedissonConfig {

    @Bean
    public RedissonClient redissonClient(){
        //配置
        Config config = new Config();
        config.useSingleServer().setAddress("redis://172.16.14.130:6379").setPassword("137321");
        return Redisson.create(config);
    }

}
```

```java
RLock lock = redissonClient.getLock("lock:order:" + userId);
boolean isLock = lock.tryLock();
if (!isLock){
    //获取锁失败
    return Result.fail("不允许重复下单!!!");
}
try {
    IVoucherOrderService proxy = (IVoucherOrderService) AopContext.currentProxy();
    return proxy.createVoucherOrder(voucherId);
}finally {

    lock.unlock();
}
```

![截屏2024-05-06 20.44.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2020.44.54.png)

**redisson可重入锁原理**

**利用hash结构记录线程id和重入次数**

![截屏2024-05-06 20.52.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2020.52.08.png)

![截屏2024-05-06 20.54.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2020.54.57.png)



**redisson不可重试**

利用信号量和PubSub功能实现等待，唤醒，获取锁失败的重试机制

消息订阅

**redisson的watchdog机制**

利用watchdog 每隔一段时间，重置超时时间

![截屏2024-05-06 21.13.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2021.13.35.png)

主从一致性问题

![截屏2024-05-06 21.18.36](/Users/sunnyday/Library/Application Support/typora-user-images/截屏2024-05-06 21.18.36.png)

![截屏2024-05-06 21.20.52](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2021.20.52.png)

![截屏2024-05-06 21.23.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-06%2021.23.05.png)

### 2.3.6Redis优化秒杀

![截屏2024-05-07 14.31.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-07%2014.31.57.png)



![截屏2024-05-07 14.35.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-07%2014.35.15.png)

![截屏2024-05-07 14.36.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-07%2014.36.56.png)

使用java的阻塞队列，使用jvm的内存，会存在内存溢出的情况

任务取出了阻塞队列， 出现了一些问题，没能加入订单(尚未处理)，但是取出后就不会再取了，出现数据安全问题

(当jvm重启的时候，数据无法保存会丢失)

![截屏2024-05-07 15.49.07](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-07%2015.49.07.png)



**lua 的优惠券脚本**

```lua
-- 1.参数列表
--1.1优惠券id
local voucherId = ARGV[1]
--1.2用户id
local userId = ARGV[2]

-- 2.数据key
--2.1库存key
local stockKey = 'seckill:stock:' .. voucherId
--2.2订单key
local orderKey = 'seckill:order' .. voucherId
--3.脚本业务

--3.1判断库存是否充足
--redis获取到的是字符串
if (tonumber(redis.call('get',stockKey)) <= 0) then
    return 1
end
--3.2判断用户是否下单
if (redis.call('sismember',orderKey,userId) == 1) then
    return 2
end
--3.3扣库存
redis.call('incrby',stockKey,-1)
--3.4下单，保存用户
redis.call('sadd',orderKey,userId)
return 0
```

```java
public Result seckillVoucher(Long voucherId) {
    Long userId = UserHolder.getUser().getId();
    //执行lua脚本
    Long res = stringRedisTemplate.execute(
            SECKILL_SCRIPT,
            Collections.emptyList(),
            voucherId.toString(), userId.toString()
    );
    //判断结果是否为0
    int r = res.intValue();
    if (r != 0) {
        return Result.fail(r == 1 ? "库存不足" : "不能重复下单");
    }
    //封装优惠券对象
    long orderId = redisIdWorker.nextId("order");
    VoucherOrder voucherOrder = new VoucherOrder();
    voucherOrder.setId(orderId);
    voucherOrder.setUserId(UserHolder.getUser().getId());
    voucherOrder.setVoucherId(voucherId);
    //放入阻塞队列
    orderTasks.add(voucherOrder);
    //获取代理对象(子线程无法获取)
    proxy = (IVoucherOrderService) AopContext.currentProxy();

    //返回订单id
    return Result.ok(orderId);
}
```

```java
//阻塞队列
private BlockingQueue<VoucherOrder> orderTasks = new ArrayBlockingQueue<>(1024*1024);
//线程池
private ExecutorService SECKILL_ORDER_EXECUTOR = Executors.newSingleThreadExecutor();
//代理对象
private IVoucherOrderService proxy;
@PostConstruct//类初始化完毕后执行
private void init(){
    SECKILL_ORDER_EXECUTOR.submit(new VoucherOrderHandler());
}
private class VoucherOrderHandler implements Runnable{
    @Override
    public void run() {
        while (true){
            try {
                // 获取队列中的信息
                VoucherOrder voucherOrder = orderTasks.take();
                //创建订单
                handleVoucherOrder(voucherOrder);
            }catch (Exception e){
                log.error("处理订单异常",e);
            }
        }
    }
}
```

```java
private void handleVoucherOrder(VoucherOrder voucherOrder) {
    //获取用户
    Long userId = voucherOrder.getUserId();
    RLock lock = redissonClient.getLock("lock:order:" + userId);
    boolean isLock = lock.tryLock();
    if (!isLock){
        //获取锁失败
        log.error("不允许重复下单");
        return;
    }
    try {
        proxy.createVoucherOrder(voucherOrder);
    }finally {
        lock.unlock();
    }
}
```

```java
@Transactional
public void createVoucherOrder(VoucherOrder voucherOrder) {
    Long userId = voucherOrder.getUserId();

    int count = query()
            .eq("user_id", userId)
            .eq("voucher_id", voucherOrder).count();

    if (count > 0) {
        log.error("用户已经购买过了");
    }


    boolean success = seckillVoucherService.update()
            .setSql("stock = stock - 1")
            .eq("voucher_id", voucherOrder)
            .gt("stock", 0)//库存>0都可以
            .update();
    if (!success) {
        log.error("库存不足");
    }

    save(voucherOrder);
}
```

### 2.3.7Redis消息队列实现异步秒杀

![截屏2024-05-07 16.01.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-07%2016.01.39.png)

#### 2.3.7.1list结构

![截屏2024-05-07 16.04.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-07%2016.04.19.png)

![截屏2024-05-07 16.06.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-07%2016.06.56.png)

#### 2.3.7.2PubSub

![截屏2024-05-07 16.09.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-07%2016.09.02.png)

![截屏2024-05-07 16.09.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-07%2016.09.31.png)

![截屏2024-05-07 16.11.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-07%2016.11.36.png)

**优点**

- 采用发布订阅模型，支持多生产多消费

**缺点**

- 不支持数据持久化(list本质不是消息队列)如果发送一个频道，而这个频道没有被任何人订阅，那么消息就丢失了不会被保存
- 无法避免消息丢失
- 消息堆积有上限，超出数据丢失(缓存空间有上限)

#### 2.3.7.3stream

##### 2.3.7.3.1单消费模式

![截屏2024-05-07 16.28.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-07%2016.28.19.png)

![截屏2024-05-07 16.33.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-07%2016.33.02.png)

**数据可以重复读**

![截屏2024-05-07 16.38.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-07%2016.38.11.png)

**当指定起始id为$的时候，代表读取最新消息，如果我们处理一条消息的过程中，又有超过一条以上的消息到达队列，则下次获取时也只能获取到最新的一条，会出现漏读消息的问题**

![截屏2024-05-07 16.44.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-07%2016.44.18.png)

##### 2.3.7.3.2消费者组

![截屏2024-05-07 16.48.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-07%2016.48.20.png)

![截屏2024-05-07 16.49.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-07%2016.49.29.png)

![截屏2024-05-07 16.51.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-07%2016.51.58.png)

**确认消息**![截屏2024-05-07 16.55.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-07%2016.55.17.png)

![截屏2024-05-07 16.56.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-07%2016.56.22.png)

![截屏2024-05-07 16.59.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-07%2016.59.22.png)

![截屏2024-05-07 17.00.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-07%2017.00.15.png)

#### 2.3.7.4java代码

![截屏2024-05-07 17.03.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-07%2017.03.58.png)

```java
private class VoucherOrderHandler implements Runnable{
    @Override
    public void run() {
        while (true){
            try {
                //获取消息队列中的订单信息
                List<MapRecord<String, Object, Object>> list = stringRedisTemplate.opsForStream().read(
                        Consumer.from("g1", "c1"),
                        StreamReadOptions.empty().count(1).block(Duration.ofSeconds(2)),
                        StreamOffset.create("stream.order", ReadOffset.latest())
                );
                //判断消息是否获取成功
                if (list == null || list().isEmpty()){
                    //如果获取失败，说明没有消息，继续下一次循环
                    continue;
                }
                //解析消息中的订单信息
                MapRecord<String, Object, Object> entries = list.get(0);
                Map<Object, Object> value = entries.getValue();
                VoucherOrder voucherOrder = BeanUtil.fillBeanWithMap(value, new VoucherOrder(), true);

                //如果获取成功，可以下单
                
                handleVoucherOrder(voucherOrder);
                //ACK确认
                stringRedisTemplate.opsForStream().acknowledge("stream.order","g1",entries.getId());
            }catch (Exception e){
                log.error("处理订单异常",e);
                handlePendingList();
            }
        }
    }

    private void handlePendingList() {
        while (true){
            try {
                //获取pending-list中的订单信息
                List<MapRecord<String, Object, Object>> list = stringRedisTemplate.opsForStream().read(
                        Consumer.from("g1", "c1"),
                        StreamReadOptions.empty().count(1),
                        StreamOffset.create("stream.order", ReadOffset.from("0"))
                );
                //判断消息是否获取成功
                if (list == null || list().isEmpty()){
                    //如果获取失败，说明没pending-list有消息，结束循环
                    break;
                }
                //解析消息中的订单信息
                MapRecord<String, Object, Object> entries = list.get(0);
                Map<Object, Object> value = entries.getValue();
                VoucherOrder voucherOrder = BeanUtil.fillBeanWithMap(value, new VoucherOrder(), true);

                //如果获取成功，可以下单

                handleVoucherOrder(voucherOrder);
                //ACK确认
                stringRedisTemplate.opsForStream().acknowledge("stream.order","g1",entries.getId());
            }catch (Exception e){
                log.error("处理订单异常",e);
                try {
                    Thread.sleep(2000);
                } catch (InterruptedException ex) {
                    throw new RuntimeException(ex);
                }
            }
        }
    }
}
```

```lua
 -- 1.参数列表
 --1.1优惠券id
 local voucherId = ARGV[1]
 --1.2用户id
 local userId = ARGV[2]
--1.3订单id
 local orderId = ARGV[3]

 -- 2.数据key
 --2.1库存key
 local stockKey = 'seckill:stock:' .. voucherId
 --2.2订单key
 local orderKey = 'seckill:order' .. voucherId
 --3.脚本业务

 --3.1判断库存是否充足
 --redis获取到的是字符串
 if (tonumber(redis.call('get',stockKey)) <= 0) then
     return 1
 end
 --3.2判断用户是否下单
 if (redis.call('sismember',orderKey,userId) == 1) then
     return 2
 end
 --3.3扣库存
 redis.call('incrby',stockKey,-1)
 --3.4下单，保存用户
 redis.call('sadd',orderKey,userId)

 --3.5发送消息到队列中
 redis.call('xadd','stream.orders','*','userId',userId,'voucherId',voucherId,'id',orderId)--写ID的目的是迎合实体类的设计
 return 0
```

## 2.4达人探店

### 2.4.1发布探店笔记

问题不大

### 2.4.2点赞

**需求**

- 同一个用户只能点赞一次，再次点击则取消点赞
- 如果当前用户已经点赞，则点赞按钮高亮显示(前端实现，判断字段Blog类的isLike属性)

```java
@Override
public Result queryBlogById(Long id) {
    //查询blog
    Blog blog = getById(id);
    if (blog == null){
        return Result.fail("笔记不存在");
    }
    queryBlogUser(blog);
    //查询blog是否被点赞了
    isBlogLiked(blog);
    return Result.ok(blog);
}

private void isBlogLiked(Blog blog) {

    Long userId = UserHolder.getUser().getId();
    //判断当前用户是否已经点赞
    String key = "blog:liked:" + blog.getId();
    Boolean b = stringRedisTemplate.opsForSet().isMember(key, userId.toString());
    blog.setIsLike(BooleanUtil.isTrue(b));
}
private void queryBlogUser(Blog blog) {
    Long userId = blog.getUserId();
    User user = userService.getById(userId);
    blog.setName(user.getNickName());
    blog.setIcon(user.getIcon());
}

@Override
public Result likeBlog(Long id) {
    //获取登录用户
    Long userId = UserHolder.getUser().getId();
    //判断当前用户是否已经点赞
    String key = "blog:liked:" + id;
    Boolean b = stringRedisTemplate.opsForSet().isMember(key, userId.toString());

    //如果未点赞，可以点赞
    if (BooleanUtil.isFalse(b)){
        //数据库点赞数+1
        boolean isSuccess = update().setSql("liked = liked + 1").eq("id", id).update();
        //保存用户到redis的set集合
        if (isSuccess){
            stringRedisTemplate.opsForSet().add(key,userId.toString());
        }
    }else{
        //已点赞，取消点赞
        //数据库点赞数-1
        boolean isSuccess = update().setSql("liked = liked - 1").eq("id", id).update();
        //把用户从redis的set集合中移除
        if (isSuccess){
            stringRedisTemplate.opsForSet().remove(key,userId.toString());
        }
    }
    return queryBlogById(id);
}
```

### 2.4.3点赞排行榜

**显示最早点赞的TopN，形成点赞排行榜**

```java
@Override
    public Result queryBlogLikes(Long id) {
        //查询top5的点赞用户
        String key = "blog:liked:" + id;
        Set<String> top5 = stringRedisTemplate.opsForZSet().range(key, 0, 4);
        if (top5 == null || top5.isEmpty()){
            return Result.ok(Collections.emptyList());
        }
        //解析出用户id
        List<Long> ids = top5.stream().map(Long::valueOf).collect(Collectors.toList());
        //根据用户id查询用户
        //顺序会出错 数据库会按照id排序 要自己指定排序的顺序
//        List<UserDTO> collect = userService.listByIds(ids)
//                .stream()
//                .map(user -> BeanUtil.copyProperties(user, UserDTO.class)).collect(Collectors.toList());
       String idStr = StrUtil.join(",",ids);
        List<UserDTO> collect = userService.query().in("id",ids)
                .last("order by field(id," + idStr + ")").list()
                .stream()
                .map(user -> BeanUtil.copyProperties(user, UserDTO.class)).collect(Collectors.toList());

        return Result.ok(collect);
    }
```

## 2.5好友关注

### 2.5.1关注和取关

关注和取关的接口 和 查询是否关注的接口

```java
@Override
public Result follow(Long followUserId, Boolean isFollow) {
    Long userId = UserHolder.getUser().getId();
    //判断关注还是取关
    if (isFollow){
        log.info("{}关注{}",userId,followUserId);
        //关注
        Follow follow = Follow.builder()
                .userId(userId)
                .followUserId(followUserId)
                .createTime(LocalDateTime.now()).build();
        save(follow);
    }else{
        remove(new QueryWrapper<Follow>().eq("user_id",userId).eq("follow_user_id",followUserId));
    }
    return Result.ok();
}

@Override
public Result isFollow(Long followUserId) {
    Long userId = UserHolder.getUser().getId();
    Integer count = query().eq("user_id", userId).eq("follow_user_id", followUserId).count();
    return Result.ok(count > 0);
}
```

ez



### 2.5.2共同关注

使用集合的交集

改造关注接口，把关注的人的id加入到一个set集合中去

```java
@Override
public Result followCommons(Long id) {
    Long userId = UserHolder.getUser().getId();
    String key1 = "follows:" + userId;
    String key2 = "follows:" + id;
    Set<String> set = stringRedisTemplate.opsForSet().intersect(key1, key2);
    if (set == null || set.isEmpty()){
        return Result.ok(Collections.emptyList());
    }
    List<Long> collect = set.stream().map(Long::valueOf).collect(Collectors.toList());
    List<UserDTO> userDTOS = userService.listByIds(collect).stream().map(user -> BeanUtil.copyProperties(user, UserDTO.class)).collect(Collectors.toList());
    return Result.ok(userDTOS);
}
```

### 2.5.3关注推送

#### 2.5.3.1Feed流

**关注推送也叫做Feed流，直译为 投喂，为用户持续的提供 沉浸式 的体验，通过无线下拉刷新获取最新消息**

![截屏2024-05-09 16.16.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-09%2016.16.04.png)

**拉模式，用户只有在读取的时候，才去把关注用户的发件箱中的内容获取，再做一个时间排序，优点是收件箱看完后就可以删除，内存占用空间小，但是每次都要读取+排序数据，耗时久，延迟高**

![截屏2024-05-09 16.19.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-09%2016.19.15.png)

**推模式 关注人直接把信息发送到粉丝的收件箱，这样不用每次获取并排序数据**

**内存占用大，数据冗余，但是速度快**

![截屏2024-05-09 16.21.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-09%2016.21.42.png)

推拉模式

![截屏2024-05-09 16.24.38](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-09%2016.24.38.png)

![截屏2024-05-09 16.24.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-09%2016.24.55.png)

#### 2.5.3.2推模式的关注推送

![截屏2024-05-09 16.27.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-09%2016.27.35.png)

![截屏2024-05-09 16.30.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-09%2016.30.22.png)

![截屏2024-05-09 16.31.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-09%2016.31.58.png)

```java
@Override
public Result saveBlog(Blog blog) {
    // 获取登录用户
    UserDTO user = UserHolder.getUser();
    blog.setUserId(user.getId());
    // 保存探店博文
    boolean isSuccess = save(blog);
    if (!isSuccess){
        return Result.fail("新增笔记失败");
    }
    //查询笔记作者的所有粉丝
    List<Follow> follows = followService.query().eq("follow_user_id", user.getId()).list();
    //推送笔记
    follows.forEach(follow -> {
        Long userId = follow.getUserId();
        String key = "feed:" + userId; 
        stringRedisTemplate.opsForZSet().add(key,blog.getId().toString(),System.currentTimeMillis());
    });
    // 返回id
    return Result.ok(blog.getId());
    
}
```

![截屏2024-05-09 16.48.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-09%2016.48.00.png)

**参数：**

**max - 当前时间戳 ｜ 上一次查询的最小时间戳**

**min - 0(时间的最小值)**

**offset - 0 ｜ 上一次的结果中，与最小值一样的元素的个数**

**count - 固定值**

```java
@Override
public Result queryBlogOfFollow(Long max, Integer offset) {
    //获取当前用户
    Long userId = UserHolder.getUser().getId();
    //查询收件箱
    String key = "feed:" + userId;
    Set<ZSetOperations.TypedTuple<String>> typedTuples = stringRedisTemplate.opsForZSet().reverseRangeByScoreWithScores(
            key, 0, max, offset, 2
    );
    if (typedTuples  == null || typedTuples.isEmpty()){
        return Result.ok();
    }
    //解析数据 blogId score（时间戳） offset
    List<Long> ids = new ArrayList<>(typedTuples.size());
    long minTime = 0;
    int os = 1;
    for (ZSetOperations.TypedTuple<String> tuple : typedTuples) {
        String idStr = tuple.getValue();
        ids.add(Long.valueOf(idStr));
        long time = tuple.getScore().longValue();
        if (minTime == time){
            os += 1;
        }else {
            os = 1;
            minTime = time;
        }
    }
    //根据id查询blog
    String idStr = StrUtil.join(",",ids);
    List<Blog> blogs = query().in("id",ids).last("order by field (id," + idStr + ")").list();
    blogs.forEach(blog -> {
        queryBlogUser(blog);
        isBlogLiked(blog);
    });
    //封装并返回
    ScrollResult r = new ScrollResult();
    r.setList(blogs);
    r.setOffset(os);
    r.setMinTime(minTime);
    return Result.ok(r);
}
```

## 2.6附近商户

### 2.6.1GEO数据结构

![截屏2024-05-09 17.38.38](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-09%2017.38.38.png)

![截屏2024-05-09 17.44.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-09%2017.44.28.png)

![截屏2024-05-09 17.45.21](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-09%2017.45.21.png)

![截屏2024-05-09 17.47.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-09%2017.47.20.png)

### 2.6.2附近商户搜索

![截屏2024-05-09 17.52.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-09%2017.52.30.png)

**添加商户地址到缓存**

```java
@Test
void loadShopData(){
    //1.查询店铺信息
    List<Shop> list = shopService.list();
    //2.把店铺分组，按照typeId分组
    Map<Long,List<Shop>> map = list.stream().collect(Collectors.groupingBy(Shop::getTypeId));
    for (Map.Entry<Long, List<Shop>> longListEntry : map.entrySet()) {
        //获取类型
        Long typeId = longListEntry.getKey();
        String key = "shop:geo:" + typeId;
        //获取同类的店铺集合
        List<Shop> value = longListEntry.getValue();
        //写入redis
        //每次遍历元素直接加入，要建立多次连接，效率低
        //value.forEach(shop -> stringRedisTemplate.opsForGeo().add(key,new Point(shop.getX(),shop.getY()),shop.getId().toString()));

        List<RedisGeoCommands.GeoLocation<String>> locations = new ArrayList<>(value.size());
        value.forEach(
                shop->
                    locations.add(new RedisGeoCommands.GeoLocation<>(shop.getId().toString(),new Point(shop.getX(),shop.getY())))
        );
        stringRedisTemplate.opsForGeo().add(key,locations);
    }
    //分批写入redis
}
```

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-redis</artifactId>
        </exclusion>
        <exclusion>
            <groupId>lettuce-core</groupId>
            <artifactId>io.lettuce</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-redis</artifactId>
    <version>2.6.2</version>
</dependency>
<!-- https://mvnrepository.com/artifact/io.lettuce/lettuce-core -->
<dependency>
    <groupId>io.lettuce</groupId>
    <artifactId>lettuce-core</artifactId>
    <version>6.1.6.RELEASE</version>
</dependency>
```

```java
@Override
public Result queryShopByType(Integer typeId, Integer current, Double x, Double y) {

    //是否需要根据坐标查询
    if  (x == null || y == null) {
        Page<Shop> page = shopService.query()
            .eq("type_id", typeId)
            .page(new Page<>(current, SystemConstants.DEFAULT_PAGE_SIZE));
    }
    //计算分页参数
    int from = (current - 1) * SystemConstants.DEFAULT_PAGE_SIZE;
    int end = current * SystemConstants.DEFAULT_PAGE_SIZE;
    //查询redis，按照距离排序，分页 shopId + distance
    String key = "shop:geo:" + typeId;
    GeoResults<RedisGeoCommands.GeoLocation<String>> results = stringRedisTemplate.opsForGeo().search(key, GeoReference.fromCoordinate(x, y),
            new Distance(5000), RedisGeoCommands.GeoSearchCommandArgs.newGeoSearchArgs().includeDistance()
                    .limit(end)
    );//只能截取0-end
    //解析id
    if (results == null){
        return Result.ok(Collections.emptyList());
    }

    List<GeoResult<RedisGeoCommands.GeoLocation<String>>> content = results.getContent();
    if (content.size() <= from){
        return Result.ok("数据到底了");
    }
    //截取从from开始的位置
    List<Long> ids = new ArrayList<>(content.size());
    Map<String,Distance> distanceMap = new HashMap<>();
    content.stream().skip(from).forEach(result ->{
        String shopIdStr = result.getContent().getName();
        Distance distance = result.getDistance();
        ids.add(Long.valueOf(shopIdStr));
        distanceMap.put(shopIdStr,distance);
    });
    //根据id查询shop
    String idStr = StrUtil.join(",",ids);
    List<Shop> shops = query().in("id", ids).last("order by field(id," + idStr + ")").list();
    //返回
    shops.forEach(shop ->
        shop.setDistance(distanceMap.get(shop.getId().toString()).getValue())
    );
    return Result.ok(shops);
}
```

## 2.7用户签到

### 2.7.1BitMap用法

![截屏2024-05-10 19.07.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-10%2019.07.56.png)

![截屏2024-05-10 19.09.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-10%2019.09.09.png)

**第一天签到**

![截屏2024-05-10 19.12.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-10%2019.12.02.png)



![截屏2024-05-10 19.14.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-10%2019.14.23.png)

```
bitfield bm1 get u2 0 // u无符号 读两位 --> 10（B） -- > 2
```

![截屏2024-05-10 19.15.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-10%2019.15.45.png)

### 2.7.2签到功能

![截屏2024-05-10 20.34.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-10%2020.34.05.png)

​	

```java
public Result sign() {
    //获取当前登录用户
    Long userId = UserHolder.getUser().getId();
    //获取日期
    LocalDateTime now = LocalDateTime.now();
    //拼接key
    String keySuffix = now.format(DateTimeFormatter.ofPattern(":yyyyMM"));
    String key = "sign:" + userId + keySuffix;
    //获取今天是本月的第几天
    int dayOfMonth = now.getDayOfMonth();
    //写入redis
    stringRedisTemplate.opsForValue().setBit(key,dayOfMonth-1,true);
    return Result.ok();
}
```

### 2.7.3签到统计

**从最后一次签到开始向前统计，直到遇到第一次未签到为止，计算总的签到次数，就是连续签到天数**

```java
@Override
public Result signCount() {
    String key = getSignKey();
    log.info("key --> {}",key);
    //获取今天是本月的第几天
    LocalDateTime now = LocalDateTime.now();
    int dayOfMonth = now.getDayOfMonth();
    log.info("dayOfMonth --> {}",dayOfMonth);
    //获取本月截止今天的签到记录，返回的是一个十进制的数字
    List<Long> list = stringRedisTemplate.opsForValue().bitField(key,
            BitFieldSubCommands.create()
                    .get(BitFieldSubCommands.BitFieldType
                            .unsigned(dayOfMonth)).valueAt(0));
    if (list == null || list.isEmpty()){
        return Result.ok(0);
    }
    Long num = list.get(0);
    if (num == null || num == 0){
        return Result.ok(0);
    }
    //循环遍历
    int count = 0;
    while (true){
        //让这个数字和1做位运算，得到最后一个数字的bit位，
        if ((num & 1) == 0 ){
            break;
        }else{
            count++;
            //原数字右移一位
            num >>>= 1;
        }
    }
    return Result.ok(count);
}
```

## 2.8UV统计

### 2.8.1HyperLogLog用法

![截屏2024-05-10 21.23.52](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-10%2021.23.52.png)

![截屏2024-05-10 21.25.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-10%2021.25.54.png)

**不会统计重复数据 ---- uv**

![截屏2024-05-10 21.29.10](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-10%2021.29.10.png)

### 2.8.2实现UV统计

```java
@Test
void testHyperLogLog(){
    String[] users = new String[1000];
    int index = 0;
    
    for (int i = 0;i <= 1000000 ; i++){
        users[index ++] = "user_" + i;
        if (i % 1000 == 0){
            index = 0;
            stringRedisTemplate.opsForHyperLogLog().add("hll1",users);
        }
    }
    
    Long size = stringRedisTemplate.opsForHyperLogLog().size("hll1");
    System.out.println(size);
}
```

# 3.高级篇

## 3.1分布式缓存

### 3.1.1单点redis问题

- redis是内存存储，服务重启可能会丢失数据 -> **实现redis数据持久化**
- 并发能力问题 - 单节点redis并发能力虽然不错，但也无法满足如618这样的高并发场景 -> **搭建主从集群，实现读写分离**
- 故障恢复问题- 如果redis宕机，则服务不可用，需要一种自动的恢复手段 -> **利用redis哨兵，实现健康检测和自动恢复**
- 存储能力问题- redis基于内存，单节点能存储的数据量难以满足海量数据需求 -> **搭建分片集群，利用插槽机制实现动态扩容**

### 3.1.2Redis持久化

#### 3.1.2.1RDB持久化

![截屏2024-05-13 14.25.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2014.25.23.png)

**快照文件称为RDB文件，默认保存在当前运行目录**

![截屏2024-05-13 14.27.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2014.27.11.png)

**save 如果较慢，会阻塞其他操作，适合在redis停机前使用**

**bgsave异步执行，适合在redis运行中执行**

默认停机前(优雅停止) 自动save，保存rdb文件

![截屏2024-05-13 14.30.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2014.30.39.png)

**在一次redis结束后，产生一个rdb文件，在启动redis之前修改了配置文件中rdb文件的名字，则再次开机的时候会无法读取rdb文件，导致之前的redis内容丢失**

 



- **bgsave开始时会fork主进程得到子进程，子进程共享主进程的内存数据，完成fork后读取内存数据并写入RDB文件**

- **linux中所有的进程都无法直接操作物理内存，由操作系统为每个进程分配虚拟内存，并维护一个虚拟内存和物理内存的映射关系表**

**(页表)**

- **fork的过程不是把内存数据做拷贝而是把页表做拷贝**

- **拷贝使得数据间产生新的映射关系，对不同数据采用新的映射关系读写**

- **极端情况下，子进程写的操作太慢了，在这个过程中，不断地有新的请求来修改数据，可能导致redis的内存占用翻倍，所以要预留一些内存空间，不能把内存全部分配给redis，否在redis在rdb的时候，可能内存溢出**

![截屏2024-05-13 14.39.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2014.39.23.png)



![截屏2024-05-13 14.47.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2014.47.09.png)

#### 3.1.2.2AOF持久化

![截屏2024-05-13 14.49.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2014.49.25.png)

![截屏2024-05-13 14.50.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2014.50.00.png)

**always只有在写入和执行写命令都执行完后才返回结果，安全性好**

**aof缓冲区在内存中，读效率高**



**aof记录的是操作，rdb记录的是值，aof的内存会大很多，因为对相同一个值做很多次操作，会在aof中重复记录多次**

![截屏2024-05-13 14.56.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2014.56.35.png)

**异步执行**

![截屏2024-05-13 14.56.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2014.56.50.png)

**重写的时机**

![截屏2024-05-13 14.58.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2014.58.09.png)

#### 3.1.2.3两者对比

![截屏2024-05-13 14.59.59](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2014.59.59.png)

### 3.1.3Redis主从

#### 3.1.3.1搭建主从架构

**redis以读为主**

![截屏2024-05-13 15.05.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2015.05.05.png)

**在同一台虚拟机中开启3个redis实例，模拟主从集群**

**1 .**

![截屏2024-05-13 15.34.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2015.34.02.png)

**2.**

**把配置文件复制过来**

**修改配置文件**

![截屏2024-05-13 15.43.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2015.43.01.png)

**3.**

![截屏2024-05-13 15.44.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2015.44.49.png)

**4.**

![截屏2024-05-13 15.56.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2015.56.03.png)

![截屏2024-05-13 16.04.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2016.04.48.png)

**5.开启主从关系**

![截屏2024-05-13 16.06.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2016.06.23.png)

还有很关键的一步 在我保存的博客里（master_link_status显示为down）

#### 3.1.3.2主从数据同步原理

**主节点可以写入，并且从节点也可以看见，但是从节点不能读取数据**

##### 3.1.3.2.1全量同步

全量同步比较消耗性能，涉及rdb

![截屏2024-05-13 17.34.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2017.34.25.png)

![截屏2024-05-13 17.38.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2017.38.25.png)

![截屏2024-05-13 17.40.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2017.40.45.png)

![截屏2024-05-13 17.41.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2017.41.53.png)

##### 3.1.3.2.2增量同步

**repl_baklog本质是一个特殊的数组，'环形'，大小有限，记满后会从头开始再覆盖**

![截屏2024-05-13 17.47.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2017.47.22.png)

![截屏2024-05-13 17.47.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2017.47.39.png)

**当未备份的数据被覆盖的时候，offset就被新的偏移量覆盖了，这样子在repl_baklog文件中就无法找到offset了，就需要进行全量同步了**

**磁盘慢 网络快- 减少一次磁盘io流，直接通过网络发送到slave(对于第一条)**

![截屏2024-05-13 17.50.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2017.50.17.png)

![截屏2024-05-13 17.53.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2017.53.57.png)

### 3.1.4Redis哨兵

#### 3.1.4.1作用和原理

![截屏2024-05-13 18.00.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2018.00.04.png)

![截屏2024-05-13 18.02.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2018.02.01.png)

![截屏2024-05-13 18.03.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2018.03.34.png)

![截屏2024-05-13 18.04.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2018.04.40.png)

![截屏2024-05-13 18.05.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2018.05.12.png)3.1.4.2搭建哨兵集群

#### 3.1.4.2搭建哨兵集群

**STEP 1**

![截屏2024-05-13 18.06.59](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2018.06.59.png)

**STEP 2**

![截屏2024-05-13 18.08.37](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2018.08.37.png)

**STEP 3**

![截屏2024-05-13 18.10.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2018.10.16.png)

#### 3.1.4.3RedisTemplate的哨兵模式

**1 - 引入依赖**

```xml
<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

**2 - 配置文件**

```yml
spring:
	redis:
		sentinel:
			master: mymaster #指定master名称
			nodes: # 指定redis-sentinel集群信息
				- 172.16.14.130:27001
				- 172.16.14.130:27002
				- 172.16.14.130:27003
```

![截屏2024-05-13 19.41.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2019.41.00.png)

### 3.1.5Redis分片集群

集群模式下

```linux
redis-cli -c -p 7001 #-c一定要加代表集群模式
```

#### 3.1.5.1搭建分片集群

​		![截屏2024-05-13 19.50.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2019.50.01.png)

**STEP - 1**

![截屏2024-05-13 19.50.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2019.50.47.png)

**STEP - 2**

![截屏2024-05-13 19.52.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2019.52.06.png)

**STEP - 3**

![截屏2024-05-13 19.56.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2019.56.29.png)

![截屏2024-05-13 19.56.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2019.56.55.png)

![截屏2024-05-13 19.58.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2019.58.45.png)

#### 3.1.5.2散列插槽

![截屏2024-05-13 20.01.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2020.01.05.png)

**redis的主节点可能出现宕机，如果一个节点删除，数据随之丢失，如果数据和插槽绑定，将节点对应的插槽转移到活着的节点，数据跟着插槽走，永远可以找到它的位置**

![截屏2024-05-13 20.07.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2020.07.05.png)

#### 3.1.5.3集群伸缩

![截屏2024-05-13 20.07.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2020.07.58.png)

![截屏2024-05-13 20.09.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2020.09.47.png)

![截屏2024-05-13 20.11.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2020.11.17.png)

![截屏2024-05-13 20.12.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2020.12.58.png)

![截屏2024-05-13 20.13.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2020.13.50.png)

#### 3.1.5.4故障转移

![截屏2024-05-13 20.16.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2020.16.58.png)

![截屏2024-05-13 20.18.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2020.18.53.png)

​		![截屏2024-05-13 20.19.33](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2020.19.33.png)

#### 3.1.5.5RedisTemplate访问分片集群

**RedisTemplate 底层同样基于lettuce实现了分片集群的支持，而使用的步骤与哨兵模式基本一致**

- **引入依赖**
- 配置分片集群地址
- **配置读写分离**

**与哨兵模式相比，其中只有分片集群的配置方式略有差异**

```yml
spring:
	redis:
		cluster:
			nodes: # 指定分片集群的每一个节点信息
				-172.16.14.130:7001
				-172.16.14.130:7002
				-172.16.14.130:7003
				-172.16.14.130:8001
				-172.16.14.130:8002
				-172.16.14.130:8003
```

## 3.2多级缓存

**亿级流量的缓存方案**

![截屏2024-05-13 20.29.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2020.29.45.png)

![截屏2024-05-13 20.33.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2020.33.09.png)

### 3.2.1JVM进程缓存

#### 3.2.1.1商品案例

![截屏2024-05-13 21.09.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2021.09.03.png)

![截屏2024-05-13 21.10.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2021.10.40.png)

#### 3.2.1.2初识Caffeine

![截屏2024-05-13 21.13.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2021.13.00.png)

```java
@Test
void testBasicOps() {
    // 创建缓存对象
    Cache<String, String> cache = Caffeine.newBuilder().build();

    // 存数据
    cache.put("gf", "迪丽热巴");

    // 取数据，不存在则返回null
    String gf = cache.getIfPresent("gf");
    System.out.println("gf = " + gf);

    // 取数据，不存在则去数据库查询
    String defaultGF = cache.get("defaultGF", key -> {
        // 这里可以去数据库根据 key查询value
        return "柳岩";
    });
    System.out.println("defaultGF = " + defaultGF);
}
```

![截屏2024-05-13 21.24.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2021.24.28.png)

#### 3.2.1.3实现进程缓存

![截屏2024-05-13 21.27.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2021.27.04.png)

```java
@Configuration
public class CaffeineConfig {
    @Bean
    public Cache<Long, Item> itemCache(){
        return Caffeine.newBuilder()
                .initialCapacity(100)
                .maximumSize(10_000)
                .build();
    }
    @Bean
    public Cache<Long, ItemStock> stockCache(){
        return Caffeine.newBuilder()
                .initialCapacity(100)
                .maximumSize(10_000)
                .build();
    }
    
}
```

```java
@GetMapping("/{id}")
public Item findById(@PathVariable("id") Long id){
    return itemCache.get(id, key -> 
        itemService.query()
                .ne("status", 3).eq("id", key)
                .one()
    );
}

@GetMapping("/stock/{id}")
public ItemStock findStockById(@PathVariable("id") Long id){
    return stockCache.get(id,key -> stockService.getById(key));
}
```

**没有的东西查数据库后会自动加入缓存**

### 3.2.2Lua语法入门

#### 3.2.2.1初始lua

**nginx + lua**

![截屏2024-05-13 21.42.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2021.42.15.png)

![截屏2024-05-13 21.43.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2021.43.42.png)

#### 3.2.2.2变量和循环

![截屏2024-05-13 21.48.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2021.48.20.png)

![截屏2024-05-13 21.49.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2021.49.19.png)

​	![截屏2024-05-13 21.50.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2021.50.29.png)

![截屏2024-05-13 21.51.14](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2021.51.14.png)

**.. 拼接字符串**

![截屏2024-05-13 21.52.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2021.52.15.png)



**ipairs 和 pairs**

![截屏2024-05-13 21.53.37](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2021.53.37.png)

#### 3.2.2.3函数和条件

![截屏2024-05-13 21.56.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2021.56.05.png)

![截屏2024-05-13 21.57.38](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2021.57.38.png)



![截屏2024-05-13 21.58.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2021.58.01.png)

**若数组为空 -> nil 则取反为正**

 ![截屏2024-05-13 21.59.21](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-13%2021.59.21.png)

### 3.2.3实现多级缓存

#### 3.2.3.1安装OpenResty

![截屏2024-05-14 14.56.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2014.56.00.png)

https://openresty.org/cn/linux-packages.html#ubuntu

**安装文档**

https://blog.csdn.net/weixin_45129599/article/details/128802397?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522171567005116800226591014%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=171567005116800226591014&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-14-128802397-null-null.142^v100^pc_search_result_base7&utm_term=ubuntu安装OpenResty&spm=1018.2226.3001.4187

![截屏2024-05-14 15.15.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2015.15.56.png)



#### 3.2.3.2OpenResty快速入门

**反向代理**

![截屏2024-05-14 15.18.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2015.18.54.png)

![截屏2024-05-14 15.20.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2015.20.34.png)

**lua文件夹在openresty的nginx目录下**

![截屏2024-05-14 16.45.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2016.45.31.png)

**nginx监听 反向代理给 openresty 其通过lua作出回答**

#### 3.2.3.3请求参数处理

![截屏2024-05-14 16.48.37](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2016.48.37.png)

![截屏2024-05-14 16.51.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2016.51.12.png)

![截屏2024-05-14 16.52.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2016.52.49.png)

#### 3.2.3.4查询Tomcat

##### 3.2.3.4.1封装http请求

![截屏2024-05-14 16.55.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2016.55.25.png)

**数据查询 + 封装**

![截屏2024-05-14 17.59.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2017.59.22.png)

![截屏2024-05-14 18.00.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2018.00.50.png)

**lualib下可以直接写文件名**

**json序列和反序列化模块**

![截屏2024-05-14 18.08.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2018.08.36.png)

![截屏2024-05-14 18.10.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2018.10.04.png)

##### **3.2.3.4.2对tomcat集群负载均衡**

**负载均衡算法**

对路径做hash运算，结果对tomcat数量取余，这样子同样的请求一定使用指向同样的tomcat

![截屏2024-05-14 18.16.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2018.16.35.png)

![截屏2024-05-14 18.17.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2018.17.17.png)

**启动2个tomcat**

![截屏2024-05-14 18.17.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2018.17.48.png)

#### 3.2.3.5Redis缓存预热

![截屏2024-05-14 18.21.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2018.21.31.png)

![截屏2024-05-14 18.22.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2018.22.50.png)

**实现InitializingBean接口，在bean创建完，autowaired注入以后，执行afterPropertiesSet方法**

![截屏2024-05-14 18.26.59](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2018.26.59.png)

#### 3.2.3.6查询Redis缓存

**三个超时时间分别为**

- 建立连接的超时时间

- 发送请求的超时时间

- 相应结果的超时时间

![截屏2024-05-14 18.29.32](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2018.29.32.png)

![截屏2024-05-14 18.34.59](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2018.34.59.png)

![截屏2024-05-14 18.38.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2018.38.16.png)

![截屏2024-05-14 18.40.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2018.40.51.png)

#### 3.2.3.7Nginx本地缓存

![截屏2024-05-14 18.43.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2018.43.01.png)

**nginx中有一个matser进程和多个worker进程**

![截屏2024-05-14 18.46.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2018.46.09.png)

![截屏2024-05-14 18.48.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2018.48.17.png)

### 3.2.4缓存同步策略

#### 3.2.4.1数据同步策略

![截屏2024-05-14 18.54.14](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2018.54.14.png)

![截屏2024-05-14 18.57.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2018.57.00.png)

![截屏2024-05-14 18.57.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2018.57.19.png)

#### 3.2.4.2安装Canal

![截屏2024-05-14 19.00.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2019.00.16.png)

![截屏2024-05-14 19.01.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2019.01.25.png)

**开启binlog(mysql)**

![截屏2024-05-14 19.02.24](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2019.02.24.png)

**设置用户权限**

![截屏2024-05-14 19.04.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2019.04.08.png)

**创建网络**

![截屏2024-05-14 19.05.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2019.05.19.png)

**创建Canal容器**

![截屏2024-05-14 19.06.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2019.06.27.png)

![截屏2024-05-14 19.07.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2019.07.45.png)

#### 3.2.4.3监听Canal

**Canal提供了各种语言的客户端，当Canal监听到binlog变化时，会通知Canal的客户端**

**编写Canal客户端，监听到变化后，跟新Redis**

![截屏2024-05-14 19.10.38](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2019.10.38.png)

![截屏2024-05-14 19.13.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2019.13.09.png)

![截屏2024-05-14 19.14.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2019.14.11.png)

**在redisHandler中封装增删操作**

![截屏2024-05-14 19.16.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2019.16.30.png)

不要忘记jvm进程缓存，使用put和invalidate

### 3.2.5总结

![截屏2024-05-14 19.22.38](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2019.22.38.png)

## 3.3Redis最佳实践

### 3.3.1Redis键值设计

#### 3.3.1.1优雅的key结构

**最好遵循以下最佳实践约定**

- 遵循基本格式：[业务名称]:[数据名]:[id]
- 长度不超过44字节 - 占用空间小
- 不包含特殊字符 - 避免出现bug

**优点**

- 可读性强
- 避免key冲突
- 方便管理(redis客户端中有层级结构)
- 更节省内存空间，key是string类型，底层编码包含int(全是树枝)，embstr和raw(**不连续，有指针，访问影响，有内存碎片**)三种。embstr在小于44字节使用，采用**连续内存空间**，内存占用空间更小(redis3.x为39字节)

#### 3.3.1.2拒绝BigKey

![截屏2024-05-14 20.33.10](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2020.33.10.png)

![截屏2024-05-14 20.35.07](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2020.35.07.png)

- **2 - 集群要数据分片，数据拆分的最小单位为key，key的分布取决于插槽，插槽是创建集群由redis自动均衡分布，有bigkey的机器数据量远超别人，内存使用率太高**
- **3 - redis单线程 无法处理其他请求**

![截屏2024-05-14 20.38.10](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2020.38.10.png)

**memory usage会对cpu会有很大的消耗**

**scan扫描一次指定一个0，会返回执行到的id，下次扫描从那个id开始**

![截屏2024-05-14 20.44.52](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2020.44.52.png)

```java
import com.heima.jedis.util.JedisConnectionFactory;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.ScanResult;
 
import java.util.HashMap;
import java.util.List;
import java.util.Map;
 
public class JedisTest {
    private Jedis jedis;
 
    @BeforeEach
    void setUp() {
        // 1.建立连接
        // jedis = new Jedis("192.168.150.101", 6379);
        jedis = JedisConnectionFactory.getJedis();
        // 2.设置密码
        jedis.auth("123321");
        // 3.选择库
        jedis.select(0);
    }
 
    final static int STR_MAX_LEN = 10 * 1024;
    final static int HASH_MAX_LEN = 500;
 
    @Test
    void testScan() {
        int maxLen = 0;
        long len = 0;
 
        String cursor = "0";
        do {
            // 扫描并获取一部分key
            ScanResult<String> result = jedis.scan(cursor);
            // 记录cursor
            cursor = result.getCursor();
            List<String> list = result.getResult();
            if (list == null || list.isEmpty()) {
                break;
            }
            // 遍历
            for (String key : list) {
                // 判断key的类型
                String type = jedis.type(key);
                switch (type) {
                    case "string":
                        len = jedis.strlen(key);
                        maxLen = STR_MAX_LEN;
                        break;
                    case "hash":
                        len = jedis.hlen(key);
                        maxLen = HASH_MAX_LEN;
                        break;
                    case "list":
                        len = jedis.llen(key);
                        maxLen = HASH_MAX_LEN;
                        break;
                    case "set":
                        len = jedis.scard(key);
                        maxLen = HASH_MAX_LEN;
                        break;
                    case "zset":
                        len = jedis.zcard(key);
                        maxLen = HASH_MAX_LEN;
                        break;
                    default:
                        break;
                }
                if (len >= maxLen) {
                    System.out.printf("Found big key : %s, type: %s, length or size: %d %n", key, type, len);
                }
            }
        } while (!cursor.equals("0"));
    }
    
    @AfterEach
    void tearDown() {
        if (jedis != null) {
            jedis.close();
        }
    }
 
}
```

**第三方工具为离线分析(时效性不好)**

![截屏2024-05-14 20.51.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2020.51.18.png)

![截屏2024-05-14 20.51.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2020.51.46.png)

集合扫描 类似 scankey

![截屏2024-05-14 20.52.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2020.52.56.png)

#### 3.3.1.3恰当的数据类型

**数据耦合，关联度高，一个字段改变，要改变一整个json字符串**

**数据存在源信息(与数据无关的信息) 导致键值对内存变大**

![截屏2024-05-14 20.56.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2020.56.51.png)



![截屏2024-05-14 20.59.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2020.59.25.png)

**修改配置**

![截屏2024-05-14 21.00.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2021.00.01.png)

![截屏2024-05-14 21.00.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2021.00.58.png)

![截屏2024-05-14 21.03.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2021.03.04.png)

### 3.3.2批处理优化

#### 3.3.2.1Pipeline

![截屏2024-05-14 21.09.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2021.09.23.png)

**大量时间花在网络传输上**

![截屏2024-05-14 21.10.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2021.10.42.png)

![截屏2024-05-14 21.12.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2021.12.00.png)

![截屏2024-05-14 21.14.13](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2021.14.13.png)

![截屏2024-05-14 21.15.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2021.15.56.png)

**pipeline的多个命令之间不具备原子性，可能会被其他的redis命令'插队'** 但是执行速度也可以接受

#### 3.3.2.2集群下的批处理

![截屏2024-05-14 21.20.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2021.20.28.png)	![截屏2024-05-14 21.20.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2021.20.40.png)

![截屏2024-05-14 21.20.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2021.20.57.png)

![截屏2024-05-14 21.25.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2021.25.11.png)

**spring客户端解决了这个问题**

![截屏2024-05-14 21.26.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2021.26.36.png)

### 3.3.3服务端优化

#### 3.3.3.1持久化配置

![截屏2024-05-14 21.41.13](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2021.41.13.png)

![截屏2024-05-14 21.38.37](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2021.38.37.png)

#### 3.3.3.2慢查询

**慢查询导致线程阻塞，影响性能，造成业务故障**

![截屏2024-05-14 21.45.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2021.45.30.png)

**这些为动态配置，要永久修改就修改conf文件**

![截屏2024-05-14 22.01.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2022.01.28.png)

#### 3.3.3.3命令及安全配置

**ssh连接服务器时，在主机和linux上分别有一份私钥和公钥，可以实现免密登录**

![截屏2024-05-14 22.06.37](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2022.06.37.png)



**修改持久化的目录变为存放公钥的文件地址**

**再把持久化文件的名字改为公钥的名字**

![截屏2024-05-14 22.09.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2022.09.23.png)

![截屏2024-05-14 22.09.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2022.09.47.png)

rename-command 的原理是把一个命令取一个只有运维人员的知道的名字，这样直接连接就不知道命令

![截屏2024-05-14 22.11.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2022.11.58.png)

```linux
systemctl restart redis#重启redis
```

#### 3.3.3.4内存配置

内存碎片 重启可以解决

![截屏2024-05-14 22.14.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2022.14.34.png)

![截屏2024-05-14 22.17.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2022.17.00.png)



![截屏2024-05-14 22.22.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2022.22.54.png)

**尽可能少bigkey**

**加大redis服务器带宽**



**客户端总述**

![截屏2024-05-14 22.24.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2022.24.15.png)



**客户端详细信息**   -   omem

![截屏2024-05-14 22.24.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2022.24.44.png)

### 3.3.4集群最佳实践

![截屏2024-05-14 22.26.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2022.26.35.png)

![截屏2024-05-14 22.27.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2022.27.02.png)

![截屏2024-05-14 22.31.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-14%2022.31.12.png)

**集群不能做lua事物**



*<u>**单体redis(主从Redis) 已经能达到万级别的QPS 并且也具备很强的高可用性**</u>*

*<u>**如果主从能满足业务需求的情况下，尽量不搭建Redis集群**</u>*

# 4.原理篇

## 4.1数据结构

### 4.1.1动态字符串SDS

**Redis中保存的key是字符串，value往往是字符串或者字符串的集合，可见字符串是Redis中最常用的一种数据结构**



非二进制安全 - 如果字符串中包含特殊字符（\0，字符串结束标识） 则会默认在这里结束，造成错误



![截屏2024-05-15 16.16.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2016.16.45.png)

![截屏2024-05-15 16.22.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2016.22.12.png)



![截屏2024-05-15 16.20.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2016.20.54.png)

![截屏2024-05-15 16.23.37](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2016.23.37.png)

**动态**

为什么要做预分配 - 内存分配的性能消耗问题 linux分为用户空间和内核空间

当我们申请内存的时候应用程序无法直接操作硬件

申请内存和内核进行交互 从用户态切换到内核态

**总之就是 申请内存的动作非常消耗资源** 预分配就是为了提高性能

![截屏2024-05-15 16.30.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2016.30.53.png)

### 4.1.2IntSet

**IntSet是Redis中set集合的一种实现方式**

**基于整数数组来实现，并且具备长度可变，有序等特性**

![截屏2024-05-15 16.36.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2016.36.31.png)

**为什么要去强调这个编码方式 -** 

每个元素的大小一样，方便intset基于数组角标去寻址，快速定位到对应的元素

c语言靠指针寻址

指针8字节大小无符号整数 - 映射物理内存空间

**索引从0开始的原因是其代表这个元素和起始元素的间隔元素的个数**

![截屏2024-05-15 16.48.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2016.48.40.png)

**intset升级**

![截屏2024-05-15 16.51.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2016.51.57.png)

**顺序 把前面的元素扩容的时候，前面元素的新位置会覆盖其原本后面的位置(因为是在连续空间内)**

**倒叙是对原来的最后一个元素，在起始位置的基础上根据index算出其位子，做内存的覆盖（可能是原来的地址也可能是新的位置）**

![截屏2024-05-15 16.59.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2016.59.43.png)

![截屏2024-05-15 17.00.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2017.00.16.png)

**底层采用二分查找方式来查询** 保持顺序

![截屏2024-05-15 17.01.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2017.01.44.png)

### 4.1.3Dict

Dictionary

**Redis是一个键值型(Key - Value - Pair) 的数据库**

**我们可以根据键实现快速的增删改查**

**键与值的映射关系是通过dict来实现的**

由于可能存在哈希冲突 used的大小可能会超过size的值

![截屏2024-05-15 17.08.14](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2017.08.14.png)

**& 和对size做余数是一样的** 得到0 - size的值

hash一样的新元素一定在队首(因为只需要一次操作，队尾要做一次遍历)

![截屏2024-05-15 17.14.26](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2017.14.26.png)

**字典结构**

![截屏2024-05-15 17.17.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2017.17.48.png)

**dict的扩容**

![截屏2024-05-15 17.20.10](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2017.20.10.png)

**两个后台进程 控制 dict_can_resize**

**dict的收缩**

![截屏2024-05-15 17.24.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2017.24.04.png)

**rehash**

![截屏2024-05-15 17.28.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2017.28.05.png)

**渐进式**

**如果数据量过大，为了不阻塞线程**

![截屏2024-05-15 17.34.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2017.34.46.png)

![截屏2024-05-15 17.35.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2017.35.47.png)

### 4.1.4ZipList

**ZipList是一种特殊的‘双端链表’ 由一系列特殊编码的连续内存块组成。可以在任意一端进行压入 \ 弹出操作**

**并且该操作的时间复杂度为O(1)**

- 尾偏移量帮助快速定位到尾节点
- entry的大小不固定，节省内存

![截屏2024-05-15 17.41.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2017.41.55.png)

![截屏2024-05-15 17.42.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2017.42.08.png)

**entry结构**

**倒叙遍历使用previous_entry_length**

![截屏2024-05-15 17.48.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2017.48.36.png)

**encoding编码**

- 字符串

![截屏2024-05-15 17.52.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2017.52.06.png)

总结构 - 

![截屏2024-05-15 17.54.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2017.54.04.png)

- 数字

![截屏2024-05-15 17.58.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2017.58.06.png)

![截屏2024-05-15 17.59.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-15%2017.59.02.png)

**查询性能不行，要限制节点的个数**



**Ziplist的连锁跟新问题**

假设一个节点变化，长度超过了254，后一位记录前一位长度的previous_entry_length长度发生变化，变成了5个字节，导致后面的节点可能都有于这个扩大而也要把previous_entry_length扩大，导致连锁跟新

![截屏2024-05-19 19.56.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-19%2019.56.30.png)

**新增和删除都有可能发生**

![截屏2024-05-19 19.57.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-19%2019.57.35.png)

### 4.1.5QuickList

**内存多是碎片化的，想要找到大片的连续空间很困难 ----> ZipList不能存储大量数据**

![截屏2024-05-20 16.06.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2016.06.00.png)

![截屏2024-05-20 16.07.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2016.07.05.png)

![截屏2024-05-20 16.08.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2016.08.00.png)

![截屏2024-05-20 16.08.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2016.08.11.png)





![截屏2024-05-20 16.10.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2016.10.08.png)

![截屏2024-05-20 16.10.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2016.10.55.png)

不用指针，内存占用减少

### 4.1.6SkipList

![截屏2024-05-20 16.14.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2016.14.30.png)

不同节点层级不一样

![截屏2024-05-20 16.17.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2016.17.53.png)

![截屏2024-05-20 16.18.59](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2016.18.59.png)

![截屏2024-05-20 16.19.21](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2016.19.21.png)

### 4.1.7RedisObject

![截屏2024-05-20 16.23.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2016.23.39.png)

**一个集合使用一个对象头 存储大量数据，这些数据单独如用string存储会在对象头上浪费大量空间**

![截屏2024-05-20 16.24.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2016.24.40.png)

![截屏2024-05-20 16.25.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2016.25.46.png)





### 4.1.8五种数据结构

#### 4.1.8.1String

![截屏2024-05-20 16.55.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2016.55.48.png)

![截屏2024-05-20 16.55.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2016.55.57.png)

**44字节，整个内存刚好为64字节，符合redis内部内存分片的规则(2的n次方)**

**不会产生内存碎片**

使用String 尽量不超过44字节

![截屏2024-05-20 16.58.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2016.58.42.png)

![截屏2024-05-20 16.59.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2016.59.34.png)

![截屏2024-05-20 17.01.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2017.01.00.png)

#### 4.1.8.2List

![截屏2024-05-20 17.03.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2017.03.35.png)

![截屏2024-05-20 17.04.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2017.04.56.png)

![截屏2024-05-20 17.11.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2017.11.11.png)

![截屏2024-05-20 17.11.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2017.11.25.png)

####   4.1.8.3Set

![截屏2024-05-20 17.15.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2017.15.18.png)

![截屏2024-05-20 17.20.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2017.20.44.png)

**set每一次插入元素都会判断这个类型，以及时变更编码方式**

![截屏2024-05-20 17.21.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2017.21.35.png)

![截屏2024-05-20 17.24.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2017.24.03.png)

![截屏2024-05-20 17.25.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2017.25.16.png)

#### 4.1.8.4ZSet

![截屏2024-05-20 17.29.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2017.29.57.png)

![截屏2024-05-20 17.32.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2017.32.27.png)

![截屏2024-05-20 17.34.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2017.34.36.png)

![截屏2024-05-20 17.35.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2017.35.03.png)

![截屏2024-05-20 17.37.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2017.37.05.png)

![截屏2024-05-20 17.38.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2017.38.25.png)

![截屏2024-05-20 17.41.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2017.41.27.png)

#### 4.1.8.5Hash

![截屏2024-05-20 19.06.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2019.06.17.png)

![截屏2024-05-20 19.07.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2019.07.28.png)

![截屏2024-05-20 19.08.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2019.08.04.png)



![截屏2024-05-20 19.08.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2019.08.57.png)

![截屏2024-05-20 19.09.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2019.09.41.png)

![截屏2024-05-20 19.11.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2019.11.56.png)

![截屏2024-05-20 19.13.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2019.13.50.png)

![截屏2024-05-20 19.15.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2019.15.36.png)

**ziplist不要搞太大**

## 4.2网络模型

### 4.2.1用户空间和内核空间

**ubuntu 和 centos是linux的发行版**

**用户应用通过ubuntu / centos 操作linux内核来访问计算机硬件**

![截屏2024-05-20 20.55.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2020.55.30.png)

**内核(操作系统)本质也是一个应用也要占用内存**

32位的操作系统的内存最大值为2 ^ 32

![截屏2024-05-20 21.40.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2021.40.46.png)

![截屏2024-05-20 21.42.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2021.42.42.png)

**linux IO模型用户进程的两个阶段**

1. **等待数据准备就绪**：这是IO操作的第一阶段。在这个阶段，用户进程会等待数据从外部设备（如磁盘、网络等）传输到内核缓冲区。在这个过程中，根据具体的IO模型（如阻塞IO、非阻塞IO等），用户进程可能会处于阻塞状态（即等待数据到达而不进行其他操作）或非阻塞状态（即可以在等待过程中进行其他操作）。
2. **将数据从内核态拷贝到用户态**：当数据在内核缓冲区准备就绪后，用户进程需要将这些数据从内核空间拷贝到用户空间（即应用程序的缓冲区）。这也是IO操作的第二阶段。在这个过程中，同样根据具体的IO模型，用户进程可能会处于阻塞状态或非阻塞状态。

### 4.2.2阻塞IO

用户**读**的模型

![截屏2024-05-20 21.45.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2021.45.48.png)

![截屏2024-05-20 21.47.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2021.47.16.png)

### 4.2.3非阻塞IO

![截屏2024-05-20 21.49.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2021.49.49.png)

**用户调用revform函数后 暂无数据的情况是 非阻塞的**

**但是调用函数后，内核存在数据的情况下，内核拷贝数据到用户空间的过程中是阻塞状态![截屏2024-05-20 21.52.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2021.52.44.png)**

### 4.2.4IO多路复用

#### 4.2.4.1概述

![截屏2024-05-20 22.01.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2022.01.50.png)

**文件描述符**

![截屏2024-05-20 22.04.37](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2022.04.37.png)

理论上来说，这种模型两个阶段也都是阻塞的

但是 select监听的多个FD 都没有数据就绪的概率非常低

而且 一旦某一个(或多个)FD数据就绪，进程循环调用recvfrom来读取FD(数据一定存在) 效率高于非阻塞IO

![截屏2024-05-20 22.08.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2022.08.29.png)

#### 4.2.4.2select

![截屏2024-05-20 22.16.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2022.16.57.png)

**内核无法直接告诉用户空间哪些fd就绪，只能覆盖用户传入的rfds数组，用户再做一次遍历才能知道结果**

![截屏2024-05-20 22.18.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2022.18.42.png)

#### 4.2.4.3 poll

![截屏2024-05-20 22.22.59](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2022.22.59.png)

![截屏2024-05-20 22.23.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2022.23.44.png)

#### 4.2.4.4epoll

**epfd 唯一对应 eventpoll**

![截屏2024-05-20 22.30.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2022.30.17.png)

![截屏2024-05-20 22.34.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2022.34.27.png)



#### 4.2.4.5事件通知机制

![截屏2024-05-20 22.37.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2022.37.00.png)

![截屏2024-05-20 22.38.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2022.38.48.png)

**在将list_head中的数据添加到events中去之后，判断系统的事件通知机制，若为LT会判断数据是否会读完而重新加入到链表中去**

**手动从ET实现LT的时候不能使用阻塞IO，不然会陷入死循环**



LT反复通知可能导致系统性能下降

在多线程背景下，LT可能使得过多线程获取到了list_head中的内容，过多浪费了系统资源(惊群)

![截屏2024-05-20 22.44.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2022.44.46.png)

#### 4.2.4.6web服务流程

![截屏2024-05-20 22.49.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-20%2022.49.25.png)

### 4.2.5信号驱动IO

![截屏2024-05-21 16.45.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2016.45.18.png)

### 4.2.6异步IO

![截屏2024-05-21 16.47.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2016.47.35.png)

![截屏2024-05-21 16.48.10](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2016.48.10.png)

![截屏2024-05-21 16.50.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2016.50.47.png)

### 4.2.7Redis网络模型

#### 4.2.7.1Redis线程问题

**对于redis到底是单线程还是多线程**

- **如果仅仅是Redis的核心业务部分(命令处理) - 单线程**
- **如果是整个redis - 多线程**

![截屏2024-05-21 16.55.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2016.55.01.png)

![截屏2024-05-21 16.56.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2016.56.12.png)

#### 4.2.7.2Redis单线程网络模型

![截屏2024-05-21 17.02.33](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2017.02.33.png)



![截屏2024-05-21 17.26.38](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2017.26.38.png)

![截屏2024-05-21 17.26.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2017.26.47.png)

![截屏2024-05-21 17.30.24](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2017.30.24.png)

![截屏2024-05-21 17.37.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2017.37.50.png)

![截屏2024-05-21 17.38.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2017.38.00.png)

**IO多复用 +  事件派发**

![截屏2024-05-21 17.39.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2017.39.36.png)

**redis单线程的瓶颈主要就是IO读写 -** 

![截屏2024-05-21 17.43.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2017.43.35.png)

**IO读速度受到网络，带宽的影响 效率低**

## 4.3通信协议

### 4.3.1RESP协议

![截屏2024-05-21 17.59.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2017.59.30.png)

![截屏2024-05-21 18.05.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2018.05.02.png)

### 4.3.2Redis客户端

![截屏2024-05-21 18.09.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2018.09.46.png)

![截屏2024-05-21 18.10.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2018.10.20.png)

![截屏2024-05-21 18.17.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2018.17.36.png)

![截屏2024-05-21 18.19.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2018.19.42.png)

![截屏2024-05-21 18.22.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2018.22.08.png)

![截屏2024-05-21 18.23.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2018.23.20.png)



## 4.4内存策略

![截屏2024-05-21 19.55.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2019.55.48.png)

### 4.4.1过期策略

**expire关键字**

![截屏2024-05-21 19.57.24](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2019.57.24.png)

![截屏2024-05-21 19.59.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2019.59.41.png)

![截屏2024-05-21 20.00.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2020.00.18.png)

**过期key的删除策略**

- 惰性删除
- 周期删除

![截屏2024-05-21 20.03.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2020.03.43.png)

![截屏2024-05-21 20.07.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2020.07.44.png)

![截屏2024-05-21 20.10.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2020.10.41.png)

![截屏2024-05-21 20.12.14](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2020.12.14.png)

### 4.4.2淘汰策略

![截屏2024-05-21 20.18.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2020.18.05.png)

**在任何命令之前进行内存的检查，尝试淘汰一部分内存**

![截屏2024-05-21 20.18.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2020.18.54.png)

![截屏2024-05-21 20.19.32](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2020.19.32.png)

![截屏2024-05-21 20.21.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2020.21.12.png)

![截屏2024-05-21 20.24.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2020.24.43.png)

![截屏2024-05-21 20.37.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2020.37.49.png)

 

eviction_pool 为样本 （一开始样本可能不准确，循环次数多了就接近于所有idleTime最大的元素了）

这个池子中的元素会按照某种方式做一个升序排序，值越大的优先淘汰  -- idleTime

![截屏2024-05-21 20.56.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-21%2020.56.04.png)

比较是否可以存入池子的时候，将待比较元素的idleTime和池子中最小的去做比较（池子满了的情况下）
