# Spring

![截屏2024-03-06 19.33.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-06%2019.33.30.png)

## IoC思想

**inversion of Control 控制反转，强调的是原来在程序中创建bean的权利转给第三方**

## DI思想

**dependency injection 依赖注入，强调Bean之间的关系，这种关系第三方负责去设置**

## AOP思想

**aspect oriented programming，面向切面编程 功能的横向抽取 主要实现方式为Proxy**

## 框架

![截屏2024-03-06 19.49.10](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-06%2019.49.10.png)

## Spring简介

![截屏2024-03-06 19.59.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-06%2019.59.41.png)

### 框架

![截屏2024-03-06 20.01.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-06%2020.01.43.png)

# BeanFactory

## 开发步骤

![截屏2024-03-12 00.25.14](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2000.25.14.png)

**1 - 导入Spring的jar包 或者Maven坐标**

**2 - 定义UserService接口 以及其UserServicelmpl实现类**

**3 - 创建beans.xml配置文件 将UserServicelmpl的信息配置到改xml中**

**4 - 编写测试代码，创建BeanFactory，加载配置文件，获取UserService实例对象**

### 导入Spring相关依赖

**1）spring核心依赖**
<u>spring-core、spring-beans、spring-context</u>

**2）spring dao依赖（提供JDBCTemplate）**
<u>spring-jdbc、spring-tx</u>

**3）spring web依赖**
<u>spring-web、spring-webmvc</u>

**4）spring test依赖**
<u>spring-test</u>

```xml
 <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
    <!-- Spring依赖 -->
    <!-- 1.Spring核心依赖 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId>
      <version>6.0.16</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-beans</artifactId>
      <version>6.1.3</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>6.1.3</version>
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
    
    <!-- 4.Spring test依赖：方便做单元测试和集成测试 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>6.1.3</version>
    </dependency>
```

### 创建接口和实现类

![截屏2024-03-12 14.51.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2014.51.23.png)

### 创建xml文件

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id = "userService" class = "spring.test.service.impl.UserServiceImpl"></bean>
</beans>
```

### 编写测试类

```java
public class BeanFactoryTest {
    public static void main(String[] args) {
        //创建工厂对象
        DefaultListableBeanFactory beanFactory = new DefaultListableBeanFactory();
        //创建一个读取器(xml文件)
        XmlBeanDefinitionReader reader = new XmlBeanDefinitionReader(beanFactory);
        //读取器绑定工厂  读取配置文件给工厂
        reader.loadBeanDefinitions("beans.xml");
        //根据id获取Bean实例对象
        UserService userService = (UserService) beanFactory.getBean("userService");
    }

}
```

### 三层架构

#### service调用dao

在 xml配置文件中配置 注意dao层一定要实现接口

service实现类中一定要有 dao 接口的set方法

```java
<bean id = "userService" class = "spring.test.service.impl.UserServiceImpl">
    <!-->在bean中寻找userDao ， 配置到UserServiceImpl 中属性为userDao的</!-->
    <property name="userDao" ref = "userDao"></property>
</bean>
<bean id = "userDao" class="spring.test.dao.impl.UserDaoImpl"></bean>
```

```java
public void setUserDao(UserDao userDao){

    System.out.println("set方法被执行了");
}
```

## 继承体系

![截屏2024-03-12 15.53.59](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2015.53.59.png)





### bean定义对象 

 封装每一个bean标签 最后 存入这个map中

![截屏2024-03-12 15.55.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2015.55.43.png)



# ApplicationContext

**ApplicationContext 称为Spring的容器，内部封装了BeanFactory，比BeanFactory功能更加丰富强大，使用ApplicationContext进行开发的时候，xml配置文件的名称习惯性写成applicationContext.xml**

## 示例

```java
// 创建ApplicationContext，加载配置文件，实例化容器
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
//根据BeanName获得容器中的Bean实例
UserService userService1 = (UserService) applicationContext.getBean("userService");
```

## 与BeanFactory的关系

![截屏2024-03-12 15.45.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2015.45.28.png)

![截屏2024-03-12 15.47.32](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2015.47.32.png)

**4 - BeanFactory 延迟加载 ApplicationContext 立即加载**

![截屏2024-03-12 15.52.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2015.52.42.png)

## 继承体系

**在Spring环境下**

![截屏2024-03-12 15.58.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2015.58.22.png)

###  常用

![截屏2024-03-12 16.00.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2016.00.15.png)

### 导入web包后

![截屏2024-03-12 16.04.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2016.04.47.png)

# Spring-xml

## Bean配置

![截屏2024-03-12 16.09.24](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2016.09.24.png)

### id / class

![截屏2024-03-12 16.14.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2016.14.40.png)

**id 是在配置文件中的唯一标识 进入容器当中 id 转化为 beanName进行存储  getBean是获取的beanName**	

### name

![截屏2024-03-12 16.20.52](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2016.20.52.png)

**如果 bean 配置了id，则beanName为 id  name中的字段指向beanName**

**如果bean没有配置id但是配置了name，那么beanName中的第一个字段设置为beanName**

**如果id，name都没配置，那么实例的全限定名作为beanName**

### scope

**作用范围**

![截屏2024-03-12 16.26.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2016.26.56.png)

**在基本的Spring环境中，只存在singleton和prototype两个选择**（spring-context）

**在springMVC环境中存在多个**

![截屏2024-03-12 16.44.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2016.44.16.png)

### lazy-init

延迟加载

![截屏2024-03-12 16.45.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2016.45.23.png)

**对beanFactory无效**

### init / destory methods

#### 普通版本

![截屏2024-03-12 16.48.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2016.48.34.png)

**构造方法比init方法先执行，构造方法执行代表对象创建，后进行init方法**

`applicationContext.close()//关闭容器`

#### 拓展版本

![截屏2024-03-12 16.53.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2016.53.29.png)

**构造方法 ---> 属性设置方法 ---> afterPropertiesSet ---> init**



### 实例化配置

<u>Spring 的实例化方式主要有如下两种：</u>

#### 构造方法实例化

**底层通过构造方法对Bean进行实例化**



**Spring**中的**<bean>**几乎都是无参构造方法



##### *有参构造方法*

![截屏2024-03-12 17.03.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2017.03.35.png)

**// name 为构造方法中指定的参数，value为指定参数的值,若有多个参数，按照这个格式多写几行**

**只要是创建Bean时构造需要的参数都可以用这个来配置**

##### construct 和 property

###### property

**`property`元素用于设置bean的属性，这些属性通常是通过setter方法设置的。当bean的实例已经被创建之后，你通常会使用`property`来注入依赖或配置。**

```xml
<bean id="myBean" class="com.example.MyBean">  
    <property name="someProperty" value="someValue" />  
</bean>
```

###### constructor-arg

**`constructor-arg`元素用于通过bean的构造函数来注入依赖或配置。当bean的实例正在被创建时，你会使用`constructor-arg`。**

```xml
<bean id="myBean" class="com.example.MyBean">  
    <constructor-arg value="someValue" />  
</bean>
```

在上面的例子中，`MyBean`类应该有一个接受一个String参数的构造函数，Spring会调用这个构造函数来创建bean的实例，并传递`someValue`作为参数。

###### 总结

![截屏2024-03-14 10.28.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-14%2010.28.31.png)

#### **工厂方式实例化 **

**底层通过调用自定义的工厂对Bean进行实例化**

##### 静态工厂方式实例化Bean



<u>1 - 创建一个静态工厂</u>

![截屏2024-03-12 17.15.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2017.15.06.png)

<u>2 - 配置xml文件</u>

![截屏2024-03-12 17.15.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2017.15.58.png)

**在return 对象之前，可以进行一些其他业务逻辑操作**



##### 实例工厂方式实例化Bean

**类 非Static**	

![截屏2024-03-12 17.21.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2017.21.29.png)

**第三方有些bean的产生不是通过构造产生的，通过某些对象的方法产生的**

![截屏2024-03-12 17.22.59](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2017.22.59.png)

**构造方法含参数的写法：**

![截屏2024-03-12 19.02.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2019.02.50.png)



##### 实现FactoryBean规范延迟实例化Bean



**首先工厂实现接口**

![截屏2024-03-12 19.10.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2019.10.39.png)



**配置xml文件**

![截屏2024-03-12 19.11.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2019.11.46.png)

**只有当真正去使用这个bean  --->getBean的时候，才把对象存到 fatctoryBeanObjectCache缓存中去，下次再调用的时候，不会再getObject 而是在缓存中去寻找**



**补充**

<u>**是的，一旦创建ApplicationContext对象后，它会根据XML配置文件中的定义，一次性创建所有的Bean对象**。这是ApplicationContext的一个特性，它在容器启动时就会校验XML文件的正确性，并且不管是否使用到某个Bean，都会将其创建并初始化。这样做的好处是可以提前发现配置错误，并确保在需要时Bean已经准备就绪。</u>



### 依赖注入配置

#### 方式

![截屏2024-03-12 19.23.07](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2019.23.07.png)

#### 注入的数据类型

![截屏2024-03-12 19.24.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2019.24.55.png)



##### **List的使用**

![截屏2024-03-12 19.27.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2019.27.02.png)

![截屏2024-03-12 19.29.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2019.29.34.png)

![截屏2024-03-12 19.30.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2019.30.03.png)

##### **set的使用**

![截屏2024-03-12 19.33.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2019.33.29.png)

##### map使用

![截屏2024-03-12 19.35.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2019.35.05.png)

<u>ke*y 代表普通字段，value-ref代表引用字段*</u>

##### **Properties使用**

![截屏2024-03-12 19.45.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2019.45.35.png)

#### 自动配置

**去自动寻找按照规则限定的bean类**



![截屏2024-03-12 19.53.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2019.53.02.png)

![截屏2024-03-12 19.54.59](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2019.54.59.png)

### 其他配置标签

![截屏2024-03-12 20.09.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2020.09.03.png)

#### 自定义标签

![截屏2024-03-12 20.11.33](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2020.11.33.png)

#### 默认标签

![截屏2024-03-12 20.12.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2020.12.29.png)

##### beans

**嵌套在跟标签内，使用profile属性切换开发环境**

![截屏2024-03-12 20.15.07](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2020.15.07.png)

**激活测试环境**

![截屏2024-03-12 20.15.21](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2020.15.21.png)

**指定某一个特定的环境的时候，公共部分 即 默认环境的bean也是会被生效的**

##### import

**导入其他配置文件，可以将一个配置文件根据业务模块进行拆分，拆分后，最终通过import标签导入到一个主配置文件中去，项目加载主配置文件，就会把import导入的文件一并加载了**

![截屏2024-03-12 20.24.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2020.24.41.png)

![截屏2024-03-12 20.26.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2020.26.22.png)

##### alias

![截屏2024-03-12 20.27.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2020.27.50.png)

**在beanFactory中维护了一个名为aliasMap<String,String>集合，存储bean和name之间的映射关系**

![截屏2024-03-12 20.29.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-12%2020.29.34.png)

## get方法

![截屏2024-03-13 11.59.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-13%2011.59.57.png)



<u>第二种情况，按照类型去匹配的时候，如果有多个同类型的bean就会报错</u>

## 配置非定义Bean

考虑：

**被配置的Bean的实例化方法是什么，无参构造还是有参构造，静态工厂方式还是实例工厂方式**

**被配置的Bean是否需要注入必要属性**

### 1 - 导包

![截屏2024-03-13 12.07.59](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-13%2012.07.59.png)

```java
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>druid</artifactId>
  <version>1.1.23</version>
</dependency>
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>5.1.32</version>
</dependency>
```

### 2 - 配置数据源信息

![截屏2024-03-13 12.14.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-13%2012.14.57.png)

```java
<bean id="dataSource" class = "com.alibaba.druid.pool.DruidDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
    <property name="url" value="jdbc:mysql://localhost:3306/database"></property>
    <property name="username" value="root"></property>
    <property name="password" value="137321Aa"></property>
</bean>
```

### 配置MySQL-Connection

![截屏2024-03-13 12.24.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-13%2012.24.04.png)

```java
<bean id = "clazz" class = "java.lang.Class" factory-method = "forName">
	<constructor-arg name = "className" value = "com.mysql.jdbc.Driver"></constructor-arg>
</bean>
<bean id = "connection" class = "java.sql.DriverManager" fatcory-method = "getConnection"
  scope = "prototype">
	<constructor-arg name = "url" value = "jdbc:mysql://localhost:3306/database"></constructor-arg>
	<constructor-arg name = "user" value = "root"></constructor-arg>
	<constructor-arg name = "password" value = "137321Aa"></constructor-arg>
</bean>
```

### 配置日期对象

#### 原始代码

```java
String currentTimeStr = "2023-08-27 07:20:00";
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
Data data = sdf.parse(currentTimeStr);
```

#### 实例工厂

```java
<bean id="sdf" class="java.text.SimpleDateFormat">
    <constructor-arg name = "pattern" value="yyyy-MM-dd HH:mm:ss"/>
</bean>
<bean id = "data" factory-bean="sdf" factory-method="parse">
    <constructor-arg name = "source" value="2023-08-29 9:21:00"/>
</bean>
```

### 配置Mybatis

#### 官方文档

 http://www.mybatis.org/mybatis-3/zh/

#### 导入坐标

```java
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>5.1.32</version>
</dependency>
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>3.5.5</version>
</dependency>
```

#### 原始配置

```java
InputStream in = Resources.getResourceAsStream("mybatis-config.xml");
SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
SqlSessionFactory sqlSessionFactory = builder.build(in);
```

#### mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "https://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/>
        <property name="username" value="root"/>
        <property name="password" value="137321Aa"/>
      </dataSource>
    </environment>
  </environments>
  <mappers>
    <mapper resource="org/mybatis/example/BlogMapper.xml"/>
  </mappers>
</configuration>
```

#### bean

```xml
<!--静态工厂-->
<bean id = "in" class = "org.apache.ibatis.io.Resources" factory-method="getResourceAsStream">
    <constructor-arg name="resource" value="mybatis-config.xml"/>
</bean>
<!--无参数实例化-->
<bean id="builder" class="org.apache.ibatis.session.SqlSessionFactoryBuilder"></bean>
<!--实例工厂方法-->
<bean id="sqlSessionFactory" factory-bean="builder" factory-method="build">
    <constructor-arg name="inputStream" ref="in"/>
</bean>
```

## 基本流程

**Spring容器在进行初始化的时候，会将xml配置的bean信息封装成一个BeanDefinition对象，所有的BeanDefninition对象存储到一个名为beanDefinitionMap的map集合中去（在beanFactory中进行维护），Spring框架在对Map进行遍历，使用反射创建Bean实例对象，创建好的Bean对象存储在一个名为singletonObject的Map集合中去，当调用getBean方法的时候从该map集合中取出Bean实例对象返回**

### beanDefinition

![截屏2024-03-13 17.31.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-13%2017.31.29.png)

![截屏2024-03-13 18.03.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-13%2018.03.40.png)

**singleObjects -- 单例池**

### 图解

![截屏2024-03-13 18.04.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-13%2018.04.17.png)

## spring-后处理器

Spring后处理器是Spring对外开发的**重要拓展点**，允许我们介入到Bean的整个**实例化流程**中来，以达到

<u>动态注册BeanDefinition</u>

<u>动态修改Beandefinition</u>

<u>以及动态修改Bean的作用</u>

![截屏2024-03-13 18.13.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-13%2018.13.15.png)

**注册BeanDefinition：把一个bean封装成对象添加到beanDefinitionMap中去的过程**

BeanFactoryPostProcessor - **执行一次**

BeanPostProcessor **每一个bean对象实例化之后都要进行**

### BeanFactoryPostProcessor

**Bean工厂后处理器**

**这是一个接口，实现该接口的实现类只要交给Spring容器管理（写入xml中去）的话，Spring机会调用接口的方法，用于对BeanDefinition注册和修改**

![截屏2024-03-13 18.23.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-13%2018.23.28.png)

#### 修改

<u>修改beanDefinition对象</u>

![截屏2024-03-13 18.31.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-13%2018.31.15.png)

#### 添加

<u>添加beanDefinition对象，非加入到单例池里面</u>

##### 顶级接口

```java
public class postFactory implements BeanFactoryPostProcessor {
    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory configurableListableBeanFactory) throws BeansException {
        //注册一个beanDefinition对象
        BeanDefinition beanDefinition = new RootBeanDefinition();
        beanDefinition.setBeanClassName("spring.test.service.impl.DogServiceImpl");
        //强制类型转换，使用接口实现类中的方法
        DefaultListableBeanFactory defaultListableBeanFactory = (DefaultListableBeanFactory) configurableListableBeanFactory;
        defaultListableBeanFactory.registerBeanDefinition("dogService",beanDefinition);
    }
}
```

##### 子接口

### ![截屏2024-03-13 19.34.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-13%2019.34.18.png)

```java
public class addTargeted implements BeanDefinitionRegistryPostProcessor {
    @Override
    public void postProcessBeanDefinitionRegistry(BeanDefinitionRegistry beanDefinitionRegistry) throws BeansException {
        BeanDefinition beanDefinition = new RootBeanDefinition();
        beanDefinition.setBeanClassName("spring.test.service.impl.DogServiceImpl");
        beanDefinitionRegistry.registerBeanDefinition("dogService",beanDefinition);
    }

    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory configurableListableBeanFactory) throws BeansException {
        System.out.println("顶级接口的方法");
    }
} 
```

**//先调用子接口的方法，再调用顶级接口的方法**

![截屏2024-03-13 19.44.33](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-13%2019.44.33.png)

#### 自定义扫描注解

<u>流程</u> ： **在类上添加注解，在BeanFactoryPostProcessor中完成包的扫描，添加BeanDefinition完成注解解析**



**创建注解解析**

![截屏2024-03-13 20.06.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-13%2020.06.16.png)



**后工厂处理器**

![截屏2024-03-13 20.03.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-13%2020.03.18.png)

### BeanPostProcessor

**bean后处理器**

![截屏2024-03-13 20.07.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-13%2020.07.51.png)



**创建对象之后，存入单例池之前**

  记得加入bean中

```java
public class beanPostFactory implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object o, String s) throws BeansException {
        System.out.println(o);
        if (o instanceof UserDao){
            UserDao userDao = (UserDao) o;
            if (userDao.getDaoName() == null){
                userDao.setDaoName("shabi");
            }
            System.out.println(((UserDao) o).getDaoName());
        }
        return o;
    }
    @Override
    public Object postProcessAfterInitialization(Object o, String s) throws BeansException {
        return o;
    }
}
```

结合之前 init 等的方法

**总体的执行顺序为**

![截屏2024-03-14 09.50.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-14%2009.50.41.png)

![截屏2024-03-14 10.10.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-14%2010.10.01.png)

## SpringBean生命周期

![截屏2024-03-14 10.11.52](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-14%2010.11.52.png)

### 初始化阶段

![截屏2024-03-14 10.16.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-14%2010.16.55.png)

#### 属性填充

##### 概述

**注入信息也会被封装BeanDefinition**

![截屏2024-03-14 10.21.33](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-14%2010.21.33.png)

##### 分类

![截屏2024-03-14 10.31.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-14%2010.31.05.png)



##### 循环引用

###### 举例

多个实体之间相互依赖，形成闭环的情况叫做**循环依赖**，也叫做**循环引用**

![截屏2024-03-14 11.07.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-14%2011.07.29.png)

###### **错误流程**

在两个菱形中，去判断容器中是否存在userDao和userService

是在 singletonObject 中寻找，此时 初始 的那个对象还是一个半成品状态，没有被加入到

singletonObject中去，所以无法被获取到，就死循环了

![截屏2024-03-14 11.09.38](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-14%2011.09.38.png)

###### 三级缓存

Spring提供三级缓存**储存完整的Bean实例和半成品实例**，用于解决循环引用的问题

![截屏2024-03-14 11.15.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-14%2011.15.42.png)







**三级缓存中，把bean包成ObjectFactory，调用getObject()返回 ----- 没有进行初始化的bean，且对象未被别人引用** 只是进行了实例化，在内存中开辟了地址



**二级缓存 -------- 中的对象已经被别人引用了**







![截屏2024-03-14 11.31.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-14%2011.31.08.png)

###### 源码	

![截屏2024-03-14 11.37.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-14%2011.37.27.png)

​	![截屏2024-03-14 11.57.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-14%2011.57.54.png)

https://blog.csdn.net/cssweb_sh/article/details/124727265?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522171038861716800211564091%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=171038861716800211564091&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-1-124727265-null-null.142^v99^pc_search_result_base7&utm_term=java%20bean%20循环引用，三级缓存%20源码解析&spm=1018.2226.3001.4187

####  Aware接口

**aop实例举例**

![截屏2024-03-14 12.05.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-14%2012.05.00.png)

```java
public class UserDaoImpl implements UserDao, ServletContextAware, BeanFactoryAware, BeanNameAware, ApplicationContextAware {
    private String daoName;
    public UserDaoImpl() {
    }
    @Override
    public void setBeanFactory(BeanFactory beanFactory) throws BeansException {
    }
    @Override
    public void setBeanName(String s) {
    }
    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
    }

    @Override
    public void setServletContext(javax.servlet.ServletContext servletContext) {

    }

    public UserDaoImpl(String daoName) {
        this.daoName = daoName;
    }


    public String getDaoName() {
        return daoName;
    }

   
    public void setDaoName(String daoName) {
        this.daoName = daoName;
    }

    public String toString() {
        return "UserDaoImpl{daoName = " + daoName + "}";
    }
}
```

## Spring IoC整体流程

![截屏2024-03-14 12.16.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-14%2012.16.17.png)

## 整合第三方框架

### 两种方案

1 - 不需要自定义名空间，不需要使用Spring的配置文件配置第三方框架本身内容，例如 : Mybatis

2 - 需要引入第三方框架命名空间，需要使用Spring的配置文件配置第三方框架本身内容，例如 : Dubbo 

### 整合MyBatis

![截屏2024-03-18 20.47.10](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-18%2020.47.10.png)

#### 导入坐标

```xml
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
  <groupId>org.springframework</groupId>
  <artifactId>spring-beans</artifactId>
  <version>4.3.7.RELEASE</version>
</dependency>
```

#### 配置bean

```xml
<!--配置数据源信息-->
<bean id="dataSource" class = "com.alibaba.druid.pool.DruidDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3306/db1"/>
    <property name="username" value="root"/>
    <property name="password" value="137321Aa"/>
</bean>
<!-- 将SqlSessionFactory对象存储到spring容器-->
<bean class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref = "dataSource"/>
</bean>
<!--扫描指定的包，产生Mapper对象存储到spring容器-->
    <bean class = "org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="spring.test.mapper"/>
    </bean>

    <bean id="mybatisTestImpl" class="spring.test.service.impl.mybatisTestImpl">
        <property name="selectMapper" ref="selectMapper"/>
    </bean>

```

#### 原理

![截屏2024-03-19 18.47.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-19%2018.47.57.png)

### 整合第三方框架

**例如Dubbo框架在于Spring进行整合的时候，要使用Dubbo提供的命名空间的拓展方式，自定义一些Dubbo的标签**

![截屏2024-03-19 19.00.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-19%2019.00.05.png)

### spring-context

#### 导入

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

```

#### 建立jdbc.properties

```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/db1
jdbc.username=root
jdbc.password=137321Aa
```

#### 加载文件

```xml
<context:property-placeholder location="classpath:jdbc.properties"/>
```

#### 修改

```xml
    <bean id="dataSource" class = "com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
```

# Spring-注解开发

## 简介

![截屏2024-03-19 19.59.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-19%2019.59.18.png)

![截屏2024-03-19 20.02.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-19%2020.02.51.png)

## @Component

### 配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

    <context:component-scan base-package="com.itheima"/>


</beans>
```

### 使用

```java
@Component("userDao")
public class UserDaoImpl implements UserDao {
}
```

**注解只有一个属性且为vaule 可以直接省略前缀 括号内vaule为id**  

**直接在类上写注解不用再写class**

```java
@Component //如果啥都不加，这个bean的默认id为类名，且首字母小写
```

### 衍生注解

![截屏2024-03-20 10.31.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-20%2010.31.45.png)

**不属于任何一层的就用@component注解**



## 其他标签

![截屏2024-03-19 20.43.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-19%2020.43.15.png)

```java
@Component("userDao")
@Scope("singleton")
@Lazy(false)

public class UserDaoImpl implements UserDao {
    public UserDaoImpl() {
        System.out.println("userDao被初始化了");
    }
    @PostConstruct
    public void init(){
        System.out.println("init....");
    }
    @PreDestroy
    public void destroy(){
        System.out.println("destroy....");
    }
}
```

### Bean依赖注入

**xml配置**

```xml
<bean id = "" class = "">
	<property name = "" ,vaule = "">
</bean>
```

### **注解注入**

#### **@Vaule** 

使用在字段或者方法上，用于注入普通字段

**这里举例的是静态的，动态改变可以用 之前的property配置文件**

```java
    @Value("nick")
    private String userName;

//    @Value("mick")
//    public void setUserName(String userName) {
//        this.userName = userName;
//    }
```

**使用注解开发，定义好字段可以不用写set方法**

**若在字段上和set方法上都加了Value注解，以set上的名字生效**



#### **@Autowired** 

 使用在字段或者方法上，用于根据类型(byType)注入引用数据

##### 概述

```java
@Autowired
UserDao userDao;  //写在set方法上也是可以注入的
@Override
public void show() {
    System.out.println(userDao);
    userDao.show();
}
```

根据类型注入的时候，如果同一个类型的Bean有多个，尝试根据名字进行二次匹配，匹配不成功再报错

##### 拓展

这个注解还可以用在普通的方法上，在容器中自动寻找

```java
@Autowired
public void xxx(UserDao userDao){
    System.out.println(userDao);
}
@Autowired
public void yyy(List<UserDao> userDaoList){
    System.out.println(userDaoList);
}
```

#### **@Qualifired** 

 使用在字段或者方法上，结合@Autowired，根据名称注入

**类型一样，一定要指定某个bean来匹配，可以使用**

```java
@Autowired
@Qualifier("userDao2") //结合Autowired使用
UserDao userDao;  //写在set方法上也是可以注入的
@Override
public void show() {
    System.out.println(userDao);
    userDao.show();
}
```



#### **@Resource** 

 使用在字段或者方法上，根据类型或名称进行注入

```java
@Resource(name = "userDao") //不指定名称参数的时候，根据类型注入，指定名称就根据名称注入，这里name不可以省略
UserDao userDao;  //写在set方法上也是可以注入的
@Override
public void show() {
    System.out.println(userDao);
    userDao.show();
}
```

## 非自定义注解开发

### 规则

**非自定义Bean不能像自定义Bean那样子使用@Componen进行管理，非自定义bean要通过工厂的方式进行实例化，使用@Bean标注方法，@Bean的属性为beanName，若不指定则为当前工厂的方法名称**

![截屏2024-03-20 16.46.50](/Users/sunnyday/Desktop/截屏2024-03-20 16.46.50.png)

### 属性注入

```java
public DataSource dataSource(@Value("${jdbc.driver}") String driverClassName,
                            @Qualifier("userService2") UserService userService){
    try {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName(driverClassName);
        return dataSource;
    }catch (Exception e){
        throw new RuntimeException(e);
    }

}
```

1 - 在方法参数注入的时候，一个Qualifier可以单独写不用再写Autowired

2 - 在 参数按类型进行注入的时候，不用再加注解也可以找到![截屏2024-03-20 17.05.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-20%2017.05.18.png)

## 配置类的开发

**定义一个配置类代替原有xml配置文件，*<bean>*标签以外的标签，一般都是在配置类上使用注解完成的**

```java
@Configuration   //标注当前类为一个配置类 -> 替代配置文件 + @Component
//@ComponentScan(basePackages = {"com.itheima"})
@ComponentScan("com.itheima")
@PropertySource("classpath:jdbc.properties")
@Import(otherBean.class) //导入其他配置
public class SpringConfig {
}
```

```java
public class ApplicationContextTest {
    public static void main(String[] args) {
//        ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("ApplicationContext.xml");
//        UserService userService = (UserService) applicationContext.getBean("userService");
//        userService.show();
        ApplicationContext applicationContext = new 			       AnnotationConfigApplicationContext(SpringConfig.class);
        UserService userService = (UserService) applicationContext.getBean("userService");
        userService.show();
    } 
}
```

![截屏2024-03-20 17.32.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-20%2017.32.04.png)

![截屏2024-03-20 21.00.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-20%2021.00.50.png)





## 配置其他注解

### **@Primary注解**

![截屏2024-03-20 17.33.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-20%2017.33.36.png)

### @Profile

![截屏2024-03-20 17.36.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-20%2017.36.31.png)

```java
System.setProperty("spring.profiles.active","test");
```

## 整合mybatis

```java
@Configuration   //标注当前类为一个配置类 -> 替代配置文件 + @Component
//@ComponentScan(basePackages = {"com.itheima"})
@ComponentScan("com.itheima")
@PropertySource("classpath:jdbc.properties")
@MapperScan("com.itheima.mapper")
public class SpringConfig {
    @Bean
    public DataSource dataSource(@Value("${jdbc.driver}") String driverClassName,
                                 @Value("${jdbc.url}")String url,
                                 @Value("${jdbc.username}")String username,
                                 @Value("${jdbc.password}")String password){
        try {
            DruidDataSource dataSource = new DruidDataSource();
            dataSource.setDriverClassName(driverClassName);
            dataSource.setUrl(url);
            dataSource.setUsername(username);
            dataSource.setPassword(password);
            return dataSource;
        }catch (Exception e){
            throw new RuntimeException(e);
        }

    }
    @Bean
    public SqlSessionFactoryBean sqlSessionFactoryBean(DataSource dataSource){
        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(dataSource);
        return sqlSessionFactoryBean;
    }
}

```

# AOP

## 简介

![截屏2024-03-20 21.44.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-20%2021.44.57.png)

![截屏2024-03-20 21.46.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-20%2021.46.56.png)

## 实现

![截屏2024-03-20 21.48.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-20%2021.48.53.png)

把B（增强对象方法）对象横向抽取 为A（目标对象）

```java
@Component
public class MockAopBeanPostProcessor implements BeanPostProcessor, ApplicationContextAware {
    ApplicationContext applicationContext;
    @Override
    public Object postProcessBeforeInitialization(Object o, String s) throws BeansException {
        System.out.println("Before function....");
        return o;
    }

    @Override
    public Object postProcessAfterInitialization(final Object o, String s) throws BeansException {
        if (o.getClass().getPackage().getName().equals("com.itheima.service.impl")){
            Object beanProxy = Proxy.newProxyInstance(
                    o.getClass().getClassLoader(),
                    o.getClass().getInterfaces(),
                    new InvocationHandler() {
                        @Override
                        public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                            MyAdvice myAdvice = applicationContext.getBean(MyAdvice.class);
                            myAdvice.beforeAdvice();
                            //执行目标方法
                            Object result = method.invoke(o, args);
                            myAdvice.afterAdvice();
                            return result;
                        }
                    }
            );
            return beanProxy;
        }
        return o;
    }

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        this.applicationContext = applicationContext;
    }
}
```

## 相关概念

![截屏2024-03-21 17.13.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-21%2017.13.16.png)

## 快速入门

![截屏2024-03-21 17.20.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-21%2017.20.57.png)

![截屏2024-03-21 17.22.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-21%2017.22.23.png)

### 导入坐标

```xml
<dependency>
  <groupId>org.aspectj</groupId>
  <artifactId>aspectjweaver</artifactId>
  <version>1.9.6</version>
</dependency>
```

```xml
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

</beans>
```

### 准备类，配置管理

**把相关类变成bean**

### 配置切点表达式

```xml
<bean id="userServiceImpl" class="com.itheima.service.impl.UserServiceImpl">
</bean>
<bean id="myAdvice" class="com.itheima.advice.MyAdvice">
</bean>
<aop:config>
    <!--配置切点表达式，目的是要指定哪些方法被增强-->
    <aop:pointcut id="myPointCut" expression="execution(void com.itheima.service.impl.UserServiceImpl.show1())"/>
    <!--配置织入，目的是要执行哪些切点与哪些通知进行结合-->
    <aop:aspect ref="myAdvice">
        <aop:before method="beforeAdvice" pointcut-ref="myPointCut"/>
        <aop:after method="afterAdvice" pointcut-ref="myPointCut"/>
    </aop:aspect>
</aop:config>
```

#### 外面

```xml
<aop:config>
    <!--配置切点表达式，目的是要指定哪些方法被增强-->
    <aop:pointcut id="myPointCut" expression="execution(void com.itheima.service.impl.UserServiceImpl.show1())"/>
    <!--配置织入，目的是要执行哪些切点与哪些通知进行结合-->
    <aop:aspect ref="myAdvice">
        <aop:before method="beforeAdvice" pointcut-ref="myPointCut"/>
        <aop:after method="afterAdvice" pointcut-ref="myPointCut"/>
    </aop:aspect>
</aop:config>
```

#### 里面

```xml
<aop:config>
    <!--配置织入，目的是要执行哪些切点与哪些通知进行结合-->
    <aop:aspect ref="myAdvice">
        <aop:before method="beforeAdvice" pointcut="execution(void com.itheima.service.impl.UserServiceImpl.show1())"/>
    </aop:aspect>
</aop:config>
```

**看使用需求 --> 即这个东西要不要多次使用**

#### 配置语法

![截屏2024-03-22 10.27.59](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-22%2010.27.59.png)

##### 第三条

![截屏2024-03-22 10.32.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-22%2010.32.49.png)



![截屏2024-03-22 10.33.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-22%2010.33.06.png)





**代表搜找service及其子包下的UserService**



##### 第四条

![截屏2024-03-22 10.34.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-22%2010.34.57.png)

![截屏2024-03-22 10.35.10](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-22%2010.35.10.png)

**show1函数重载，参数内写两个. 代表任意参数**

##### 汇总举例

![截屏2024-03-22 10.38.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-22%2010.38.58.png)

### 通知类型

![截屏2024-03-22 10.40.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-22%2010.40.16.png)

#### around	

```xml
<aop:aspect ref="myAdvice">
    <aop:around method="around" pointcut-ref="myPointCut"/>
</aop:aspect>
```

```java
public Object around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable { 
  //正在执行的切入点

    System.out.println("before around");
    Object res = proceedingJoinPoint.proceed(); //执行目标方法
    System.out.println("after around");
    return res;
}
```

#### 额外参数

![截屏2024-03-22 10.53.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-22%2010.53.45.png)

```java
public Object around(ProceedingJoinPoint proceedingJoinPoint, JoinPoint joinPoint) throws Throwable { //正在执行的切入点

    System.out.println(joinPoint.getTarget());
    System.out.println("before around");
    Object res = proceedingJoinPoint.proceed(); //执行目标方法
    System.out.println("after around");
    return res;
}
```

![截屏2024-03-22 11.01.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-22%2011.01.05.png)

## 语法类型

![截屏2024-03-22 11.02.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-22%2011.02.06.png)

```java
public class MyAdvice2 implements MethodBeforeAdvice, AfterReturningAdvice {
    @Override
    public void before(Method method, Object[] objects, Object o) throws Throwable {
        System.out.println("before .... ");
    }
    @Override
    public void afterReturning(Object o, Method method, Object[] objects, Object o1) throws Throwable {
        System.out.println("after .... ");
    }
} //单独的before和after	
```

```xml
<bean id="userServiceImpl2" class="com.itheima.service.impl.UserServiceImpl2">
</bean>
<bean id="myAdvice2" class="com.itheima.advice.MyAdvice2">
</bean>
<aop:config>
    <aop:pointcut id="myPointCut2" expression="execution(void com.itheima.service.impl.UserServiceImpl2.show1())"/>
    <aop:advisor advice-ref="myAdvice2" pointcut-ref="myPointCut2"/>
</aop:config>
```



```java
public class MyAdvice3 implements MethodInterceptor {
    @Override
    public Object invoke(MethodInvocation methodInvocation) throws Throwable {
        System.out.println("环绕前");
        //执行目标对象
        Object res = methodInvocation.getMethod().invoke(methodInvocation.getThis(),methodInvocation.getArguments());
        System.out.println("环绕后");
        return res;
    }
} //环绕方法
```

```xml
<aop:config>
    <aop:pointcut id="myPointCut2" expression="execution(void com.itheima.service.impl.UserServiceImpl2.show1())"/>
    <aop:advisor advice-ref="myAdvice3" pointcut-ref="myPointCut2"/>
</aop:config>
```

![截屏2024-03-22 11.23.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-22%2011.23.06.png)

**都是在一个类中，advice指定好了接口类型而aspect一个类中有多个方法可以选择**

## xml原理

![截屏2024-03-22 11.26.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-22%2011.26.17.png)

## 注解方式

### 基本使用

**配置目标，配置通知，配置aop！！！**

目标和通知使用@。。注解加入spring容器

```java
@Component
@Aspect //类上加
public class MyAdvice {

    @Before("execution(void com.itheima.service.impl.UserServiceImpl.show1())")//方法上加
    public void beforeAdvice(){
        System.out.println("PreStrengthening!!!");
    }
    public void afterAdvice(){
        System.out.println("PostStrengthening!!!");
    }
    public Object around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable { //正在执行的切入点

        System.out.println("before around");
        Object res = proceedingJoinPoint.proceed(); //执行目标方法
        System.out.println("after around");
        return res;
    }
}
```

在xml中

```xml
<aop:aspectj-autoproxy/> //开启aop注解扫描,配置aspectj的自动代理
```

```java
@EnableAspectJAutoProxy //全注解模式，核心配置类@标签
```

### 详解

![截屏2024-03-22 11.40.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-22%2011.40.23.png)

![截屏2024-03-22 11.42.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-22%2011.42.09.png)

### 切点表达式抽取

```java
public class MyAdvice {

    @Pointcut("execution(void com.itheima.service.impl.UserServiceImpl.show1())")
    public void myPointCut(){} //抽取

    @Before("MyAdvice.myPointCut()") //引用
    public void beforeAdvice(){
        System.out.println("PreStrengthening!!!");
    }
}
```

# AOP-事物

## 简述

**基于AOP的声明式事物控制**

![截屏2024-03-22 14.23.14](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-22%2014.23.14.png)

**不同的持久层API控制事物的方式不同，Spring提供统一的方式**



**编程式事物控制**

*Spring提供了事物控制的类和方法，使用编码的方式对业务代码进行事物控制，事物控制代码和业务操作代码耦合到了一起，开发中并不使用*

**声明式事物控制**

*Spring将事物控制的代码封装，对外提供xml和注解配置的方式，通过配置的方式完成事物的控制，可以达到事物控制与业务操作代码解耦合，开发中推荐使用*

## 事物编程

![截屏2024-03-22 14.29.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-22%2014.29.40.png)

## xml控制

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd
       http://www.springframework.org/schema/tx
       http://www.springframework.org/schema/tx/spring-tx.xsd
       http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
  
  
  
    <context:property-placeholder location="classpath:jdbc.properties"/>
  
  			<bean id="dataSource" class = "com.alibaba.druid.pool.DruidDataSource">
        	<property name="driverClassName" value="${jdbc.driver}"/>
        	<property name="url" value="${jdbc.url}"/>
        	<property name="username" value="${jdbc.username}"/>
        	<property name="password" value="${jdbc.password}"/>
    		</bean>
				
  			<!--配置平台事物管理器-->
        <bean id="transactionManager" 			      class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
            <property name="dataSource" ref="dataSource"/>
        </bean>
  
         <tx:advice id = "txAdvice" transaction-manager="transactionManager">
            <tx:attributes>
              	<!--配置不同的方法的事物属性
									name - 方法名称 *代表通配符
									addUser,addAccount,addOrders => add*
								-->
                <tx:method name="*"/>
            </tx:attributes>
        </tx:advice>

    	
      <aop:config>
            <aop:pointcut id="txPointCut" expression="execution(void com.itheima.service.impl.UserServiceImpl2.show1())"/>
            <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointCut"/>
      </aop:config>


</beans>
```

**execution中的配置和tx中的配置对比**

![截屏2024-03-22 17.54.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-22%2017.54.25.png)

### tx配置

#### isolation

![截屏2024-03-22 16.46.26](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-22%2016.46.26.png)

#### timeout

**默认-1，单位为s**

#### read-only

**是否只读**

一般查询才设置成只读操作

#### propagation

事物的传播行为，解决业务方法调用业务方法（事物嵌套）![截屏2024-03-22 17.26.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-22%2017.26.56.png)

## 注解控制

 **在要控制的类或者方法上面加注解**

```
@Transactional(isolation = Isolation.DEFAULT)
public void conduct(String sender, String accepter, int val) {
    show();
    send(val,sender);
    accept(val,accepter);
}
```

**在xml中**

```xml
<tx:annotation-driven/> //事物的自动代理，注解驱动
//transaction-manager 默认会找transactionManager 如果不指定的话
```

```java
@Configuration
@ComponentScan("accountManagement")
@PropertySource("classpath:jdbc.properties")
@MapperScan("accountManagement.mapper")
@EnableTransactionManagement
public class springConfig {
    @Bean
    public DataSource dataSource(@Value("${jdbc.driver}") String driverClassName,
                                 @Value("${jdbc.url}")String url,
                                 @Value("${jdbc.username}")String username,
                                 @Value("${jdbc.password}")String password){
        try {
            DruidDataSource dataSource = new DruidDataSource();
            dataSource.setDriverClassName(driverClassName);
            dataSource.setUrl(url);
            dataSource.setUsername(username);
            dataSource.setPassword(password);
            return dataSource;
        }catch (Exception e){
            throw new RuntimeException(e);
        }

    }
    @Bean
    public SqlSessionFactoryBean sqlSessionFactoryBean(DataSource dataSource){
        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(dataSource);
        return sqlSessionFactoryBean;
    }
    @Bean
    public DataSourceTransactionManager transactionManager(DataSource dataSource){
        DataSourceTransactionManager dataSourceTransactionManager = new DataSourceTransactionManager();
        dataSourceTransactionManager.setDataSource(dataSource);
        return dataSourceTransactionManager;
    }
}
```
