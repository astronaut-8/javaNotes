# **微服务技术**

黑马飞书文档

https://b11et3un53m.feishu.cn/wiki/NNAtw4CFQijiYakX8tgczWvWn0b

![截屏2024-06-05 14.23.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-05%2014.23.54.png)

# **单体架构**![截屏2024-06-06 10.17.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-06%2010.17.57.png)

# **微服务架构**

![截屏2024-06-06 10.21.33](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-06%2010.21.33.png)

#  **springCloud**

![截屏2024-06-06 10.24.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-06%2010.24.42.png)

# 微服务拆分

## 拆分原则

![截屏2024-06-06 10.49.59](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-06%2010.49.59.png)

![截屏2024-06-06 10.53.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-06%2010.53.58.png)

## 拆分服务

**有两种工程结构：**

- **独立project**
- **Maven聚合**

![截屏2024-06-06 10.55.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-06%2010.55.53.png)

##  远程调用

**网络调用**![截屏2024-06-06 17.10.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-06%2017.10.16.png)

### RestTemplate

![截屏2024-06-06 17.15.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-06%2017.15.55.png)

```java
private void handleCartItems(List<CartVO> vos) {
    // 1.获取商品id
    Set<Long> itemIds = vos.stream().map(CartVO::getItemId).collect(Collectors.toSet());
    // 2.查询商品
    //List<ItemDTO> items = itemService.queryItemByIds(itemIds);
    //发起http请求，得到http相应
    Map<String,String> map = new HashMap<>();
    map.put("ids", CollUtil.join(itemIds, ","));//不能直接new一下再put，那样子是返回put的返回值，为put键的旧值
    log.info("map -> {}",map);
    ResponseEntity<List<ItemDTO>> response = restTemplate.exchange(
            "http://localhost:8081/items?ids={ids}",
            HttpMethod.GET,
            null,
            new ParameterizedTypeReference<List<ItemDTO>>() { //解决list的类型字节码问题
            },
            map);
    //解析响应
    if (! response.getStatusCode().is2xxSuccessful()){
        //查询失败，直接结束
        return;
    }
    List<ItemDTO> items = response.getBody();
    if (CollUtils.isEmpty(items)) {
        return;
    }
    // 3.转为 id 到 item的map
    Map<Long, ItemDTO> itemMap = items.stream().collect(Collectors.toMap(ItemDTO::getId, Function.identity()));
    // 4.写入vo
    for (CartVO v : vos) {
        ItemDTO item = itemMap.get(v.getItemId());
        if (item == null) {
            continue;
        }
        v.setNewPrice(item.getPrice());
        v.setStatus(item.getStatus());
        v.setStock(item.getStock());
    }
}
```

**存在问题**

我的目标 tomcat的服务器有可能为了应付高并发环境 会部署多份 负载均衡 但是写死的url无法更改

- 服务调用者不知道服务器的地址
- 目标服务宕机无法及时处理
- 无法感知服务器的变更(哪些宕机了，或者新增了)
- ![截屏2024-06-06 18.57.44](/Users/sunnyday/Library/Application Support/typora-user-images/截屏2024-06-06 18.57.44.png)

# 服务治理

## 注册中心原理

![截屏2024-06-06 19.04.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-06%2019.04.47.png)

![截屏2024-06-06 19.06.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-06%2019.06.11.png)

## Nacos注册中心

![截屏2024-06-10 21.08.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-10%2021.08.27.png)

## 服务注册

![截屏2024-06-10 21.26.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-10%2021.26.31.png)



![截屏2024-06-10 21.35.14](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-10%2021.35.14.png)

## 服务发现

![截屏2024-06-10 21.36.32](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-10%2021.36.32.png)

```java
private void handleCartItems(List<CartVO> vos) {
    // 1.获取商品id
    Set<Long> itemIds = vos.stream().map(CartVO::getItemId).collect(Collectors.toSet());
    //根据服务名称获取服务对应的实例列表
    List<ServiceInstance> instances = discoveryClient.getInstances("item-service");
    if (CollUtils.isEmpty(instances)){
        return;
    }
    //模拟负载均衡，从服务列表中挑选一个实例
    ServiceInstance instance = instances.get(RandomUtil.randomInt(instances.size()));
    //发起http请求，得到http相应
    Map<String,String> map = new HashMap<>();
    map.put("ids", CollUtil.join(itemIds, ","));
    log.info("map -> {}",map);
    ResponseEntity<List<ItemDTO>> response = restTemplate.exchange(
            instance.getUri() + "/items?ids={ids}",
            HttpMethod.GET,
            null,
            new ParameterizedTypeReference<List<ItemDTO>>() { //解决list的类型字节码问题
            },
            map);
    //解析响应
    if (! response.getStatusCode().is2xxSuccessful()){
        //查询失败，直接结束
        return;
    }
    List<ItemDTO> items = response.getBody();
    if (CollUtils.isEmpty(items)) {
        return;
    }
    // 3.转为 id 到 item的map
    Map<Long, ItemDTO> itemMap = items.stream().collect(Collectors.toMap(ItemDTO::getId, Function.identity()));
    // 4.写入vo
    for (CartVO v : vos) {
        ItemDTO item = itemMap.get(v.getItemId());
        if (item == null) {
            continue;
        }
        v.setNewPrice(item.getPrice());
        v.setStatus(item.getStatus());
        v.setStock(item.getStock());
    }
}
```

# OpenFeign

## 快速入门

![截屏2024-06-10 23.42.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-10%2023.42.08.png)

![截屏2024-06-10 23.42.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-10%2023.42.47.png)

![截屏2024-06-10 23.43.14](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-10%2023.43.14.png)

![截屏2024-06-10 23.44.13](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-10%2023.44.13.png)

```java
@FeignClient("item-service")
public interface ItemClient {
    @GetMapping("/items")
    List<ItemDTO> queryItemByIds(@RequestParam("ids") Collection<Long> ids);
}
```

```java
private void handleCartItems(List<CartVO> vos) {
    // 1.获取商品id
    Set<Long> itemIds = vos.stream().map(CartVO::getItemId).collect(Collectors.toSet());
    List<ItemDTO> items = itemClient.queryItemByIds(itemIds);

    if (CollUtils.isEmpty(items)) {
        return;
    }
    // 3.转为 id 到 item的map
    Map<Long, ItemDTO> itemMap = items.stream().collect(Collectors.toMap(ItemDTO::getId, Function.identity()));
    // 4.写入vo
    for (CartVO v : vos) {
        ItemDTO item = itemMap.get(v.getItemId());
        if (item == null) {
            continue;
        }
        v.setNewPrice(item.getPrice());
        v.setStatus(item.getStatus());
        v.setStock(item.getStock());
    }
}
```



## 连接池

**openFeign默认使用client发送http请求**（java默认的）

**每一次都要创建client对象，效率低**

使用连接池来优化

![截屏2024-06-11 00.14.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-11%2000.14.20.png)

![截屏2024-06-11 00.15.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-11%2000.15.43.png)

```XML
<!--OK http 的依赖 -->
<dependency>
  <groupId>io.github.openfeign</groupId>
  <artifactId>feign-okhttp</artifactId>
</dependency>
```

```yml
feign:
  okhttp:
    enabled: true
```

## 最佳实践

几个不同的微服务模块，可能都要用到一个client，那么本来在每个modle中都要编写同一个client，代码重复率高，统一修改成本高



第一种模式中，每个模块单独有dto什么的，即dto不放在common包中

![截屏2024-06-11 00.26.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-11%2000.26.46.png)

item模块自己维护向外的接口，其他模块pom导入

会使得代码结构变复杂，虽然结构最清晰合理

![截屏2024-06-11 00.29.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-11%2000.29.09.png)

建立公共模块，增加代码耦合度

启动一个springboot，其扫描的是本模块下指定bean的文件夹，不会扫到别的地方引入的bean，所以要指定

![截屏2024-06-11 00.41.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-11%2000.41.11.png)

- 在hm-api中创建公共dto和client
- 在需要使用api的modle中pom导入api依赖
- 在启动类注解中增加包扫描路径

```xml
<dependency>
    <groupId>org.example</groupId>
    <artifactId>hm-api</artifactId>
    <version>1.0.0</version>
</dependency>
```



## 日志

![截屏2024-06-11 00.47.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-11%2000.47.40.png)

![截屏2024-06-11 00.50.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-11%2000.50.30.png)

```java
public class DefaultFeignConfig {
    @Bean
    public Logger.Level feignLoggerLevel(){
        return Logger.Level.BASIC;
    }
}
```

![截屏2024-06-11 00.55.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-11%2000.55.48.png)

# 网关路由

## 定义

**网络的关口**

**负责请求的路由，转发和身份校验**

前端访问网关，无感知微服务变化

后端不需要暴露具体微服务ip和端口

![截屏2024-06-11 17.42.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-11%2017.42.40.png)

![截屏2024-06-11 17.44.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-11%2017.44.09.png)

## 快速入门

![ ](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-11%2017.50.45.png)



```xml
<!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-gateway -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
```

```xml
<!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-loadbalancer -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-loadbalancer</artifactId>
</dependency>
```

```yml
server:
  port: 8080

spring:
  application:
    name: gateway
  cloud:
    nacos:
      server-addr: 116.198.203.111:8848
    gateway:
      routes:
        - id: item-service
          uri: lb://item-service
          predicates:
            - Path=/items/**,/search/**
        - id: user-service
          uri: lb://user-service
          predicates:
            - Path=/addresses/**,/users/**
        - id: cart-service
          uri: lb://cart-service
          predicates:
            - Path=/carts/**
        - id: pay-service
          uri: lb://pay-service
          predicates:
            - Path=/pay-orders/**
        - id: trade-service
          uri: lb://trade-service
          predicates:
            - Path=/orders/**
```

## 路由属性

![截屏2024-06-11 18.24.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-11%2018.24.25.png)

**路由predicates**断言方式

![截屏2024-06-11 18.24.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-11%2018.24.40.png)

![截屏2024-06-11 18.26.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-11%2018.26.19.png)

filter配置在单个route中

如果想要让filter全局有效，配置在gateway下，和route同级 --- default-filters

![截屏2024-06-11 18.32.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-11%2018.32.04.png)

![截屏2024-06-11 18.32.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-11%2018.32.30.png)

## 网关登录校验

### 思路流程

![截屏2024-06-12 01.10.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-12%2001.10.06.png)

![截屏2024-06-12 01.11.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-12%2001.11.50.png)





![截屏2024-06-12 23.48.24](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-12%2023.48.24.png)

### 自定义GlobalFilter

![截屏2024-06-12 15.07.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-12%2015.07.11.png)

![截屏2024-06-12 15.11.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-12%2015.11.12.png)

```java
@Component
public class MyGlobalFilter implements GlobalFilter, Ordered {
    @Override
    public int getOrder() {
        return 0; //过滤器执行顺序，值越小，优先级越高
    }

    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        ServerHttpRequest request = exchange.getRequest();
        System.out.println("pre stage");
        //放行
        return chain.filter(exchange);
    }
}
```

### 自定义GatewayFilter

![截屏2024-06-12 15.22.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-12%2015.22.19.png)

写完filter的配置类后要写入yml配置文件

**无参的情况**

```java
@Component
public class PrintAnyGatewayFilterFactory extends AbstractGatewayFilterFactory<Object> {
    @Override
    public GatewayFilter apply(Object config) {
        return new GatewayFilter() {
            @Override
            public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
                //filter逻辑
                return chain.filter(exchange);
            }
        }; //这种匿名类无法再实现Ordered接口，无法排序  可以构建内部类，new出来，或者换继承对象
//        return new OrderedGatewayFilter(new GatewayFilter() {
//            @Override
//            public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
//                //filter逻辑
//                return chain.filter(exchange);
//            }
//        },1);
    }
}
```

**有参情况**

工厂模式

![截屏2024-06-12 15.45.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-12%2015.45.53.png)

​	**shortcutFieldOrder 中的顺序为变量接受顺序如 把 b 放在第一位 那么 b接受第一个数据 然后为config中的b set值**

**对abc排序就是决定其获取变量的顺序**

### 实现登录校验

```java
@Component
@RequiredArgsConstructor
public class AuthGlobalFilter implements GlobalFilter, Ordered {


    private final AuthProperties authProperties;

    private final JwtTool jwtTool;


    private final AntPathMatcher antPathMatcher = new AntPathMatcher();
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        //获取request
        ServerHttpRequest request = exchange.getRequest();
        //判断是否要做登录拦截(exclude path)
        if (isExclude(request.getPath().toString())){
            //放行
            return chain.filter(exchange);
        }
        //获取token
        String token = null;
        List<String> list = request.getHeaders().get("authorization");
        if (CollUtils.isNotEmpty(list)){
            token = list.get(0);
        }
        Long userId = null;
        //校验并解析token
        try {
            userId = jwtTool.parseToken(token);
        }catch (UnauthorizedException e){
            //拦截 设置状态码
            ServerHttpResponse response = exchange.getResponse();
            response.setStatusCode(HttpStatus.UNAUTHORIZED);
            return response.setComplete();//终止
        }

        //TODO 传递用户信息

        //放行
        return chain.filter(exchange);
    }

    private boolean isExclude(String string) {
        for (String pathPattern : authProperties.getExcludePaths()) {
            if (antPathMatcher.match(pathPattern,string)){//spring的路径匹配器
                return true;
            }
        }
        return false;
    }

    @Override
    public int getOrder() {
        return 0;
    }
}
```

### 网关传递用户

![截屏2024-06-12 22.55.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-12%2022.55.11.png)

**1 - 网关保存用户到请求头**

```java
//TODO 传递用户信息
ServerWebExchange build = exchange.mutate()
        .request(builder -> builder.header("user-info", userInfo))
        .build();
//放行
return chain.filter(build);
```

**2 - 构建拦截器**

```java
public class UserInfoInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //获取登录用户信息
        String userInfo = request.getHeader("user-info");
        //判断是否获取了用户，如有，存入ThreadLocal
        if (StrUtil.isNotBlank(userInfo)){
            UserContext.setUser(Long.valueOf(userInfo));
        }
        //放行
        return true;
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        UserContext.removeUser();
    }
}
```

**3 - 注册拦截器**

```java
@Configuration
@ConditionalOnClass(DispatcherServlet.class)
#由于每个modle都引入了common，包括网关，网关依赖中没有springMvc，无法加载这个bean，所以这个bean需要有条件得加载
#代码含义即为，存在这个类的springBoot加载这个bean（其为业务功能，包含SpringMvc）
public class MvcConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new UserInfoInterceptor());//默认拦截所有路径
    }
}
```

**4 - 修改配置文件**

```factories
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
  com.hmall.common.config.MyBatisConfig,\
  com.hmall.common.config.MvcConfig,\
  com.hmall.common.config.JsonConfig
```

springBoot启动的时候，不会把common中的拦截器加入bean中

需要再common中手动说明需要变成bean的内容![截屏2024-06-12 23.22.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-12%2023.22.09.png)

### OpenFeign传递用户

![截屏2024-06-12 23.38.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-12%2023.38.06.png)

```java
public class DefaultFeignConfig {
    @Bean
    public Logger.Level feignLoggerLevel(){
        return Logger.Level.BASIC;
    }

    @Bean
    public RequestInterceptor userInfoRequestInterceptor(){
        return new RequestInterceptor() {
            @Override
            public void apply(RequestTemplate requestTemplate) {
                Long userId = UserContext.getUser();
                if (userId != null){
                    requestTemplate.header("user-info", userId.toString());
                }
            }
        };
    }
}
```

**为了让springBoot包以外的bean生效**

**指定配置文件**

```java
@MapperScan("com.hmall.trade.mapper")
@EnableFeignClients(basePackages = "com.hmall.api.client",defaultConfiguration = DefaultFeignConfig.class)
@SpringBootApplication
public class TradeApplication {
    public static void main(String[] args) {
        SpringApplication.run(TradeApplication.class, args);
    }


}
```

# 配置管理

**不管是微服务还是网关，都存在大量的配置文件，配置文件重复高，而且大多都是写死的**

**配置管理使得配置共享，并且实现热部署**

![截屏2024-06-13 15.22.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-13%2015.22.00.png)

## 配置共享

**添加共享文件到nacos中 - jdbc,MybatisPlus,日志，Swagger,OpenFeign ...**



有关jdbc的yaml，把要变的内容都放到变量中去 ------   发布配置

```yaml
spring:
  datasource:
    url: jdbc:mysql://${hm.db.host}:${hm.db.port:3306}/${hm.db.database}?useUnicode=true&characterEncoding=UTF-8&autoReconnect=true&serverTimezone=Asia/Shanghai
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password: ${hm.db.pw}
mybatis-plus:
  configuration:
    default-enum-type-handler: com.baomidou.mybatisplus.core.handlers.MybatisEnumTypeHandler
  global-config:
    db-config:
      update-strategy: not_null
      id-type: auto
```

![截屏2024-06-13 15.42.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-13%2015.42.46.png)

**拉取共享配置**

基于Nacos的共享配置，比application配置先读取，但是Nacos的地址记录在application中，导致矛盾

所以要把Nacos的地址记录在新的bootstrap(引导)中，项目首先会去读引导配置，application避免配置引导文件中的内容

![截屏2024-06-13 15.39.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-13%2015.39.22.png)

加入依赖

```XML
  <!--nacos配置管理-->
  <dependency>
      <groupId>com.alibaba.cloud</groupId>
      <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
  </dependency>
  <!--读取bootstrap文件-->
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-bootstrap</artifactId>
  </dependency>
```

```bootstrap.yaml
spring:
  application:
    name: cart-service # 微服务名称
  profiles:
    active: local
  cloud:
    nacos:
      server-addr: 116.198.203.111:8848
      config:
        file-extension: yaml
        shared-configs:
          - data-id: shared-jdbc.yaml
          - data-id: shared-log.yaml
          - data-id: shared-swagger.yaml
```

## 配置热跟新

**修改配置文件中的配置的时候，微服务无需重启即可使配置生效**

项目启动的时候

会自动运行 条件1中格式的配置文件 （nacos之中）

![截屏2024-06-13 17.26.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-13%2017.26.34.png)



```java
@Data
@Component
@ConfigurationProperties(prefix = "hm.cart")
public class CartProperties {
    private Integer maxItems;
}
```

![截屏2024-06-13 17.36.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-13%2017.36.36.png)

## 动态路由



![截屏2024-06-13 17.47.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-13%2017.47.15.png)

https://nacos.io/zh-cn/docs/sdk.html

![截屏2024-06-13 18.10.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-13%2018.10.05.png)

![截屏2024-06-13 18.13.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-13%2018.13.56.png)

```java
@Slf4j
@Component
public class DynamicRouteLoader {

    @Resource
    private NacosConfigManager nacosConfigManager;


    @Resource
    private RouteDefinitionWriter routeDefinitionWriter;

    private final Set<String> routeIds = new HashSet<>();//记录id的set容器

    private final String dataId = "gateway-route.json";//json格式便于解析
    private final String group = "DEFAULT_GROUP";
    @PostConstruct//在Bean初始化之后就执行
    public void initRouteConfigListener() throws NacosException {
        //项目启动，先拉取一次配置，并且添加配置监听器
        String configInfo = nacosConfigManager.getConfigService().getConfigAndSignListener(dataId, group, 5000,
                new Listener() {
                    @Override
                    public Executor getExecutor() {
                        return null;
                    }

                    @Override
                    public void receiveConfigInfo(String s) {
                        //监听到配置变更，需要跟新路由表
                        updateConfigInfo(s);
                    }
                });

        //第一次读取到配置，也需要跟新到路由表
        updateConfigInfo(configInfo);
    }


    private void updateConfigInfo(String configInfo){
        log.debug("listen new configInfo: {}",configInfo);
        //解析configInfo，转为routeDefinition
        List<RouteDefinition> list = JSONUtil.toList(configInfo, RouteDefinition.class);

        //删除旧的路由表
        //每次新的config监测到跟新，万一是删除了一个route，能被监测到，但仅仅遍历跟新，无法删除
        routeIds.forEach(routeId -> routeDefinitionWriter.delete(Mono.just(routeId)).subscribe());
        routeIds.clear();
        //跟新路由表
        list.forEach(routeDefinition -> {

            //数据要封装到Mono容器
            routeDefinitionWriter.save(Mono.just(routeDefinition)).subscribe();//订阅，响应式编程
            //记录id，方便下一次删除
            routeIds.add(routeDefinition.getId());
        });
    }
}
```

# 服务保护和分布式事务

## 雪崩问题

### 引发

微服务调用链路中的某个服务故障，引起整个链路中的所有微服务都不可用，这就是雪崩



如：  商品服务出现问题，返回数据缓慢，导致购物车服务存在阻塞，请求停滞，知道资源耗尽，无法调用购物车服务

![截屏2024-07-08 13.42.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-08%2013.42.50.png)

- 微服务相互调用，服务提供者出现故障或阻塞
- 服务调用者没有做好异常处理，导致自身故障
- 调用链中的所有服务级联失败，导致整个集群故障

**思路**

- 尽量避免服务出现故障或堵塞
- 保证代码健壮性，网络通畅，能应对较高并发请求

- 服务调用者做好远程调用异常的后备方案，避免故障扩散



### 解决方案 

#### 请求限流 

- 限制访问微服务请求的并发量， 避免服务因流量激增出现故障

![截屏2024-07-08 14.49.14](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-08%2014.49.14.png)

#### 线程隔离 

- 舱壁隔离，模拟船舱隔板的防水原理。通过限定每个业务能使用的线程数量而将故障业务隔离，避免故障扩散

![截屏2024-07-08 14.53.14](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-08%2014.53.14.png)

#### 服务熔断

- 由断路器统计请求的异常比例或慢调用比例，如果超出阈值则会熔断改业务，则拦截该接口的请求

熔断期间，所有请求快速失败，全部走fallback逻辑(后备处理)

![截屏2024-07-08 14.58.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-08%2014.58.50.png)

![截屏2024-07-08 15.01.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-08%2015.01.19.png)

## Sentinel

### 快速入门

![截屏2024-07-08 15.45.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-08%2015.45.51.png)



![截屏2024-07-08 15.51.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-08%2015.51.41.png)

restful 风格的api请求路径一般相同，这回导致簇点资源名称相同，因此要修改配置，把**请求方式+请求路径**作为簇点资源名称

```yml
spring:
  cloud:
    sentinel:
      transport:
        dashboard: localhost:8090
      http-method-specify: true #开启请求方式前缀
```

### 请求限流

 ![截屏2024-07-08 17.14.33](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-08%2017.14.33.png)



QPS - 流量

### 线程隔离

![截屏2024-07-08 17.22.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-08%2017.22.45.png)





### Fallback

调用链路出问题 而 openFeign调用接口 不在 簇点资源中

簇点资源默认只有 SpringMVC的 http接口



**这里的默认规则是针对异常情况(流量上限，资源耗尽后的请求)**   



![截屏2024-07-08 17.49.10](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-08%2017.49.10.png)

**1 - 增加簇点配置规则**

```yml
feign:
  okhttp:
    enabled: true
  sentinel:
    enabled: true
```

**2 - 编写fallback逻辑**

```java
@Slf4j
public class ItemClientFallbackFactory implements FallbackFactory<ItemClient> {
    @Override
    public ItemClient create(Throwable cause) {

        return new ItemClient() {
            @Override
            public List<ItemDTO> queryItemByIds(Collection<Long> ids) {
                log.error("查询商品失败！",cause);

                return CollUtils.emptyList();
            }

            @Override
            public void deductStock(List<OrderDetailDTO> items) {
                log.error("扣减商品失败！",cause);
                throw new RuntimeException(cause);
            }
        };
    }
}
```

**3 - 将目标对象的fallback配置逻辑交给spring管理**

放入配置类中

```java
@Bean
public ItemClientFallbackFactory itemClientFallbackFactory(){
    return new ItemClientFallbackFactory();
}
```

**4 - 修改服务client**

```java
@FeignClient(value = "item-service",
        configuration = DefaultFeignConfig.class,
        fallbackFactory = ItemClientFallbackFactory.class)
//bean配置在DefaultFeignConfig中
public interface ItemClient {
    @GetMapping("/items")
    List<ItemDTO> queryItemByIds(@RequestParam("ids") Collection<Long> ids);

    @PutMapping("/items/stock/deduct")
    void deductStock(@RequestBody List<OrderDetailDTO> items);
}
```

可以做更加准确的流量控制(对于具体的调用链路)

![截屏2024-07-08 18.23.07](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-08%2018.23.07.png)

### 服务熔断

<u>断路器的三个状态</u> 

**一开始处于关闭**

**到达失败阈值后进入开启状态**

**一段时间进入检测状态**

![截屏2024-07-08 18.29.52](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-08%2018.29.52.png)

![截屏2024-07-08 18.32.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-08%2018.32.34.png)

## 分布式事务

![截屏2024-07-08 20.38.21](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-08%2020.38.21.png)

在分布式系统中，如果一个业务需要多个服务合作完成，而且每一个事物都有事物，多个事物必须同时成功或失败，这样的事物就是**分布式事务**。其中每一个服务的事物就是一个**分支事物**，整个业务为**全局事物**

### Seata

![截屏2024-07-08 20.56.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-08%2020.56.43.png)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             



![截屏2024-07-08 21.00.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-08%2021.00.53.png)



### 部署TC服务





```Shell
docker run --name seata \
-p 8099:8099 \
-p 7099:7099 \
-e SEATA_IP=116.198.203.111 \
-v ./seata:/seata-server/resources \
--privileged=true \
--network hm-net \
-d \
seataio/seata-server:1.5.2
```



https://b11et3un53m.feishu.cn/wiki/QfVrw3sZvihmnPkmALYcUHIDnff

**准备seata的配置文件部署在虚拟机的docker上(作为一个springboot服务，选择交给nacos管理)**

**7099是客户端访问端口**

**8099是服务端端口**

### 微服务集成Seata







### XA模式









### AT模式