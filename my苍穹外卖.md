# 1.sky-pojo

**Entity**: 实体，通常和数据库中的表对应

**DTO**：数据传输对象，通常用于程序中各层之间传输数据

**VO**：视图对象，为前端展示数据提供的对象

**POJO**：普通java对象，只有属性和对应的getter和setter	

# 2.Git

**创建Git本地仓库**

**创建Git远程仓库**

**将本地文件推送到Git远程仓库**

​	

# 3.Nginx

**nginx反向代理，将前端发送到动态请求由ngnix转发给后端服务器**

好处：

**提高访问速度，在nginx可以缓存**

**进行负载均衡，把大量请求按照我们指定的方式均匀地分配给集群中的每台服务器**

**保证后端服务安全，前端不能直接请求，防止后端暴露**

# 4.登录问题

**用户的密码在数据库中被明文存储，安全性低 ---> 使用MD5加密方式对密码加密**

只能把一个明文加密，但是不能从密文变成明文

```java
password = DigestUtils.md5DigestAsHex(password.getBytes());//md5加密
if (!password.equals(employee.getPassword())) {
    //密码错误
    throw new PasswordErrorException(MessageConstant.PASSWORD_ERROR);
}
```

# 5.Swagger

## 5.1介绍

使用Swagger你只需要按照它的规范去定义接口以及接口相关的信息，就可以做到生成接口文档，以及在线接口调试页面

knife4j是javaMVC集成Swagger生成Api文档的增强解决方案

## 5.2使用方式

**1 - 导入坐标**

```xml
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>knife4j-spring-boot-starter</artifactId>
    <version>3.0.2</version>
</dependency>
```

**2 - 在配置类中加入knife4j相关配置**

```java
@Bean
public Docket docket() {
    ApiInfo apiInfo = new ApiInfoBuilder()
            .title("苍穹外卖项目接口文档")
            .version("2.0")
            .description("苍穹外卖项目接口文档")
            .build();
    Docket docket = new Docket(DocumentationType.SWAGGER_2)
            .apiInfo(apiInfo)
            .select()
            .apis(RequestHandlerSelectors.basePackage("com.sky.controller"))
            .paths(PathSelectors.any())
            .build();
    return docket;
}
```

**3 - 设置静态资源映射，否则接口文档页面无法访问**

```java
protected void addResourceHandlers(ResourceHandlerRegistry registry) {
    //当头中出现这些路径，到后面的路径资源找
    registry.addResourceHandler("/doc.html").addResourceLocations("classpath:/META-INF/resources/");
    registry.addResourceHandler("/webjars/**").addResourceLocations("classpath:/META-INF/resources/webjars/");
}
```

## 5.3常用注解

| 注解              | 说明                                                   |
| ----------------- | ------------------------------------------------------ |
| @Api              | 用在类上，例如Controller，表明对类的说明               |
| @ApiModel         | 用在类上，例如entity，DTO，VO                          |
| @ApiModelProperty | 用在属性上，描述属性信息                               |
| @ApiOperation     | 用在方法上，例如Controller的方法，说明方法的用途，作用 |

```java
@RestController
@RequestMapping("/admin/employee")
@Slf4j
@Api(tags = "员工相关接口")
public class EmployeeController {

    @Autowired
    private EmployeeService employeeService;
    @Autowired
    private JwtProperties jwtProperties;


    @PostMapping("/login")
    @ApiOperation(value = "员工登录")
    public Result<EmployeeLoginVO> login(@RequestBody EmployeeLoginDTO employeeLoginDTO) {
        log.info("员工登录：{}", employeeLoginDTO);

        Employee employee = employeeService.login(employeeLoginDTO);

        //登录成功后，生成jwt令牌
        Map<String, Object> claims = new HashMap<>();
        claims.put(JwtClaimsConstant.EMP_ID, employee.getId());
        String token = JwtUtil.createJWT(
                jwtProperties.getAdminSecretKey(),
                jwtProperties.getAdminTtl(),
                claims);

        EmployeeLoginVO employeeLoginVO = EmployeeLoginVO.builder()
                .id(employee.getId())
                .userName(employee.getUsername())
                .name(employee.getName())
                .token(token)
                .build();

        return Result.success(employeeLoginVO);
    }

  

}
```

```java
@Data
@ApiModel(description = "员工登录时传递的数据模型")
public class EmployeeLoginDTO implements Serializable {

    @ApiModelProperty("用户名")
    private String username;

    @ApiModelProperty("密码")
    private String password;

}
```

# 6.ThreadLocal

**ThreadLocal并不是一个Thread，而是Thread的局部变量**

**ThreadLocal为每个线程提供单独一份存储空间，具有线程隔离的效果，只有在线程内才能获取到对应的值，线程外不能访问**



*service controller 拦截器 一次请求在同一个线程中*

常用方法：

```java
public void set(T value)//设置局部变量的值
public T get() //返回局部变量的值
public void remove() //移除局部变量
```

# 7.时间datatime格式调整

1 - 加注解



要一一加，太麻烦

```java
@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss")
private LocalDateTime createTime;

@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss")
private LocalDateTime updateTime;
```

2 - 配置文件

统一配置 

**在WebConfiguration中拓展SpringMVC的消息转换器，统一对日期类型进行格式化处理**

```java
//TODO 消息扩展器的依赖一直出现问题：原装的jackson好像版本被挤掉了

//    拓展springMVC框架的消息转换器，统一后端格式转换
//    @Override
//    protected void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
//        log.info("扩展消息转换器");
//        //创建一个消息转换器对象
//        MappingJackson2CborHttpMessageConverter converter = new MappingJackson2CborHttpMessageConverter();
//        //需要创建一个对象转换器，将java对象序列化为json数据
//        converter.setObjectMapper(new JacksonObjectMapper());
//        //将自己的消息转换器加入容器，排在第一位
//        converters.add(0,converter);
//        super.extendMessageConverters(converters);
//    }
```

# 8.公共字段自动填充

**1 - 自定义注解AutoFill 用于标识需要进行自动填充的方法**

**2 - 自定义切面类AutoFillAspect统一拦截加入AutoFill注解的方法，通过反射为公共字段赋值**

**3 - 使用注解**

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface AutoFill {
    OperationType value();
}
```

```java
@Aspect
@Component
@Slf4j
public class AutoFillAspect {
    //切入点
    @Pointcut("execution(* com.sky.mapper.*.*(..)) && @annotation(com.sky.annotation.AutoFill)")
    public void autoFillPointCut(){

    }

    @Before("autoFillPointCut()")
    public void autoFill(JoinPoint joinPoint){
        log.info("开始进行公共字段的自动填充: {}",joinPoint);

        //获取方法签名
        MethodSignature signature = (MethodSignature) joinPoint.getSignature();
        //获取方法上的注解对象
        AutoFill autoFill = signature.getMethod().getAnnotation(AutoFill.class);
        //获取数据库操作类型
        OperationType operationType = autoFill.value();
        //获取当前拦截方法的参数,约定第一个为实体对象
        Object[] args = joinPoint.getArgs();
        if (args == null || args.length == 0){
            return;
        }
        Object entity = args[0];

        //准备需要赋值的数据
        LocalDateTime now = LocalDateTime.now();
        Long currentId = BaseContext.getCurrentId();

        if (operationType == OperationType.INSERT){
            try {
                Method setCreateTime = entity.getClass().getDeclaredMethod(AutoFillConstant.SET_CREATE_TIME, LocalDateTime.class);
                Method setCreateUser = entity.getClass().getDeclaredMethod(AutoFillConstant.SET_CREATE_USER, Long.class);
                Method setUpdateTime = entity.getClass().getDeclaredMethod(AutoFillConstant.SET_UPDATE_TIME, LocalDateTime.class);
                Method setUpdateUser = entity.getClass().getDeclaredMethod(AutoFillConstant.SET_UPDATE_USER, Long.class);

                setCreateTime.invoke(entity,now);
                setUpdateTime.invoke(entity,now);
                setCreateUser.invoke(entity,currentId);
                setUpdateUser.invoke(entity,currentId);
            } catch (Exception e) {
                throw new RuntimeException(e);
            }


        } else if (operationType == OperationType.UPDATE) {
            try {
                Method setUpdateTime = entity.getClass().getDeclaredMethod(AutoFillConstant.SET_UPDATE_TIME, LocalDateTime.class);
                Method setUpdateUser = entity.getClass().getDeclaredMethod(AutoFillConstant.SET_UPDATE_USER, Long.class);

                setUpdateTime.invoke(entity,now);
                setUpdateUser.invoke(entity,currentId);
            } catch (Exception e) {
                throw new RuntimeException(e);
            }
        }

    }
}
```

# 9.HttpClient

## 9.1介绍

客户端编程的工具包

```xml
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
    <version>4.5.13</version>
</dependency>
```

本项目中导入了阿里云oss的依赖，里面的底层用这个实现 所以不用再导入了

![截屏2024-04-07 20.39.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-07%2020.39.08.png)

## 9.2GET

```java
@Test
void testGet() throws Exception {
    //创建httpclient对象
    CloseableHttpClient httpClient = HttpClients.createDefault();
    //创建请求对象
    HttpGet httpGet = new HttpGet("http://localhost:8080/user/shop/status");
    //发送请求
    CloseableHttpResponse response = httpClient.execute(httpGet);
    int statusCode = response.getStatusLine().getStatusCode();//状态码
    System.out.println(statusCode);
    HttpEntity entity = response.getEntity();
    String body = EntityUtils.toString(entity);//请求体
    System.out.println(body);
    //关闭资源
    response.close();
    httpClient.close();
}
```

![截屏2024-04-07 20.48.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-07%2020.48.11.png)

## 9.3POST

```java
@Test
void testPost() throws Exception {
    CloseableHttpClient httpClient = HttpClients.createDefault();

    HttpPost httpPost = new HttpPost("http://localhost:8080/admin/employee/login");
    JSONObject jsonObject = new JSONObject();
    jsonObject.put("username","admin");
    jsonObject.put("password",137321);
    StringEntity entity = new StringEntity(jsonObject.toString());
    entity.setContentEncoding("utf-8");
    entity.setContentType("application/json");
    httpPost.setEntity(entity);

    CloseableHttpResponse response = httpClient.execute(httpPost);

    int statusCode = response.getStatusLine().getStatusCode();//状态码
    System.out.println(statusCode);
    HttpEntity preBody = response.getEntity();
    String body = EntityUtils.toString(preBody);//请求体
    System.out.println(body);

    response.close();
    httpClient.close();
}
```

# 10.微信登录

![截屏2024-04-07 21.52.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-07%2021.52.51.png)

https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/login.html

```yml
wechat:
  appid: ${sky.wechat.appid}
  secret: ${sky.wechat.secret}
```

```yml
wechat:
  appid: wx6ea2c2bde22d59c6
  secret: f940e19632e65f38229c308a5081451d
```

```yml
sky:
  jwt:
    # 设置jwt签名加密时使用的秘钥
    admin-secret-key: itcast
    # 设置jwt过期时间
    admin-ttl: 7200000
    # 设置前端传递过来的令牌名称
    admin-token-name: token
    user-secret-key: itheima
    user-ttl: 7200000
    user-token-name: authentication
```

前端在使用login（）后获取到临时登录凭证 传到后端 



后端根据凭证 和 另外的参数（这个小程序的相关参数）可以获取到 用户的openid并判断其身份

会话密钥 `session_key` 是对用户数据进行 [加密签名](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/signature.html) 的密钥。为了应用自身的数据安全，开发者服务器**不应该把会话密钥下发到小程序，也不应该对外提供这个密钥**。

```java
public User wxLogin(UserLoginDTO userLoginDTO) {
    //调用微信接口 获取openid
    Map<String, String> map = new HashMap<>();
    map.put("appid",weChatProperties.getAppid());
    map.put("secret",weChatProperties.getSecret());
    map.put("js_code", userLoginDTO.getCode());
    map.put("grant_type","authorization_code");
    String json = HttpClientUtil.doGet(WX_LOGIN, map);

    JSONObject jsonObject = JSON.parseObject(json);
    String openid = jsonObject.getString("openid");
    //判断openid是否为空
    if (openid == null){
        throw new LoginFailedException(MessageConstant.LOGIN_FAILED);
    }
    //判断是不是新用户
    User user = userMapper.getByOpenId(openid);
    //如果是 完成注册
    if (user == null){
        user = User.builder()
                .openid(openid)
                .createTime(LocalDateTime.now())
                .build();
        userMapper.insert(user);
    }
    //返回用户对象
    return user;
}
```

# 11.Spring_Cache

**这是一个框架，实现了基于注解的缓存功能，加一个注解，就能实现缓存的功能**

**其提供一层抽象，底层可以切换不同的缓存实现--->**EHCache  Caffeine  **Redis**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```

## 11.1常用注解

| 注解           | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| @EnableCaching | 开启缓存注解功能，通常加在配置类上                           |
| @Cacheable     | 在方法执行前先查询缓存中是否有数据，如果有数据，则直接返回数据，没有，调用方法并将返回值放入缓存 |
| @CachePut      | 将方法的返回值放到缓存(只放不取)                             |
| @CacheEvict    | 将一条或多条数据从缓存中删除                                 |

## 11.2方法使用

```java
@PostMapping
@CachePut(cacheNames = "userCache",key = "#user.id")//使用Spring cache缓存
//key生成: userCache::(user.id),user为形参
//@CachePut(cacheNames = "userCache",key = "#result.id") result固定 为返回值	
//@CachePut(cacheNames = "userCache",key = "#p0.id")//第一个参数  或者为  a0.id 
//@CachePut(cacheNames = "userCache",key = "#root.args[0].id")也是第一个参数
public User save(@RequestBody User user){
    userMapper.insert(user);
    return user;
}
```

```java
@GetMapping
@Cacheable(cacheNames = "userCache",key = "#id")//不能使用result
public User getById(Long id){
    User user = userMapper.getById(id);
    return user;
}
```

底层是代理技术，在代理对象中没找到 反射调用源方法



```java
@DeleteMapping
@CacheEvict(cacheNames = "userCache",key = "#id")
public void deleteById(Long id){
   userMapper.deleteById(id);
}
```



```java
@DeleteMapping
@CacheEvict(cacheNames = "userCache",allEntries = true)
public void deleteAll(Long id){
   userMapper.deleteAll();
}
```

# 12.微信支付

![image-20240409213637816](https://typora---------image.oss-cn-beijing.aliyuncs.com/image-20240409213637816.jpg)

https://pay.weixin.qq.com/docs/merchant/apis/jsapi-payment/direct-jsons/jsapi-prepay.html

https://pay.weixin.qq.com/docs/merchant/apis/mini-program-payment/mini-transfer-payment.html

获取微信支付平台证书

商户私钥文件

```yml
sky:
  wechat:
    appid: wxcd2e39f677fd30ba
    secret: 84fbfdf5ea288f0c432d829599083637
    mchid : 1561414331
    mchSerialNo: 4B3B3DC35414AD50B1B755BAF8DE9CC7CF407606
    privateKeyFilePath: D:\apiclient_key.pem
    apiV3Key: CZBK51236435wxpay435434323FFDuv3
    weChatPayCertFilePath: D:\wechatpay_166D96F876F45C7D07CE98952A96EC980368ACFC.pem
    notifyUrl: https://www.weixin.qq.com/wxpay/pay.php
    refundNotifyUrl: https://www.weixin.qq.com/wxpay/pay.php 
```

```yml
sky:
  wechat:
    appid: ${sky.wechat.appid}
    secret: ${sky.wechat.secret}
    mchid : ${sky.wechat.mchid}
    mchSerialNo: ${sky.wechat.mchSerialNo}
    privateKeyFilePath: ${sky.wechat.privateKeyFilePath}
    apiV3Key: ${sky.wechat.apiV3Key}
    weChatPayCertFilePath: ${sky.wechat.weChatPayCertFilePath}
    notifyUrl: ${sky.wechat.notifyUrl}
    refundNotifyUrl: ${sky.wechat.refundNotifyUrl}
```

​	当前端调用了下单接口的时候，后端先会返回订单号之类的数据（此时为下单 但是没有付款）

然后再次待用付款接口：

传入 **下单号和支付方式**（暂时只有微信支付）



返回预支付交易标识再经过数据加密

返回给前端![截屏2024-04-10 17.11.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-10%2017.11.00.png)

前端可以根据这些请求（微信服务器） 获取正宗的付款界面

# 13.Spring Task

## 13.1介绍

Spring Task是Spring框架提供的任务调度工具 可以按照约定的时间自动执行某个代码逻辑 

## 13.2cron表达式

cron表达式是一个字符串，通过这个表达式可以定义任务触发的时间

规则：

**分为6或7个域，由空格隔开，每个域代表一个含义**

**每个域的含义分别为：**

**秒，分钟，小时，日，月，周，年（可选）**

![截屏2024-04-10 19.44.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-10%2019.44.28.png)

https://cron.qqe2.com ->在线生成器

## 13.3入门案例

![截屏2024-04-10 19.50.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-10%2019.50.08.png)

![截屏2024-04-10 19.52.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-10%2019.52.56.png)

# 14.WebSocket

**webSocket是基于TCP的一种新的网络协议，实现了浏览器和服务器的全双工通信--浏览器和服务器只需要完成一次握手，两者就可以创建*持久性*的连接，并实现双向数据传输**

Http是短连接 并且是单向的



但是HTTP和WebSocket都是基于TCP的



应用：**视频弹幕，网络聊天，体育实况跟新，股票基金报价实时跟新**

![image-20240411113950438](https://typora---------image.oss-cn-beijing.aliyuncs.com/image-20240411113950438.jpg)

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-websocket</artifactId>
</dependency>
```

# 15.Apache ECharts

前端技术

**一款基于js的数据可视化图表库，提供直观，生动，可交互，可个性化定制的数据可视化图表**

# 16.Apache POI

**Apache POI是一个处理Miscrosoft Office各种文件格式的开源项目，简单来说，可以使用POI在Java程序中对Miscrosoft**

**Office各种文件进行读写操作**



**一般来说，POI都是用于操作Excel文件**



**企业开发的时候，报表一般比较复杂，样式，颜色什么的，直接在poi中操作比较繁琐，所以一般都是准备一个样式文件，进行添加数据**



**导入坐标**

```xml
<!-- poi -->
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi</artifactId>
</dependency>
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi-ooxml</artifactId>
</dependency>
```

```java
package com.sky.test;

import org.apache.poi.xssf.usermodel.XSSFCell;
import org.apache.poi.xssf.usermodel.XSSFRow;
import org.apache.poi.xssf.usermodel.XSSFSheet;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;

import java.io.FileInputStream;
import java.io.FileOutputStream;

/**
 * @author abstractMoonAstronaut
 * {@code @date} 2024/4/14
 * {@code @msg} reserved
 */
public class POITest {


    /**
     * 使用POI操作Excel文件
     */
    public static void write() throws Exception{

        //在内存中创建一个Excel文件
        XSSFWorkbook excel = new XSSFWorkbook();
        //在文件中创建一个sheet页
        XSSFSheet sheet = excel.createSheet("info");
        //在sheet中创建行对象
        XSSFRow row = sheet.createRow(0);//第一行
        //在行上创建单元格
        XSSFCell cell = row.createCell(1);//第二格
        //写入数据
        cell.setCellValue("姓名");

        FileOutputStream fileOutputStream = new FileOutputStream("/Users/sunnyday/Desktop/info.xlsx");

        excel.write(fileOutputStream);

        fileOutputStream.close();
        excel.close();
    }

    /**
     * 通过POI读取excel
     */
    public static void read() throws Exception{
        FileInputStream in = new FileInputStream("/Users/sunnyday/Desktop/info.xlsx");
        //读取已有的excel文件
        XSSFWorkbook excel = new XSSFWorkbook(in);
        //读取excel的sheet页
        //按照名字
        XSSFSheet sheet = excel.getSheet("info");
        //按照索引读
        //XSSFSheet sheet = excel.getSheetAt(0);

        int lastRowNum = sheet.getLastRowNum();//获取最后一个行号（从0开始）
        System.out.println(lastRowNum);
        for (int i = 0;i <= lastRowNum;i++){
            XSSFRow row = sheet.getRow(i);
            String cellValue1 = row.getCell(1).getStringCellValue();
            System.out.println(cellValue1);
        }
        excel.close();
        in.close();
    }



    public static void main(String[] args) {
        try {
            read();
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }
}
```
