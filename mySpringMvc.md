# Spring整合Web环境

## JavaWeb三大组件

![截屏2024-03-24 13.47.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-24%2013.47.05.png)

## Spring整合思路

![截屏2024-03-24 13.52.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-24%2013.52.17.png)

### 解决方案

![截屏2024-03-24 14.00.26](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-24%2014.00.26.png)

```java
@Listener ???
public class ContextLoaderListener implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent servletContextEvent) {
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(SpringConfig.class);
        servletContextEvent.getServletContext().setAttribute("applicationContext",applicationContext);
    }

    @Override
    public void contextDestroyed(ServletContextEvent servletContextEvent) {

    }
}
```

```java
public class ApplicationContextTest2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext servletContext = req.getServletContext();
        ApplicationContext applicationContext = (ApplicationContext) servletContext.getAttribute("applicationContext");
        UserService userService = (UserService) applicationContext.getBean("userService");
        userService.show1();
    }
}
```

`/*没有写复杂的优化过程*/`

## Spring-web

### 导包

```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-web</artifactId>
  <version>4.3.7.RELEASE</version>
</dependency>
```

![截屏2024-03-24 14.33.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-24%2014.33.34.png)

```java
public class ApplicationContextTest2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext servletContext = req.getServletContext();
        ApplicationContext applicationContext = WebApplicationsContextUtils.getWebApplicationContext(servletContext);
        UserService userService = (UserService) applicationContext.getBean("userService");
        userService.show1();
    }
}
```

### 	伪实现

![截屏2024-03-24 14.59.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-24%2014.59.16.png)

![截屏2024-03-24 15.00.13](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-24%2015.00.13.png)

![截屏2024-03-24 15.01.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-24%2015.01.00.png)

### 注解版

![截屏2024-03-24 14.42.13](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-24%2014.42.13.png)

![截屏2024-03-24 14.46.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-24%2014.46.15.png)

![截屏2024-03-24 14.46.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-24%2014.46.25.png)

**指定好路径后，根据源码，不走else**

# SpringMVC

## 1.思想设计架构

![截屏2024-03-24 15.04.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-24%2015.04.56.png)

![截屏2024-03-24 15.06.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-24%2015.06.35.png)

![截屏2024-03-24 15.08.21](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-24%2015.08.21.png)

## 2.概述

![截屏2024-03-24 15.12.38](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-24%2015.12.38.png)

## 3.快速入门

### 3.1导入坐标

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
<!-- Spring依赖 -->
<!-- 1.Spring核心依赖 -->
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-core</artifactId>
  <version>4.3.7.RELEASE</version>
</dependency>

<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-beans</artifactId>
  <version>4.3.7.RELEASE</version>
</dependency>

<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-context</artifactId>
  <version>4.3.7.RELEASE</version>
</dependency>

<!-- 2.Spring dao依赖 -->
<!-- spring-jdbc包括了一些如jdbcTemplate的工具类 -->
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-jdbc</artifactId>
  <version>4.3.7.RELEASE</version>
</dependency>

<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-tx</artifactId>
  <version>4.3.7.RELEASE</version>
</dependency>

<!-- 3.Spring web依赖 -->
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-web</artifactId>
  <version>4.3.7.RELEASE</version>
</dependency>

<dependency>

  <groupId>org.springframework</groupId>
  <artifactId>spring-webmvc</artifactId>
  <version>4.3.7.RELEASE</version>
</dependency>

<!-- 4.Spring test依赖：方便做单元测试和集成测试 -->
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-test</artifactId>
  <version>4.3.7.RELEASE</version>
</dependency>
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>druid</artifactId>
  <version>1.1.23</version>
</dependency>
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>8.0.33</version>
</dependency>
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>3.5.9</version>
</dependency>

<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis-spring</artifactId>
  <version>2.0.5</version>
</dependency>
<dependency>
  <groupId>org.aspectj</groupId>
  <artifactId>aspectjweaver</artifactId>
  <version>1.9.6</version>
</dependency>
<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>javax.servlet-api</artifactId>
  <version>3.1.0</version>
</dependency>
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.3.7</version>
  </dependency>
```

### 3.2配置前端控制器

![截屏2024-03-25 00.11.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2000.11.55.png)

```xml
<servlet>
  <servlet-name>DispatcherServlet</servlet-name>
  <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  <init-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:spring-mvc.xml</param-value>
  </init-param>
  <load-on-startup>2</load-on-startup>
</servlet>
<servlet-mapping>
  <servlet-name>DispatcherServlet</servlet-name>
  <url-pattern>/</url-pattern>
</servlet-mapping>
```

### 3.3编写Controller

```java
@Controller
public class QuickController {
    @RequestMapping("/show")
    public void show(){
        System.out.println("showing! this is a test");
    }
}
```

![截屏2024-03-25 15.16.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2015.16.00.png)

默认要给浏览器返回一个视图才不会发生500的错误

### 3.4.Controller访问容器Bean

**1 . 新建一个xml扫描管理service**

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd
       http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--组件扫描-->
    <context:component-scan base-package="com.itheima.service"/>


</beans>
```

**2  . 加入监听器，启动spring容器**

```xml
<context-param>
  <param-name>contextConfigLocation</param-name>
  <param-value>classpath:applicationContext.xml</param-value>
</context-param>
<listener>
  <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```

### 3.5 关键组件简析

核心控制类一般叫做组件

#### 3.5.1分类

![截屏2024-03-25 15.28.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2015.28.05.png)

**匹配 调用 返回视图**

#### 3.5.2 组件的关系



![截屏2024-03-25 15.29.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2015.29.57.png)

## 4.请求处理

### 4.1请求路径的配置

![截屏2024-03-25 15.45.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2015.45.22.png)

#### 4.1.1 注解属性

**1 -** **路径为默认的value属性**

**2 -** **多个路径：**

```java
@RequestMapping(value = {"/show","/show1"})
```

**3 - method**

指定方法

```java
@RequestMapping(value = {"/show","/show1"},method = RequestMethod.GET)//枚举类
```

**4 - GET/POST**	

用法类似，不加method参数

**5 - 用在类上**

```java
@Controller
@RequestMapping("/quick")
public class QuickController {
    final QuickService quickService;
    @Autowired
    public QuickController(QuickService quickService) {
        this.quickService = quickService;
    }
    @RequestMapping("show")
    public String show(){
        System.out.println(quickService);
        return "/index.jsp";
    }
}
```

*注意 方法上还是要写@RequestMapping*

**此时的访问路径**

`http://localhost:8080/springMVCTest_war/quick/show`

### 4.2请求数据的接收

#### 4.2.1GET

![截屏2024-03-25 16.27.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2016.27.35.png)

##### 4.2.1.1直接获取

```
public class QuickController {
    @GetMapping("show")
    public String show(String username,int age){ //注意名字要和url里面的参数名字一样
        System.out.println(username + "->" +age);// 没收到参数为null
        return "/index.jsp";
    }
}
```

##### **4.2.1.2改名**

```java
@Controller
public class QuickController {
    @RequestMapping(value = "/show",method = RequestMethod.GET)
    public String show(@RequestParam("username")String name, int age){
        System.out.println(name+"- > "+age);
        return "/index.jsp";
    }
}
```

##### 4.2.1.3**接收多个同名参数**

```java
@Controller
public class QuickController {
    @RequestMapping(value = "/show",method = RequestMethod.GET)
    public String show(String[] hobby){ //这里不能是List<String>，因为List是一个接口，spring会尝试把这个东西变成对象封装  一定要这么写 在前面加@RequestParam 注解->@RequestParam List<String> hobby(忽略底层) 大概是申明不创建对象，只封装数据
        for (String temp:hobby){
          System.out.println(temp);
        }
        return "/index.jsp";
    }
}
```

##### **4.2.1.4使用Map**

```java
@Controller
public class QuickController {
    @RequestMapping(value = "/show",method = RequestMethod.GET)
    public String show(@RequestParam Map<String,String> map){
        System.out.println((k,v)->{
          System.out.println(k+"->"+v);
        });
        return "/index.jsp";
    }
}
```

##### 4.2.1.5@Requestparam详解

`@RequestParam(value = "username",required = true,defaultValue = "???")`

**当required设置为true，若没有传这个参数会报错，不会设置为null,可以设置初始值**

**username为引用数据类型，不写会为null，但是int类型数据为基本数据类型，不写会直接报错，除非把int改为Integer**

##### 4.2.1.6封装对象

```java
@Controller
public class QuickController {
    @RequestMapping(value = "/show",method = RequestMethod.GET)
    public String show(Student student){
        System.out.println(student.getUsername()+"- > "+student.getAge());
        return "/index.jsp";
    }
}
```

![截屏2024-03-25 17.07.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2017.07.51.png)

**其中，Address也是一个对象，url中写address.city="zj"&address.area='nb'**

#### 4.2.2POST

##### 4.2.2.1整个请求体

###### 4.2.2.1.1获取

```java
public class QuickController {
    @PostMapping("/show")
    public String show(@RequestBody String body){
       System.out.println(body);
       return "/index.jsp";
    }
}
```



###### **4.2.2.1.2使用jackson将数据转换**

```xml
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-databind</artifactId>
  <version>2.9.0</version>
</dependency>
```

![截屏2024-03-25 19.17.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2019.17.55.png)

###### **4.2.2.1.3配置自动转换**

![截屏2024-03-25 19.21.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2019.21.17.png)

##### 4.2.2.2Restful风格数据

###### 4.2.2.2.1定义

![截屏2024-03-25 19.25.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2019.25.39.png)

###### 4.2.2.2.2风格

![截屏2024-03-25 19.26.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2019.26.46.png)

![截屏2024-03-25 19.28.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2019.28.16.png)

![截屏2024-03-25 19.30.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2019.30.20.png)

###### 4.2.2.2.3举例

```java
@Controller
public class QuickController {
    @GetMapping("/stu/{id}")
    public String getById(@PathVariable("id") int id){
        System.out.println(id);
        return "/index.jsp";
    }
}
```

##### 4.2.2.3上传文件

###### 4.2.2.3.1注意事项

![截屏2024-03-25 19.39.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2019.39.51.png)

###### 4.2.2.3.2配置上传解析器

![截屏2024-03-25 19.45.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2019.45.48.png)

```xml
<dependency>
  <groupId>commons-fileupload</groupId>
  <artifactId>commons-fileupload</artifactId>
  <version>1.4</version>
</dependency>
```

```xml
<!--文件上传解析器-->
<bean id = "multipartResolver"<!--这里源码是按照名字获取的，非类型，所以一定要写名字-->
      class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <property name="defaultEncoding" value="UTF-8"/> <!--文件编码格式，默认是ISO8859-1-->
    <property name="maxUploadSizePerFile" value="1048576"/><!--上传的每个文件限制的大小，单位字节-->
    <property name="maxUploadSize" value="3145728"/><!--上传文件总大小-->
    <property name="maxInMemorySize" value="1048576"/><!--上传文件缓存大小-->
</bean>
```

###### 4.2.2.3.3代码实现

```java
@Controller

public class QuickController {
    @GetMapping("/show")
    public String show(@RequestBody MultipartFile myFile) throws IOException { //默认没有开启文件解析功能，要手动配置
        //将上传文件进行保存
        //1 - 获取输入流
        InputStream inputStream = myFile.getInputStream();
        //2 - 获取输出流
        OutputStream outputStream = new FileOutputStream("dizhi" + myFile.getOriginalFilename());

        IOUtils.copy(inputStream,outputStream);
        inputStream.close();
        outputStream.close();
        return "/index.jsp";
    }
}
```

#### 4.2.3接收请求头

![截屏2024-03-25 20.17.10](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2020.17.10.png)

```java
public class QuickController {
    @GetMapping("/show")
    public String show(@RequestHeader ("Accept-Encoding")String headerValue){ //获取指定头
        System.out.println(headerValue);
        return "/index.jsp";
    }
}
```

```java
@Controller
public class QuickController {//获取所有的header
    @GetMapping("/show")
    public String show(@RequestHeader Map<String,String> map){
        map.forEach((k,v)->System.out.println(k+"->"+v));
        return "/index.jsp";
    }
```

#### 4.2.4获取cookie	

```java
@Controller
public class QuickController {
    @GetMapping("/show")
    public String show(@CookieValue(value = "JSESSIONID",defaultValue = "")String jsessionid){
        //若第一次没有返回默认值
      System.out.println(jsessionid);
        return "/index.jsp";
    }
}
```

#### 4.2.5转发域对象

获得转发Request域中的对象，在进行资源转发的时候，有时候需要将一些参数存储到request域携带给下一个资源

```java
@GetMapping("/request1")
public String request1(HttpServletRequest req){
    req.setAttribute("username","fyc");
    return "/request2";
}
@GetMapping("/request2")
public String request2(@RequestAttribute("username")String username){
    System.out.println(username);
    return "index.jsp";
}
```



### 4.3Javaweb常用对象获取

```java
@GetMapping("/show")
public String show(HttpServletRequest req, HttpServletResponse resp){
    System.out.println(req);
    System.out.println(resp);//自动填充
    return "/index.jsp";
}
```

### 4.4请求静态资源

![截屏2024-03-25 20.52.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2020.52.40.png)

**javaweb开发中，请求静态资源，在DefaultServlet中，自动会寻找**

**自己写的web.xml覆盖了默认xml**

**失去了寻找静态资源的功能**

#### 4.4.1web.xml形式

在web.xml中

```xml
<servlet-mapping>
  <servlet-name>default</servlet-name>
  <url-pattern>*.html</url-pattern>
</servlet-mapping>
```

```xml
<servlet-mapping>
  <servlet-name>default</servlet-name>
  <url-pattern>/img/*</url-pattern>
</servlet-mapping>
```

#### 4.4.2spring-mvc.xml形式

在spring-mvc中去配置静态资源**映射**，匹配映射路径的请求到指定的位置去匹配资源

```xml
<mvc:resources mapping="/img/*" location="/img/"/>
<mvc:resources mapping="/css/*" location="/css/"/>
```

**在浏览器中匹配到mapping映射路径的地址到时候，去寻找location下对应的资源**

#### 4.4.3配置处理器

注册一个DefaultServletHttpRequestHandler处理器，静态资源的访问都由该处理器去处理

```xml
<mvc:default-servlet-handler/>
```

### 4.5注解驱动

```xml
<mvc:annotation-driven/>标签
```

改标签内部会帮我们注册RequestMappingHandleMapping，注册RequestMappingHandlerAdapter并注入Json消息转换器

![截屏2024-03-25 21.25.07](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2021.25.07.png)

**配置HandlerMapping**

```xml
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>
```

4.4.2 和 4.4.3都会向容器中注入HandlerMapping，但不是我们想要的，所以要手动配置![截屏2024-03-25 21.20.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2021.20.55.png)

![截屏2024-03-25 21.21.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2021.21.36.png)

## 5.响应处理

### 5.1同步数据

**准备好模型数据，在跳转到执行页面进行展示，此方式使用越来越少了，基于历史原因，一些旧项目还在使用**

#### 5.1.1请求资源转发

```java
@Controller
public class QuickController {
    @GetMapping("/show")
    public String show(){
       	System.out.println("show");
        return "forward:/index.jsp";
    }
}
```

#### 5.1.2请求资源重定向

![截屏2024-03-25 21.35.10](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2021.35.10.png)

```java
@Controller
public class QuickController {
    @GetMapping("/show")
    public String show(){
       	System.out.println("show");
        return "redirect:/index.jsp"; //地址会变
    }
}
```

#### 5.1.3响应模型数据

```java
@GetMapping("/map")
public ModelAndView mav(ModelAndView modelAndView){
    Student student = new Student("fyc",18);
    modelAndView.addObject("student",student);
    modelAndView.setViewName("/index.jsp");
    return modelAndView;
}
```

```jsp
<%@ page contentType="text/html;charset = UTF-8" language="java" %>
<html>
<body>
<h2>Hello World!</h2>
<h1>${student.username} +  ${student.age}</h1>
</body>
</html>
```

#### 5.1.4直接写数据给客户端

 

```java
@Controller
public class QuickController {
    @GetMapping("/show")
    @ResponseBody //告诉springMVC返回的字符串不是视图名，是以响应体的方式响应数据
    public String show(){

        return "hello";
    }
}    
```

### 5.2异步数据

**同步方式返回数据，是将数据响应给浏览器进行页面展示的，而异步方式返回数据一般是回写给Ajax引擎的，即谁访问服务器端，服务器端就把数据响应给谁**

**同步方式回写的数据，一般是无特定格式的字符串，而异步方式回写的数据大多是Json格式字符串**

*1 - 直接写*

![截屏2024-03-25 21.56.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2021.56.55.png)

*2 - 使用工具*

![截屏2024-03-25 21.59.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2021.59.17.png)

```java
@Controller
public class QuickController {
    @GetMapping("/show")
    @ResponseBody //告诉springMVC返回的字符串不是视图名，是以响应体的方式响应数据
    public Student show(){
        Student stu = new Student("s",1);
        return stu;
    }
} //记得导入依赖
```

**优化**

**可以把@ResponseBody放在类上，这样子就不用再在每个方法上写了**

**@RestController = @Controller + @Responsebody**

## 6.拦截器

### 6.1Interceptor简介

**SpringMVC的拦截器Interceptor规范，主要是对Controller资源访问时进行拦截操作的技术，当然拦截后可以进行权限控制，功能增强，类似于Javaweb中的Filter，区别如下**

![截屏2024-03-25 22.12.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2022.12.56.png)

![截屏2024-03-25 22.15.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2022.15.29.png)

![截屏2024-03-25 22.19.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2022.19.16.png)

**post 那个 和方法执行对应，after那个和true或者false对应**

### 6.2快速入门

```java
public class MyInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o) throws Exception {
        System.out.println("hello interceptor");
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, ModelAndView modelAndView) throws Exception {
        System.out.println("post interceptor");
    }

    @Override
    public void afterCompletion(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) throws Exception {
        System.out.println("after interceptor ");
    }
}
```

```xml
<mvc:interceptors>
    <mvc:interceptor>
        <mvc:mapping path="/**"/>
        <bean class="com.itheima.interceptor.MyInterceptor"/>
    </mvc:interceptor>
</mvc:interceptors>
```

### 6.3执行顺序

```xml
<mvc:interceptors>
    <mvc:interceptor>
        <mvc:mapping path="/**"/>
        <bean class="com.itheima.interceptor.MyInterceptor"/>
    </mvc:interceptor>
    <mvc:interceptor>
        <mvc:mapping path="/**"/>
        <bean class="com.itheima.interceptor.MyInterceptor2"/>
    </mvc:interceptor>
</mvc:interceptors>
```

**多个interceptor 执行顺序取决于写的先后位置**

![截屏2024-03-25 22.32.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2022.32.16.png)

![截屏2024-03-25 22.33.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-25%2022.33.04.png)

### 6.4执行原理

![截屏2024-03-26 00.16.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-26%2000.16.35.png)

## 7.全注解开发

### 7.1SpringMvc.xml组件转化为注解

![截屏2024-03-26 00.40.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-26%2000.40.47.png)

```java
@Configuration
@PropertySource("classpath:file.properties")
@ComponentScan("com.itheima.controller")
@EnableWebMvc
public class SpringMVCConfig {
  	@Bean
    public CommonsMultipartResolver multipartResolver(
            @Value("${file.defaultEncoding}")String defaultEncoding,
            @Value("${file.maxUploadSizePerFile}")int maxUploadSizePerFile,
            @Value("${file.maxUploadSize}")int maxUploadSize,
            @Value("${file.maxInMemorySize}")int maxInMemorySize
    ){
        CommonsMultipartResolver multipartResolver = new CommonsMultipartResolver();
        multipartResolver.setDefaultEncoding(defaultEncoding);
        multipartResolver.setMaxUploadSizePerFile(maxUploadSizePerFile);
        multipartResolver.setMaxUploadSize(maxUploadSize);
        multipartResolver.setMaxInMemorySize(maxInMemorySize);
        return multipartResolver;
    }
}
```

```properties
file.defaultEncoding = UTF-8
file.maxUploadSizePerFile = 1048576
file.maxUploadSize = 3145728
file.maxInMemorySize = 1048576
```

```java
@Component
public class MyWebMvcConfigurer implements WebMvcConfigurer {
    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer defaultServletHandlerConfigurer) {
        //开启默认的Servlet解析器
        defaultServletHandlerConfigurer.enable();
    }

    @Override
    public void addInterceptors(InterceptorRegistry interceptorRegistry) {
        //添加一个拦截器，并且配置拦截路径，按照顺序执行
        interceptorRegistry.addInterceptor(new MyInterceptor()).addPathPatterns("/**");
    }
}
```

![截屏2024-03-26 00.53.33](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-26%2000.53.33.png)

### 7.2DispatcherServlet加载核心配置类

![截屏2024-03-26 15.18.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-26%2015.18.48.png)

​	

```xml
<!--    <init-param>-->
<!--      <param-name>contextConfigLocation</param-name>-->
<!--      <param-value>classpath:spring-mvc.xml</param-value>-->
<!--    </init-param>-->
    <init-param>
      <param-name>contextClass</param-name>
      <param-value>com.itheima.config.MyAnnotationConfigWebApplicationContext</param-value>
    </init-param>
```

```java
public class MyAnnotationConfigWebApplicationContext extends AnnotationConfigApplicationContext {
    public MyAnnotationConfigWebApplicationContext() {
        super.register(SpringMVCConfig.class);
    }
}
```

### 7.3消除web.xml

![截屏2024-03-26 15.25.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-26%2015.25.58.png)

```java
public class MyAbstractAnnotationConfigDispatcherServletInitializer
extends AbstractAnnotationConfigDispatcherServletInitializer {

    //提供spring容器的核心配置类
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{SpringConfig.class};
    }

    //提供springMVC容器的核心配置类
    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMVCConfig.class};
    }

    //提供前端控制器的映射路径
    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
}
```

```java
@HandlesTypes({}) // 把接口的实现类放入set
public class MyServiceContainerInitializer implements ServletContainerInitializer {
    @Override
    public void onStartup(Set<Class<?>> set, ServletContext servletContext) throws ServletException {
        System.out.println("sss");
    }
}
```

![截屏2024-03-26 15.50.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-26%2015.50.31.png)



## 8.组件原理剖析

### 8.1前端控制器初始化

![截屏2024-03-26 15.57.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-26%2015.57.31.png)

![截屏2024-03-26 15.58.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-26%2015.58.46.png)

![截屏2024-03-26 15.59.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-26%2015.59.15.png)

#### 8.2前端控制器执行主流程![截屏2024-03-26 16.01.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-26%2016.01.12.png)

## 9.异常处理机制

### 9.1异常处理流程

![截屏2024-03-26 16.05.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-26%2016.05.12.png)

![截屏2024-03-26 16.07.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-26%2016.07.09.png)

### 9.2异常处理方式

#### 9.2.1简单异常处理器

**使用SpringMVC内置的异常处理器 - SimpleMappingExceptionResolver**

*在SpringMVCConfig中*

```java
 		@Bean
    public SimpleMappingExceptionResolver simpleMappingExceptionResolver(){
        SimpleMappingExceptionResolver simpleMappingExceptionResolver = new SimpleMappingExceptionResolver();
        //不管什么错误，设置默认的视图名字
        simpleMappingExceptionResolver.setDefaultErrorView("/error.html");
        //根据类型跳转
        Properties properties = new Properties();
        //key:异常对象全限定名 value:跳转的视图
        properties.setProperty("java.lang.RuntimeException","/error1.html");
        properties.setProperty("java.io.FileNotFoundException","/error2.html");
        simpleMappingExceptionResolver.setExceptionMappings(properties);
        return simpleMappingExceptionResolver;
    }
```

#### 9.2.2自定义异常处理器

**实现HandlerExceptionResolver接口，自定义异常处理**

```java
@Component
public class MyHandlerExceptionResolver implements HandlerExceptionResolver {
    @Override
    public ModelAndView resolveException(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) {
        Student student = new Student("name",18);
        try {
            httpServletResponse.getWriter().println(student);
        } catch (IOException ex) {
            throw new RuntimeException(ex);
        }

        return null;
    }
}
```

```java
@ComponentScan({"com.itheima.controller","com.itheima.ex"})
```

**注意不要忘记增加扫描的地址**

#### 9.2.3注解方式

**使用@ControllerAdvice + @ExceptionHandler来处理**

```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(RuntimeException.class)
    public ModelAndView runtimeExceptionResolverMethod(RuntimeException runtimeException){
        System.out.println(runtimeException);//这里可以对异常做进一步判断或者其他操作
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.setViewName("/error.html");
        return modelAndView;
    }
    @ExceptionHandler(IOException.class)
    @ResponseBody // 转成json
    public Student runtimeExceptionResolverMethod(IOException ioException){
        System.out.println(ioException);//这里可以对异常做进一步判断或者其他操作

        Student student = new Student("f",18);
        return student;
    }
}
```



### 9.3异常处理原理

🧄了，牢弟

。。。。。！！！。。。。。

### 9.4SpringMVC常用的异常解析器

![截屏2024-03-26 17.06.52](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-26%2017.06.52.png)

![截屏2024-03-26 17.07.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-26%2017.07.25.png)