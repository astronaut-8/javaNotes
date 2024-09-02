# 1.基础版

**用来简化Spring应用的初始搭建以及开发过程**

## 1.1搭建项目

### 1.1.1idea官网创建

![截屏2024-03-27 17.50.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2017.50.51.png)

![截屏2024-03-27 17.51.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2017.51.17.png)

![截屏2024-03-27 17.52.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2017.52.51.png)

### 1.1.2基于spring官网创建项目

**得到一个压缩文件然后添加进入模块**

![截屏2024-03-27 19.28.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2019.28.40.png)

### 1.1.3基于阿里云创建项目

**1 - 阿里云提供的坐标版本比较低，如果要使用高版本，进入工程后可以手动切换SpringBoot版本**

**2-  阿里云提供的工程模版与Spring官网提供的工程模版略有不同**

![截屏2024-03-27 19.32.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2019.32.45.png)

### 1.1.4手工打造

![截屏2024-03-27 19.40.38](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2019.40.38.png)

![截屏2024-03-27 19.40.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2019.40.00.png)

![	](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2019.40.25.png)

### 1.1.5隐藏文件夹

![截屏2024-03-27 19.43.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2019.43.43.png)

![截屏2024-03-27 19.43.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2019.43.23.png)

## 1.2入门案例解析

### 1.2.1parent

![截屏2024-03-27 19.47.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2019.47.45.png)

**spring-boot-starter-parent继承了spring-boot-dependencies->定义若干属性名字，和依赖管理信息**

**统一由SpringBoot管理坐标版本**

![截屏2024-03-27 19.53.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2019.53.54.png)

### 1.2.2starter

**开发SpringBoot程序需要导入坐标的时候，通常导入对应的starter**

**每一个starter包含了若干个坐标定义的pom管理文件**

**达到了减少依赖配置的目的**  -- 简化配置



<u>区别于parent：</u>

![截屏2024-03-27 20.02.13](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2020.02.13.png)

![截屏2024-03-27 19.59.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2019.59.12.png)

### 1.2.3引导类

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SpringBoot0101Application {

    public static void main(String[] args) {
        SpringApplication.run(SpringBoot0101Application.class, args);
    }

}
```

**引导类是SpringBoot工程的执行入口，运行main方法就可以启动项目**

**Springboot工程运行后初始化Spring容器，扫描引导类所在包加载bean**

###  1.2.4内嵌tomcat 

#### 1.2.4.1依赖关系

**把tomcat运行的执行过程抽取出来，变成一个对象，并交给spring容器去管理**

![截屏2024-03-27 20.37.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2020.37.39.png)

#### 1.2.4.2变更内嵌服务器

**换服务器：排除依赖+引入新依赖**

![截屏2024-03-27 20.36.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2020.36.11.png)

#### 1.2.4.3内置服务器

![截屏2024-03-27 20.38.52](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2020.38.52.png)

## 1.3REST开发

**Representational State Transfer** -> 表现形式状态转换

### 1.3.1与传统的区别

![截屏2024-03-27 20.57.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2020.57.19.png)

### 1.3.2**优点**

**1 - 隐藏资源访问行为，无法通过地址得知对资源是何种操作**

**2 - 书写简化**

### 1.3.3动作和约定

![截屏2024-03-27 20.59.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2020.59.11.png)

![截屏2024-03-27 21.00.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2021.00.17.png)

### 1.3.4路径参数

![截屏2024-03-27 21.04.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2021.04.46.png)

### 1.3.5参数获取区别

![截屏2024-03-27 21.07.24](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2021.07.24.png)

### 1.3.6简化开发

```java
@Controller + @ResponseBody = @RestController
```

## 1.4基础配置

### 1.4.1属性配置

**在resources下面的application.properties修改**

**添加键值对**

```properties
#服务器端口配置
server.port=80


#修改banner（横幅图片）
#spring.main.banner-mode=off
#把图片资源放入resources
#spring.banner.image.location=http:

#日志
#logging.level.root = error
```

![截屏2024-03-27 21.38.26](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2021.38.26.png)

![截屏2024-03-27 21.38.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2021.38.53.png)

### 1.4.2配置文件分类

```yaml
server:
  port: 80
  servlet:
    context-path: /spring0101
```

**优先级**

**properties > yml > yaml**

**不同配置文件中的相同配置按照加载优先级相互覆盖，不同配置文件中不同配置完全保留**



 <u>若没有提示符</u>

![截屏2024-03-27 21.50.07](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2021.50.07.png)

![截屏2024-03-27 21.50.21](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2021.50.21.png)

### 1.4.3yaml文件

#### 1.4.3.1特点

![截屏2024-03-27 21.54.52](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2021.54.52.png)

#### 1.4.3.2语法规则

![截屏2024-03-27 21.56.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2021.56.05.png)

#### 1.4.3.3**数组形式**

![截屏2024-03-27 22.01.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2022.01.48.png)

#### 1.4.3.4字面值表示

![截屏2024-03-27 22.03.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2022.03.28.png)

#### 1.4.3.5变量引用

```yaml
baseDir: c:\windows
tempDir: ${baseDir}\temp
```

```yaml
baseDir: c:\windows
tempDir: "${baseDir}\temp" # 用引号包起来后，里面的\t就会发生转义字符的效果
```

### 1.4.4yaml数据读取

#### 1.4.4.1单个数据

![截屏2024-03-27 22.10.33](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-27%2022.10.33.png)

```java
//读取yaml数据的单一数据
@Value("${country}")
private String country;
```

```java
//读取yaml数据的对象数据
@Value("${user.name}")
private String name;
```

```java
//读取yaml数据的数组的单个数据
@Value("${names[0]}")
private String name;
```

#### 1.4.4.2获取所有数据

```java
@Autowired // 使用自动装配，把所有数据封装到Environment中去
private Environment environment;
@GetMapping("/show")
public String getById(){
    System.out.println(environment.getProperty("")); //引号中是@Value形式装配的格式
    return "SpringBoot is running";
}
```

#### 1.4.4.3获取一个特定类

```yaml
datasource:
  driver: com.mysql.jdbc.Driver
  url: jdbc:mysql://localhost/springboot_db
  username: root
  password: 137321Aa
```

```java
//定义数据模型，封装yaml文件中对应的数据
@Component//加入bean容器
@ConfigurationProperties(prefix = "datasource") //读取类前缀
public class MyDataSource {
    private String driver;
    private String url;
    private String username;
    private String password;
  //省略构造方法
}
```

## 1.5整合第三方技术

### 1.5.1整合JUnit

```java
@SpringBootTest
class SpringBoot0101ApplicationTests {

    @Autowired
    UserDao userDao; //要注入的对象
    @Test
    void contextLoads() {
        userDao.showUser(); //要执行的对象的方法
    }

}
```

![截屏2024-03-28 10.06.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-28%2010.06.09.png)

```java
@SpringBootTest(classes = Application.class) // 当测试类的引导类和他的包位置不匹配，需要加上classes，指定配置类
class SpringBoot0101ApplicationTests {

    @Autowired
    UserDao userDao; //要注入的对象
    @Test
    void contextLoads() {
        userDao.showUser(); //要执行的对象的方法
    }

}
```

**如果测试类在SpringBoot启动类的包或者子包中，可以省略启动类的配置，也就是省略classes的设定**

### 1.5.2整合Mybatis

#### 1.5.2.1流程概述

**1 - 勾选Mybatis技术 --> 导入Mybatis对应的stater**

**2 - 数据库连接相关信息转成配置**

**3 - 数据库SQL映射需要添加@Mapper被容器识别到**

```yml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/db1
    username: root
    password: 137321Aa
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">




<mapper namespace="com.example.dao.AccountDao">
    <select id="getAccountById" resultType="com.example.entity.Account">
        select * from account where id = #{id};
    </select>
</mapper>
```

```java
@Mapper
public interface AccountDao {
    Account getAccountById(@Param("id")Integer id);
}
```

```java
@SpringBootTest
class SpringBoot02MybatisApplicationTests {


    @Autowired
    AccountDao accountDao;
    @Test
    void contextLoads() {
        System.out.println(accountDao.getAccountById(1));
    }

}
```

#### 1.5.2.2解决时区问题

```yml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver  #本来是com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/db1?serverTimezone=UTC
    username: root
    password: 137321Aa
```

#### 1.5.2.3驱动过时

```
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/db1
    username: root
    password: 137321Aa
```

### 1.5.3整合Mybatis-Plus

```
//later
```

### 1.5.4整合Druid

```xml
<!-- https://mvnrepository.com/artifact/com.alibaba/druid-spring-boot-starter -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.2.19</version>
</dependency>
```

 

```yml
#spring:
#  datasource:
#    driver-class-name: com.mysql.cj.jdbc.Driver
#    url: jdbc:mysql://localhost:3306/db1
#    username: root
#    password: 137321Aa
#    type: com.alibaba.druid.pool.DruidDataSource
#


spring:
  datasource:
    druid:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/db1
      username: root
      password: 137321Aa 
```

## 1.6SSM开发

### 1.6.1lombok开发

**一个java类库，提供了一组注解，简化POJO实体类开发**,

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
</dependency>
```

```java
@Data 
@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
```

### 1.6.2业务层开发命名

业务层以功能为主，dao层与数据库名字相关

![截屏2024-03-28 16.43.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-28%2016.43.43.png)

### 1.6.3表现层消息一致性处理

![截屏2024-03-28 17.34.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-28%2017.34.18.png)

###  1.6.4异常抛出

他妈怎么没东西啊

### 1.6.5分页功能

```java
@Configuration
public class MybatisConfig {
    /**
     *分页操作是在MybatisPlus的常规操作基础上得到的，内部是动态的拼写Sql语句
     *因此需要增强对应的功能，使用拦截器实现
     */
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
        MybatisPlusInterceptor mybatisPlusInterceptor = new MybatisPlusInterceptor();
        PaginationInnerInterceptor paginationInnerInterceptor = new PaginationInnerInterceptor(DbType.MYSQL);
        paginationInnerInterceptor.setMaxLimit(1000L);//设置分页上限
        mybatisPlusInterceptor.addInnerInterceptor(paginationInnerInterceptor);
        return mybatisPlusInterceptor;
    }
}
```

```java
@Test
void testGetPage(){
    IPage<Book> page = new Page<Book>(4,2);
    IPage<Book> bookIPage = bookMapper.selectPage(page, null);
    System.out.println(bookIPage.getPages()); //总页数
    System.out.println(bookIPage.getTotal()); //总条数
    System.out.println(bookIPage.getRecords()); //当前分页的具体值
    System.out.println(bookIPage.getCurrent()); //当前是第几页
    System.out.println(bookIPage.getSize()); //当前页应该多少数据
    System.out.println(bookIPage.getRecords().size()); //当前页实际的记录数
}
```







# 2.运维实用篇

## 2.1打包与运行

### 2.1.1windows

#### 2.1.1.1打包运行

![截屏2024-03-28 19.44.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-28%2019.44.22.png)

不在pom.xml 中配置maven打包插件，打出来的jar包会缺少文件和相关配置

![截屏2024-03-28 19.53.07](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-28%2019.53.07.png)

#### 2.1.1.2windows端口占用

![截屏2024-03-28 19.56.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-28%2019.56.22.png)

### 2.1.2Linux

```
Later
```

## 2.2配置高级

### 2.2.1配置临时端口

`java -jar springboot.jar --server.port=8080`

更多配置 在后面加空格 --server....

### 2.2.2临时属性设置

**若不希望用户通过临时参数覆盖程序配置，直接把args关掉，断开读取外部临时配置对应的入口**

![截屏2024-03-28 21.00.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-28%2021.00.16.png)

![截屏2024-03-28 21.01.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-28%2021.01.41.png)

### 2.2.3配置文件4级分类

![截屏2024-03-28 21.08.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-28%2021.08.43.png)

AB文件分别配置，相互覆盖相互没有的配置，相同的配置，高级的文件覆盖低级的文件

**第三级：运维阶段，与jar包同目录下的yml配置文件再做一次覆盖**

**第四级:  最后最高等级为同层目录下config里面的yml文件**

### 2.2.4自定义配置文件

**1 - 修改args参数**

`--spring.config.name=ebank`

`--spring.config.location=d:\ab...`

![截屏2024-03-28 21.14.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-28%2021.14.34.png)

![截屏2024-03-28 21.16.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-28%2021.16.56.png)

**支持写多个，级别最高的是最后一个**

## 2.3多环境开发

### 2.3.1YAML版

```yml
#应用环境
#公共配置

spring:
  profiles:
    active: dev

---
#生产环境
#过时了
spring:
  profiles:pro
server:
  port: 80
---
#开发环境
spring:
  profiles: dev
server:
  port: 81
---
#测试环境
spring:
  config:
    activate:
      on-profile: test
server:
  port: 82
```

变成独立文件

![截屏2024-03-29 14.14.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-29%2014.14.42.png)

### 2.3.2Properties版

做法类似

![截屏2024-03-29 14.16.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-29%2014.16.29.png)



### 2.3.3多环境分组管理

根据功能对配置文件中的信息进行拆分，并制作成独立的配置文件：

`application-devDB.yml`

`application-devRedis.yml`

`application-devMVC.yml`

使用include激活：

```yml
spring:
	profiles:
		active: dev
			include: debDB,devRedis,DevMVC
			#用逗号分隔
```

**后加载配置覆盖前面已有的属性**

**主环境和其他环境有重叠，主环境生效**



**使用分组**

```yml
spring:
	profiles:
		active: dev
			group:
      	"dev": debDB,devRedis,DevMVC
      	"pro": proDB,proRedis,proMVC
			#用逗号分隔
```

### 2.3.4多环境开发控制

```xml
<profiles>
    <profile>
        <id>env_dev</id>
        <properties>
            <profile.active>dev</profile.active>
        </properties>
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
    </profile>
    <profile>
        <id>env_pro</id>
        <properties>
            <profile.active>pro</profile.active>
        </properties>
    </profile>
</profiles>
```

```yml
spring:
	profiles:
		active: @profile.active@
			group:
      	"dev": debDB,devRedis,DevMVC
      	"pro": proDB,proRedis,proMVC
```

**若切换环境没有用，手工使用compile编译**

![截屏2024-03-29 14.39.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-29%2014.39.46.png)

## 2.4日志

### 2.4.1基础

#### 2.4.1.1日志作用

![截屏2024-03-29 14.42.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-29%2014.42.36.png)

#### 2.4.1.2使用

```java
@RestController
@RequestMapping("/books")
public class BookController {
    //创建记录日志的对象
    private static final Logger log = LoggerFactory.getLogger(BookController.class);
    private final BookService bookService;
    @Autowired
    public BookController(BookService bookService) {
        this.bookService = bookService;
    }

    @GetMapping
    public List<Book> getAdd(){
        log.trace("trace...");
        log.debug("debug..."); //在yml中写debug: true 或者在配置中加--debug
        //推荐使用logging:
                //level:
                    //root: debug
        log.info("info...");
        log.warn("warn...");
        log.error("error...");
        return bookService.selectAll();
    }
   
}
```

```yml
logging:
  #设置分组
  group:
    ebank: com.example.controller,com.example.entity
    iService: com.alibaba
  level:
    root: info
    ebank: warn
```

```yml
logging:
  level:
    root: info
    #设置某个包的日志级别
    com.example.controller: debug
```

```java
public class BaseClass { // 让别的类来继承得到日志对象
    public static Logger log;
    public BaseClass() {
        log = LoggerFactory.getLogger(this.getClass());
    }
}
```

```java
@Slf4j //生成日志对象注解（lombok）
@RestController
@RequestMapping("/books")
public class BookController {
    
    private final BookService bookService;
    @Autowired
    public BookController(BookService bookService) {
        this.bookService = bookService;
    }

    @GetMapping
    public List<Book> getAdd(){
        log.trace("trace...");
        log.debug("debug..."); //在yml中写debug: true 或者在配置中加--debug
        //推荐使用logging:
                //level:
                    //root: debug
        log.info("info...");
        log.warn("warn...");
        log.error("error...");
        return bookService.selectAll();
    }
   
}
```

### 2.4.2输出格式控制

![截屏2024-03-29 15.16.42](/Users/sunnyday/Library/Application Support/typora-user-images/截屏2024-03-29 15.16.42.png)

![截屏2024-03-29 15.18.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-29%2015.18.04.png)

```yml
logging:
  level:
    root: info
    #设置某个包的日志级别
    com.example.controller: debug
  pattern:
    console: "%d %5p %n" # 5p为5为的级别信息，可以对齐
    #console: "%d %clr(%5p) %n" 加颜色
    #console: "%d %5p --- [%16t] %40c %n" t为线程名 c类名
    #console: "%d %clr(%5p) --- [%16] %clr(%-40.40c){cyan} : %m %n" 
```

### 2.4.3日志文件

```yml
logging:
  level:
    root: info
    #设置某个包的日志级别
    com.example.controller: debug
  pattern:
    console: "%d %5p %n"
  file:
    name: server.log
  logback:
    rollingpolicy:
      max-file-size: 4KB
      file-name-pattern: server.%d{yyyy-MM-dd}.%i.log #d日期，i编号
```

# 3.开发实用篇

## 3.1热部署

### 3.1.1手动启动热部署

**导入依赖**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
</dependency>
```

**激活热部署**

![截屏2024-03-29 16.58.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-29%2016.58.43.png)

![截屏2024-03-29 16.59.13](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-29%2016.59.13.png)

**热部署仅仅加载当前开发者自定义开发的资源，不加载jar资源**

### 3.1.2自动启动热部署

![截屏2024-03-29 17.03.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-29%2017.03.23.png)

![截屏2024-03-29 17.07.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-29%2017.07.05.png)

`shift + option + command + /`

**idea失去焦点5s后启动热部署**

### 3.1.3热部署范围配置

![截屏2024-03-29 17.11.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-29%2017.11.47.png)

```yml
spring:
  datasource:
    druid:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/bookManagement
      username: root
      password: 137321Aa
  devtools:
    restart:
      exclude: static/**
```



### 3.1.4关闭热部署

```yml
spring:
  datasource:
    druid:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/bookManagement
      username: root
      password: 137321Aa
  devtools:
    restart:
      exclude: static/**
      enabled: false # 这样子不安全，因为可以被更高级别的配置文件覆盖
```

```java
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        System.setProperty("spring.devtools.restart.enabled","false"); //系统的级别一定比任何配置文件高，可以确保生效
        SpringApplication.run(Application.class, args);
    }

}
```

## 3.2配置高级

### 3.2.1@ConfigurationProperties

为第三方bean绑定属性

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.23</version>
</dependency>
```

```yml
datasource:
  driverClassName:com.mysql.jdbc.Driver
```

```java
@Bean
@ConfigurationProperties(prefix = "datasource") //属性会填充进去
public DruidDataSource dataSource(){
    DruidDataSource druidDataSource = new DruidDataSource();
    //druidDataSource.setDriverClassName();
    return druidDataSource;
}
```

![截屏2024-03-29 20.25.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-29%2020.25.57.png)

### 3.2.2宽松绑定/松散绑定

**@ConfigurationProperties绑定属性支持宽松绑定**

**绑定前缀命名规范：仅能使用纯小写字母，数字，下划线作为合法字符**

![截屏2024-03-29 20.32.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-29%2020.32.08.png)

```java
@Value("${server.port}") // 不支持松散绑定
private String port;
```

### 3.2.3常用计量单位绑定

```java
@Component
@Data
@ConfigurationProperties(prefix = "servers")
public class ServerConfig {
    private long timeout;//一半用long作为毫秒的存储单位

    @DurationUnit(ChronoUnit.DAYS)//设置单位
    private Duration serverTimeout;

    @DataSizeUnit(DataUnit.TERABYTES)//设置单位
    private DataSize dataSize;
}
```

### 3.2.4数据校验

**导入依赖**

**接口**

```xml
<dependency>
    <groupId>javax.validation</groupId>
    <artifactId>validation-api</artifactId>
</dependency>
```

**实现类**

```xml
<dependency>
    <groupId>org.hibernate.validator</groupId>
    <artifactId>hibernate-validator</artifactId>
</dependency>
```

```java
@Component
@Data
@ConfigurationProperties(prefix = "servers")
@Validated//开启对当前bean的属性最大校验
public class ServerConfig {
    //设置具体规则
    @Max(value = 8888,message = "最大值不能超过8888")
    private int port;

}
```

![截屏2024-03-29 21.06.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-29%2021.06.23.png)

## 3.3测试

### 3.3.1加载测试专用属性

```yml
test:
  prop: sd
```

```java
@SpringBootTest(properties = {"test.prop = test"}) //变更或者增加测试专用的属性
//@SpringBootTest(args={"--test.prop=test"}) //添加临时命令行参数，优先级更高
class SpringBoot04SsmApplicationTests {

    @Value("${test.prop}")
    private String prop;
    @Test
    void contextLoads() {
        System.out.println(prop);
    }

}
```

### 3.3.2加载测试专用配置

```java
@Configuration
public class MsgConfig { //模拟一个临时加载的bean
    @Bean
    public String msg(){
        return "bean msg";
    }
}
```

```java
@SpringBootTest()
@Import(MsgConfig.class) //多个的话用数组的形式
class SpringBoot04SsmApplicationTests {
    @Autowired
    private String msg;
    @Test
    void contextLoads() {
        System.out.println(msg);
    }

} 
```

### 3.3.3Web环境模拟测试

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.DEFINED_PORT)
public class ControllerTest {

    @Test
    void test(){

    }
}
```

![截屏2024-03-29 21.38.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-29%2021.38.44.png)

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.DEFINED_PORT)
@AutoConfigureMockMvc//开启虚拟MVC调用
public class ControllerTest {
    @Test
    void test(@Autowired MockMvc mockMvc) throws Exception {
        MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
        mockMvc.perform(builder);
    }
}
```

#### 3.3.4.1匹配响应执行状态

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.DEFINED_PORT)
@AutoConfigureMockMvc//开启虚拟MVC调用
public class ControllerTest {
    @Test
    void test(@Autowired MockMvc mockMvc) throws Exception {
        MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
        ResultActions action = mockMvc.perform(builder);

        //设定预期值，与真实值进行比较，成功测试通过，失败测试失败
        //定义预期值
        StatusResultMatchers status = MockMvcResultMatchers.status();
        ResultMatcher ok = status.isOk();
        //添加预计值到本次调用过程中进行匹配
        action.andExpect(ok);
    }
}
```

#### 3.3.4.2匹配响应体

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.DEFINED_PORT)
@AutoConfigureMockMvc//开启虚拟MVC调用
public class ControllerTest {
    @Test
    void test(@Autowired MockMvc mockMvc) throws Exception {
        MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
        ResultActions action = mockMvc.perform(builder);

        //设定预期值，与真实值进行比较，成功测试通过，失败测试失败
        //定义预期值
        ContentResultMatchers content = MockMvcResultMatchers.content();
        ResultMatcher result = content.string("springboot");
        //添加预计值到本次调用过程中进行匹配
        action.andExpect(result);
    }
}
```



```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.DEFINED_PORT)
@AutoConfigureMockMvc//开启虚拟MVC调用
public class ControllerTest {
    @Test
    void test(@Autowired MockMvc mockMvc) throws Exception {
        MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
        ResultActions action = mockMvc.perform(builder);

        //设定预期值，与真实值进行比较，成功测试通过，失败测试失败
        //定义预期值
        ContentResultMatchers content = MockMvcResultMatchers.content();
        ResultMatcher result = content.json("{\"id\":1}");
        //添加预计值到本次调用过程中进行匹配
        action.andExpect(result);
    }
}
```

#### 3.3.4.3匹配响应头

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.DEFINED_PORT)
@AutoConfigureMockMvc//开启虚拟MVC调用
public class ControllerTest {
    @Test
    void test(@Autowired MockMvc mockMvc) throws Exception {
        MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
        ResultActions action = mockMvc.perform(builder);

        //设定预期值，与真实值进行比较，成功测试通过，失败测试失败
        //定义预期值
        HeaderResultMatchers header = MockMvcResultMatchers.header();
        ResultMatcher resultMatcher = header.string("Context-Type", "application/json");
        //添加预计值到本次调用过程中进行匹配
        action.andExpect(resultMatcher);
    }
}
```

### 3.3.4数据层测试回滚

```java
@SpringBootTest()
@Transactional // 开启事物不提交
@Rollback(true)
public class ControllerTest {
    @Test
    void test(@Autowired MockMvc mockMvc) throws Exception {
        //数据库操作
    }
}
```

### 3.3.5测试用例数据设定

```yml
testcase:
  book:
    id: ${random.int}
    id2: ${random.int(10,20)}
    name: ${random.value}
```

![截屏2024-03-31 02.06.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2002.06.27.png)



## 3.4数据层解决方案

### 3.4.1内置数据源

**使用druid的情况**

![截屏2024-03-31 02.14.24](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2002.14.24.png)

**使用druid**

![截屏2024-03-31 02.10.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2002.10.50.png)

**SpringBoot提供了3种内嵌的数据源对象供开发者选择**

`HikariCP,Tomcat提供DataSource,Common DBCP`

![截屏2024-03-31 02.18.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2002.18.56.png)

![截屏2024-03-31 02.20.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2002.20.00.png)

![截屏2024-03-31 02.20.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2002.20.51.png)

### 3.4.2JDBC Template

**内置持久化解决方案**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```

![截屏2024-03-31 14.27.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2014.27.44.png)

![截屏2024-03-31 14.29.07](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2014.29.07.png)

### 3.4.3H2数据库

**SpringBoot提供了3种内嵌数据库供开发者选择提高开发测试效率:**

**H2,HSQL,Derby**

**添加依赖**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
</dependency>
```

**配置参数**

```yml
spring:
  h2:
    console:
      path: /h2
      enabled: true
```

![截屏2024-03-31 14.55.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2014.55.09.png)

![截屏2024-03-31 15.07.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2015.07.45.png)



## 3.5整合第三方技术









## 3.6监控
