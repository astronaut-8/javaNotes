# 		单元测试

## 概念

针对最小的功能单元（方法），编写测试代码对其进行正确性测试

**使用Juint单元测试框架**

![截屏2024-03-15 16.39.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-15%2016.39.40.png)

## 步骤

![截屏2024-03-15 16.40.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-15%2016.40.39.png)



![截屏2024-03-15 16.44.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-15%2016.44.55.png)

![截屏2024-03-15 16.49.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-15%2016.49.40.png)

## 常用注解

![截屏2024-03-15 16.51.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-15%2016.51.43.png)

# 反射

## 概念

加载类，并允许以编程的方式解剖类中的各种成分（成员变量，方法，构造器）

![截屏2024-03-15 18.08.10](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-15%2018.08.10.png)



## 加载类

**加载类，获取类的字节码:Class对象**

![截屏2024-03-15 18.09.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-15%2018.09.45.png)

```java
//1 -  直接获取
Class class = 类名.class
//2 - 调用Class提供方法
public static Class forName (String package); //package 为全类名
//3 - Object提供的方法	
public Class getClass(); Class class = 对象.getClass();
```

## 获取

### 构造器

![截屏2024-03-17 14.20.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-17%2014.20.01.png)

```java
Class class = cat.class;
Constructor[] constructors = class.getConstructors();
constructor.getName(); //获取构造器名称
constructor.getParameterCount(); //获取构造器参数个数
```

```java
Class class = cat.class;
Constructor constructor1 = class.getConstructor(); // 获取单个构造器，获取到的是无参的
Constructor constructor2 = class.getDeclaredConstructor(String.class,int.class);
//获取第一个参数为String，第二个参数int的构造器
```

### 初始化对象

**调用构造器来完成对象的初始化**

![截屏2024-03-17 14.34.10](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-17%2014.34.10.png)

```java
Constructor constructor = class.getDeclaredConstructor(String.class,int.class);
Cat cat = (Cat)constructor.newInstance(); // 初始化对象
//------
Constructor<Cat> constructor = class.getDeclaredConstructor(String.class,int.class);
Cat cat = constructor.newInstance(); // 初始化对象,为了普适性，一半不会在获取构造器的时候增加泛型
```



**反射会破坏类的封装性，当设置setAccessible后，原本private的构造方法也可以执行了**

```java
Constructor constructor = class.getDeclaredConstructor(String.class,int.class);
//禁止检查访问权限
constructor.setAccessible(true);//若constructor对应的构造方法为私有的，也可以正常构造
Cat cat = (Cat)constructor.newInstance(); // 初始化对象
```

```java
Constructor constructor = class.getDeclaredConstructor(String.class,int.class);
Cat cat = (Cat)constructor.newInstance("name",18); // 有参构造方法初始化对象
```

### 成员变量

#### 获取

![截屏2024-03-17 14.44.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-17%2014.44.00.png)

```java
Class class = Cat.class;
Field[] fields = class.getDeclaredFields();
for (Firld field : fields){
  System.out.println(field.getName() + field.getType()); //获取成员变量的名字和类型
}
```

```java
Class class = Cat.class;
Field field = class.getDeclaredField("Name"); //获取具体某个
```

#### 操作

![截屏2024-03-17 16.13.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-17%2016.13.18.png)

```java
Class class = Cat.class;
Field field = class.getDeclaredField("Name"); //获取具体某个
//----------------
Cat cat = new Cat();
field.setAccessible(true);
field.set(cat,"sdsd"); //把cat对象中的name换成sdsd
//----------------
String name = (String) field.get(cat);//获取cat中的Name

```

### 成员方法

![截屏2024-03-17 16.20.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-17%2016.20.00.png)



#### 获取

```java
Class class = Cat.class;
Method[] methods = class.getDeclaredMethods(); //获取类的全部成员方法
method.getName(); //获取方法名字
method.getParametersCount(); // 获取方法参数个数
method.getReturnType();//获取方法返回值类型
```



```java
Class class = Cat.class;
Method run = class.getDeclaredMethod("run")//获取没有参数的名字为run的方法
Method eat = class.getDeclaredMethod("eat",String.class)//获取有一个参数的名字为eat的方法

```

#### 执行

![截屏2024-03-17 16.27.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-17%2016.27.46.png)

```java
Cat cat = new Cat();
run.setAccessible(true);
eat.setAccessible(true);
Object object = run.invoke(cat)//调用cat对象的没有参数的run方法
  
String string = (String)eat.invoke(cat,"fish");
```

# 注解

## 概念

annotation

**就是java代码里面的特殊标记**

**其作用为让其他程序根据注解信息来决定怎么执行程序**

## 自定义

![截屏2024-03-17 16.52.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-17%2016.52.41.png)

```java
public @interface Mytest1{
	String aaa();
  boolean bbb() default true;
  String[] ccc();
}

@myTest1(aaa = "ss",ccc = {"111"})
public class AnnotationTest{
  @myTest1(aaa = "sd",bbb = false,ccc = "sssd")
  public void test(){
    
  }
}
// 注解定义中有默认值的变量不一定要写，没有默认值的字段一定要赋值
```

```java
public @interface Mytest2{
	String value();//特殊属性，在写的时候不用加名字
}
//若有多个字段，但是除了vaule外的都有默认值，那还是可以省略的
```

## 原理

![截屏2024-03-17 17.02.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-17%2017.02.16.png)

![截屏2024-03-17 17.03.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-17%2017.03.36.png)

## 元注解

修饰注解的注解

![截屏2024-03-17 17.06.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-17%2017.06.05.png)

### @Target

![截屏2024-03-17 17.06.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-17%2017.06.48.png)

```java
@Target(ElementType.Type) // @Target({Elementtype.Method,ElementType.Type})
public @interface Mytest3{

}
```

### @Retention

![截屏2024-03-17 17.10.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-17%2017.10.22.png)

## 注解的解析

判断类，方法，成员变量上是否存在注解，并把注解上的内容给解析出来

![截屏2024-03-17 17.13.52](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-17%2017.13.52.png)

```java
Class class = Demo.class;
if (class.isAnnotationPresent(MyTest4.class)){
  MyTest4 myTest4 = (MyTest4)class.getDeclaredAnnotation(Mytest4.class);
    System.out.println{
    myTest4.vaule();
    myTest4.aaa();
    Arrays.toString(myTest4.bbb());
    }
} //方法也是可以的  替换class的位置
```

# 动态代理

```java
public static Object newProxyInstance(ClassLoader loader, //指定一个类的加载器
                                          Class<?>[] interfaces, //指定生成的代理长什么样子，也就是有哪些方法
                                          InvocationHandler h//用来指定生成的代理要做什么事情)
```

```java
package url_sta.proxyDemo;
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
public class ProxyUtil {
    public static Star createProxy(BigStar bigStar){

        Star starProxy = (Star) Proxy.newProxyInstance(ProxyUtil.class.getClassLoader(), new Class[]{Star.class},
                new InvocationHandler() {
                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        //代理对象要做的事情                //Object[] args: 这是一个对象数组，包含了被调用方法的参数。
                        if (method.getName().equals("sing")){
                            System.out.println(proxy);
                            return method.invoke(bigStar,args);
                        }else{
                            return method.invoke(bigStar,args);
                        }
                    } //这里还可以用lambda表达式写法
                });
        return starProxy;
    }
}
```

![截屏2024-03-17 20.10.38](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-17%2020.10.38.png)







**动态代理分为JDK动态代理和CGLIB动态代理**



jdk动态代理是Java标准库中的代理机制，依赖于接口来实现创建代理对象

**依赖接口** - 目标代理类必须实现某个接口

```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

interface MyInterface {
    void doSomething();
}

class MyClass implements MyInterface { // 代理函数
    @Override
    public void doSomething() {
        System.out.println("Doing something");
    }
}

class MyInvocationHandler implements InvocationHandler { // 标准接口
    private final Object target;

    public MyInvocationHandler(Object target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("Before method");
        Object result = method.invoke(target, args);
        System.out.println("After method");
        return result;
    }
}

public class Main {
    public static void main(String[] args) {
        MyClass myClass = new MyClass();
        MyInterface proxyInstance = (MyInterface) Proxy.newProxyInstance(
            myClass.getClass().getClassLoader(),
            myClass.getClass().getInterfaces(),
            new MyInvocationHandler(myClass)
        ); // 代理接口实现增强
        proxyInstance.doSomething();
    }
}

```



CGLIB动态代理

通过继承的方式来创建代理对象，不依赖于接口

不依赖接口 - 可以代理没有实现接口的类，通过生成目标类的子类来实现

性能会比jdk的动态代理效率高，但是这不是java标准库中的一部分，需要额外的库支持(Spring中含有)

```java
import org.springframework.cglib.proxy.Enhancer;
import org.springframework.cglib.proxy.MethodInterceptor;
import org.springframework.cglib.proxy.MethodProxy;
import java.lang.reflect.Method;

class MyClass {
    public void doSomething() {
        System.out.println("Doing something");
    }
}

class MyMethodInterceptor implements MethodInterceptor {
    @Override
    public Object intercept(Object obj, Method method, Object[] args, MethodProxy proxy) throws Throwable {
        System.out.println("Before method");
        Object result = proxy.invokeSuper(obj, args);
        System.out.println("After method");
        return result;
    }
}

public class Main {
    public static void main(String[] args) {
        Enhancer enhancer = new Enhancer();
        enhancer.setSuperclass(MyClass.class);
        enhancer.setCallback(new MyMethodInterceptor());
        
        MyClass proxyInstance = (MyClass) enhancer.create();
        proxyInstance.doSomething();
    }
}

```













# Java8新特性

## lambda

**Lambda 允许把函数作为一个方法的参数（函数作为参数传递进方法中）**

**使用 Lambda 表达式可以使代码变的更加简洁紧凑**

### 集合排序

```
Collections.sort(tickets, (a, b) -> a.get(1).compareTo(b.get(1)));
```

代码中的`(a, b) -> a.get(1).compareTo(b.get(1))`是一个Lambda表达式，它定义了一个比较器。这个比较器用于比较`tickets`中的两个元素`a`和`b`。

- `a.get(1)`和`b.get(1)`分别获取`a`和`b`中索引为`1`的元素。
- `compareTo`方法用于比较这两个元素。如果`a.get(1)`小于`b.get(1)`，则返回一个负数；如果两者相等，则返回0；如果`a.get(1)`大于`b.get(1)`，则返回一个正数。

因此，`Collections.sort`方法会根据`tickets`中每个元素的索引为`1`的属性值来进行排序。排序后的`tickets`集合将按照这些属性值的升序排列。

### 示例

**这是一个定义操作数字的接口**

```java
public interface work {
    int count(int val1,int val2);
}
```

**原本的操作实现一个加法**



*实现类*

```java
public class add implements work{
    @Override
    public int count(int val1, int val2) {
        return val1+val2;
    }
}
```

*主函数*

```java
public class workImpl {

    
    public static void countNum(work temp,int num1,int num2){
      
       System.out.println(temp.count(num1,num2));
    }
    public static void main(String[] args) {
        add impl = new add(); //add为接口的实例化对象，而作为参数传递
        countNum(impl,1,2);
    }
}
```

*lambda优化*

```java
public class workImpl {


    public static void countNum(work temp,int num1,int num2){

       System.out.println(temp.count(num1,num2));
    }
    public static void main(String[] args) {
        countNum((int a,int b)->{
            return a+b;
        },1,2);
    }
}
```

*或者*

```java
public static void main(String[] args) {
    work temp = (int a,int b)->{return a+b;};
    countNum(temp,1,2);
}
```

*或者函数引用*

*需要注意被调用方法的参数列表和返回值类型需要和函数式接口中保持一致*

```java
public static void main(String[] args) {
    //work temp = Integer::sum;
    countNum(Integer::sum,1,2);
}
```

### 函数式接口

有且只有一个抽象方法，若存在多个，则Lambda表达式会出错

![截屏2023-12-01 20.32.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2023-12-01%2020.32.53.png)

### 变量访问

**lambda可以访问所在外层作用域的变量**

**实例成员变量this.name 静态成员变量直接访问**

**访问本函数内的局部变量，必须是final类型**

### 双冒号

![截屏2024-03-18 17.02.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-18%2017.02.31.png)

![截屏2024-03-18 17.02.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-18%2017.02.48.png)

**calc 是 Calculable 可以接受三个对象：Calculabe对象，Lambda表达式，和方法引用**

## 方法引用

**方法引用通过方法的名字来指向一个方法。**

**方法引用可以使语言的构造更紧凑简洁，减少冗余代码。**

*方法引用使用一对冒号 ::*

```java
import java.util.List;
import java.util.ArrayList;
 
public class Java8Tester {
   public static void main(String args[]){
      List<String> names = new ArrayList();
        
      names.add("Google");
      names.add("Runoob");
      names.add("Taobao");
      names.add("Baidu");
      names.add("Sina");
        
      names.forEach(System.out::println);
   }
}
```



方法引用主要有四种形式：

1. **静态方法引用**：ClassName::staticMethodName
2. **特定对象的实例方法引用**：instance::instanceMethodName
3. **特定类型的任意对象的实例方法引用**：ClassName::instanceMethodName
4. **构造方法引用**：ClassName::new





**方法的引用会根据你这个位置需要什么样子的方法而去调用，自动匹配的**

## 默认方法

**Java 8 新增了接口的默认方法。**

**简单说，默认方法就是接口可以有实现方法，而且不需要实现类去实现其方法。**

**我们只需在方法名前面加个 default 关键字即可实现默认方法。**

**为什么要有这个特性？**

首先，之前的接口是个双刃剑，好处是面向抽象而不是面向具体编程，缺陷是，当需要修改接口时候，需要修改全部实现该接口的类，目前的 java 8 之前的集合框架没有 foreach 方法，通常能想到的解决办法是在JDK里给相关的接口添加新的方法及实现。然而，对于已经发布的版本，是没法在给接口添加新方法的同时不影响已有的实现。所以引进的默认方法。他们的目的是为了解决接口的修改与现有的实现不兼容的问题。



*语法*

```java
public interface Vehicle {
   default void print(){
      System.out.println("我是一辆车!");
   }
}
```

如果有多重接口 

1 - 覆盖重写方法

2 - ![截屏2024-03-18 17.37.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-18%2017.37.45.png)

*静态默认方法*

![截屏2024-03-18 17.38.21](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-18%2017.38.21.png)

## Optional 类

**Optional 类是一个可以为null的容器对象。如果值存在则isPresent()方法会返回true，调用get()方法会返回该对象。**

**Optional 是个容器：它可以保存类型T的值，或者仅仅保存null。Optional提供很多有用的方法，这样我们就不用显式进行空值检测。**

**Optional 类的引入很好的解决空指针异常。**

```java
import java.util.Optional;
 
public class Java8Tester {
   public static void main(String args[]){
   
      Java8Tester java8Tester = new Java8Tester();
      Integer value1 = null;
      Integer value2 = new Integer(10);
        
      // Optional.ofNullable - 允许传递为 null 参数
      Optional<Integer> a = Optional.ofNullable(value1);
        
      // Optional.of - 如果传递的参数是 null，抛出异常 NullPointerException
      Optional<Integer> b = Optional.of(value2);
      System.out.println(java8Tester.sum(a,b));
   }
    
   public Integer sum(Optional<Integer> a, Optional<Integer> b){
    
      // Optional.isPresent - 判断值是否存在
        
      System.out.println("第一个参数值存在: " + a.isPresent());
      System.out.println("第二个参数值存在: " + b.isPresent());
        
      // Optional.orElse - 如果值存在，返回它，否则返回默认值
      Integer value1 = a.orElse(new Integer(0));
        
      //Optional.get - 获取值，值需要存在
      Integer value2 = b.get();
      return value1 + value2;
   }
}
```

```java
Optional<Integer> a = Optional.ofNullable(value1);
Optional<Integer> b = Optional.of(value2);
Integer value1 = a.orElse(new Integer(0));
```

## stream流

### 简介

**可以让你以一种声明的方式处理数据**

**Stream 使用一种类似用 SQL 语句从数据库查询数据的直观方式来提供一种对 Java 集合运算和表达的高阶抽象**

这种风格将要处理的元素集合看作一种流， 流在管道中传输， 并且可以在管道的节点上进行处理， 比如筛选， 排序，聚合等。

元素流在管道中经过中间操作（intermediate operation）的处理，最后由最终操作(terminal operation)得到前面处理的结果。

```java
/*
+--------------------+       +------+   +------+   +---+   +-------+
| stream of elements +-----> |filter+-> |sorted+-> |map+-> |collect|
+--------------------+       +------+   +------+   +---+   +-------+
*/
List<Integer> transactionsIds = 
widgets.stream()
             .filter(b -> b.getColor() == RED)
             .sorted((x,y) -> x.getWeight() - y.getWeight())
             .mapToInt(Widget::getWeight)
             .sum();

```

### stream

一个来自数据源的元素队列并支持聚合操作

- **元素**是特定类型的对象，形成一个队列。 Java中的Stream并**不会存储元素，而是按需计算。**
- **数据源** 流的来源。 可以是**集合，数组，I/O channel， 产生器generator** 等。
- **聚合操作** 类似SQL语句一样的操作， 比如**filter, map, reduce, find, match, sorted**等。

和以前的Collection操作不同， Stream操作还有**两个基础的特征**：

- **Pipelining**: 中间操作**都会返回流对象本身**。 这样多个操作可以串联成一个管道， 如同流式风格（fluent style）。 这样做可以对操作进行优化， 比如**延迟执行(laziness)和短路( short-circuiting)**。
- **内部迭代**： 以前对集合遍历都是通过Iterator或者For-Each的方式, **显式的在集合外部进行迭代**， 这叫做外部迭代。 Stream提供了内部迭代的方式， 通过**访问者模式**(Visitor)实现。

### 生成流

- **stream()** − 为集合创建串行流。
- **parallelStream()** − 为集合创建并行流。

```java
List<String> strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");
List<String> filtered = strings.stream().filter(string -> !string.isEmpty()).collect(Collectors.toList());
```

### forEach

Stream 提供了新的方法 'forEach' 来迭代流中的每个数据。以下代码片段使用 forEach 输出了10个随机数

```java
Random random = new Random();
random.ints().limit(10).forEach(System.out::println);
//randmon.ints() ints方法返回一个无限的伪随机整数流
//limit(10) 告诉这个流只生成前10个整数
```

### map

map 方法用于映射每个元素到对应的结果，以下代码片段使用 map 输出了元素对应的平方数

```java
List<Integer> numbers = Arrays.asList(3, 2, 2, 3, 7, 3, 5);
// 获取对应的平方数
List<Integer> squaresList = numbers.stream().map( i -> i*i).distinct().collect(Collectors.toList());
```

### filter

filter 方法用于通过设置的条件过滤出元素。以下代码片段使用 filter 方法过滤出空字符串

```java
List<String>strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");
// 获取空字符串的数量
long count = strings.stream().filter(string -> string.isEmpty()).count();
```

### limit

limit 方法用于获取指定数量的流。 以下代码片段使用 limit 方法打印出 10 条数据

```java
Random random = new Random();
random.ints().limit(10).forEach(System.out::println);
```

### sorted

sorted 方法用于对流进行排序。以下代码片段使用 sorted 方法对输出的 10 个随机数进行排序

```
Random random = new Random();
random.ints().limit(10).sorted().forEach(System.out::println);
```

### 并行（parallel）程序

parallelStream 是流并行处理程序的代替方法。以下实例我们使用 parallelStream 来输出空字符串的数量

```java
List<String> strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");
// 获取空字符串的数量
long count = strings.parallelStream().filter(string -> string.isEmpty()).count();
```

![截屏2024-03-19 14.18.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-19%2014.18.28.png)

### Collectors

Collectors 类实现了很多归约操作，例如将流转换成集合和聚合元素。Collectors 可用于返回列表或字符串

```java
List<String>strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");
List<String> filtered = strings.stream().filter(string -> !string.isEmpty()).collect(Collectors.toList());
 
System.out.println("筛选列表: " + filtered);
String mergedString = strings.stream().filter(string -> !string.isEmpty()).collect(Collectors.joining(", "));
System.out.println("合并字符串: " + mergedString);
```

### 统计

另外，一些产生统计结果的收集器也非常有用。它们主要用于int、double、long等基本类型上，它们可以用来产生类似如下的统计结果

```java
List<Integer> numbers = Arrays.asList(3, 2, 2, 3, 7, 3, 5);
 
IntSummaryStatistics stats = numbers.stream().mapToInt((x) -> x).summaryStatistics();
 
System.out.println("列表中最大的数 : " + stats.getMax());
System.out.println("列表中最小的数 : " + stats.getMin());
System.out.println("所有数之和 : " + stats.getSum());
System.out.println("平均数 : " + stats.getAverage());
```

