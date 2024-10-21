![截屏2024-09-27 09.10.52](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2009.10.52.png)

# 基础篇

## 初识JVM

### 什么是JVM

**jvm本质是运行在计算机上的程序， 用来运行java字节码文件**

![截屏2024-09-27 09.17.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2009.17.09.png)

jvm 将字节码文件变成机器码

### JVM功能

![截屏2024-09-27 09.19.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2009.19.05.png)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2009.19.58.png" alt="截屏2024-09-27 09.19.58" style="zoom:50%;" />

**java实时解释 - 为了支持跨平台特性**





**即时编译 - Just-In-Time**

对热点代码解释后保存在内存 方便下次直接调用

![截屏2024-09-27 09.22.04](/Users/sunnyday/Library/Application Support/typora-user-images/截屏2024-09-27 09.22.04.png)

### 常见的JVM

![截屏2024-09-27 09.24.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2009.24.28.png)

![截屏2024-09-27 09.25.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2009.25.44.png)

![截屏2024-09-27 09.31.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2009.31.06.png)

![截屏2024-09-27 15.17.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2015.17.58.png)

## 字节码文件详解

### Java虚拟机的组成

![截屏2024-09-27 15.21.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2015.21.29.png)

### 字节码文件的组成

#### 正确打开字节码文件

字节码文件中保存源代码编译后的内容，以二进制方法存储，没有用比如utf-8编译成可读的字符串，无法直接用记事本打开阅读



使用**jclasslib**工具查看字节码文件

https://github.com/ingokegel/jclasslib

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2015.35.03.png" alt="截屏2024-09-27 15.35.03" style="zoom: 50%;" />

#### 字节码文件组成

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2015.35.36.png" alt="截屏2024-09-27 15.35.36" style="zoom:50%;" />

##### 基本信息

![截屏2024-09-27 15.46.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2015.46.11.png)

**java字节码文件中，将文件头称为magic魔数**

![截屏2024-09-27 15.38.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2015.38.02.png)

**版本号的作用是判断当前字节码的版本和运行时的JDK是否兼容**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2015.40.30.png" alt="截屏2024-09-27 15.40.30" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2015.45.22.png" alt="截屏2024-09-27 15.45.22" style="zoom:50%;" />

##### 常量池

**避免相同的内容重复定义，节省空间**





String对象和 引用字面量都要存在 由字符串的对象后 加载保留到常量池 出现了字面量 后面别的String对象可以直接指向字面量

也就是说创建一个普通常量字符串 会出现一个对象一个字面量 字面量被这个对象引用 也会被其他对象引用

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2015.55.22.png" alt="截屏2024-09-27 15.55.22" style="zoom: 50%;" />

![截屏2024-09-27 15.58.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2015.58.54.png)

##### 方法

**iconst_0** - 把常量放到操作数栈中

**istore** - 把操作数栈中的数据加入到局部变量表中去(加入完之后 栈中数据就不在了)

**iload** - 把局部变量表中的数据复制一份加入到操作数栈中去，变量表中的数据不会消失



局部变量表中0的位置为mian方法的String[] args

![截屏2024-09-27 16.12.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2016.12.53.png)

**iinc 是直接在局部变量表中去增加**

![截屏2024-09-27 16.16.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2016.16.29.png)

**iinc 1 by 1的位置在iload之前**

![截屏2024-09-27 16.18.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2016.18.27.png)

 

#### 字节码常用工具

##### javap -v

![截屏2024-09-27 16.27.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2016.27.42.png)

![截屏2024-09-27 16.30.07](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2016.30.07.png)

**将输出内容放入到一个文件中去**

![截屏2024-09-27 16.30.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2016.30.44.png)

##### jclasslib插件

插件里搜索jclasslib



- 只能通过切换文件来打开字节码文件，不能直接在右侧字节码窗口中去做操作

- 文件发生变化后需要手动重新编译 不然jclasslib不会自动跟新

![截屏2024-09-27 16.32.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2016.32.29.png)

![截屏2024-09-27 16.41.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2016.41.03.png)

##### arthas

```terminal
cd /Users/sunnyday/Documents/文稿\ -\ Sunny的MacBook\ Air/Java-jar
java -jar arthas-boot.jar
```

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2016.48.40.png" alt="截屏2024-09-27 16.48.40" style="zoom: 33%;" />



<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2016.49.22.png" alt="截屏2024-09-27 16.49.22" style="zoom:33%;" />

###### **cls** 

- 清理窗口

 

###### **dashboard **

**-i -n刷新实时数据间隔和次数**

展示了线程信息 内存信息 和 运行配置信息

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2017.04.28.png" alt="截屏2024-09-27 17.04.28" style="zoom:50%;" />

###### **dump**

dump 类的全限定名：dump已加载类的字节码文件到特定目录

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2017.07.44.png" alt="截屏2024-09-27 17.07.44" style="zoom: 50%;" />

![截屏2024-09-27 17.09.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2017.09.25.png)

###### jad

**jad 类的全限定名：反编译已加载类的源码**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2017.12.14.png" alt="截屏2024-09-27 17.12.14" style="zoom: 50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2017.15.00.png" alt="截屏2024-09-27 17.15.00" style="zoom:50%;" />



### 类的生命周期

**类生命周期描述一个类加载使用卸载的过程**

#### 生命周期概述

![截屏2024-09-27 20.31.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2020.31.56.png)

#### 加载阶段

**全限定名 - 包名 + 类名**

![截屏2024-09-27 20.33.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2020.33.16.png)







<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2020.35.14.png" alt="截屏2024-09-27 20.35.14" style="zoom:50%;" />



**jdk 8 静态字段的数据在堆中**

![截屏2024-09-27 20.36.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2020.36.45.png)

InstanceKlass是c++编写的对象 java代码一般不能直接操作

堆中的 对象是java代码封装的对象 开发者可以访问

Java虚拟机就可以**控制开发者访问数据的范围**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2020.40.50.png" alt="截屏2024-09-27 20.40.50" style="zoom:50%;" />

 ![截屏2024-09-27 20.42.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2020.42.17.png)



**查看所有的java进程**

<img src="/Users/sunnyday/Library/Application Support/typora-user-images/截屏2024-09-27 20.44.55.png" alt="截屏2024-09-27 20.44.55" style="zoom:50%;" />

#### 连接阶段

- **验证** - 验证内容是否满足java虚拟机规范
- **准备** - 给静态变量赋初值
- **解析** - 将常量池中的符号引用替换成指向内存的直接引用



##### 验证

![截屏2024-09-27 20.58.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2020.58.51.png)



![截屏2024-09-27 20.58.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2020.58.16.png)



##### 准备

这个阶段不是赋真实值，为变量默认值

**因为静态变量存在于一块内存**

**如果本身的定义语句没有给变量赋初值，那块内存上可能本来就存在值，导致变量为一个随机值，不太好，为统一规范，先有初值**

但是如果变量是**基本数据类型**的**final修饰的**静态变量 准备阶段直接会将代码中的值进行赋值

因为java虚拟机认为值不会再发生变化，就提前赋值了

 

![截屏2024-09-27 21.00.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2021.00.28.png)



![截屏2024-09-27 21.00.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2021.00.44.png)

##### 解析

**将常量池中的符号引用替换为直接引用**

**符号引用就是在字节码文件中使用编号来访问常量池中的内容**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2021.06.52.png" alt="截屏2024-09-27 21.06.52" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-27%2021.07.07.png" alt="截屏2024-09-27 21.07.07" style="zoom:50%;" />





#### 初始化阶段

- 初始化阶段会执行**静态代码块**中的代码，并**为静态变量赋值**
- 初始化阶段会执行字节码文件中**clinit**（class init）部分的**字节码指令**

clinit 代码块中就包含对静态代码赋值和static代码块

**静态变量赋值 和 static代码块执行顺序跟程序本身的书写顺序一致**

![截屏2024-09-28 08.59.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-28%2008.59.02.png)



**以下几种方式会导致类的初始化**

- 访问一个类的静态变量或者静态方法，但是变量为final修饰且等号右边为常量不会触发初始化
- 调用Class.forName(String className) 底层有方法重载 默认会使类初始化 也可以自己加参数false让他不初始化
- new 一个类的对象
- 执行Main方法的当先类



**以下方式也可以直观看到初始化的类**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-28%2009.06.54.png" alt="截屏2024-09-28 09.06.54" style="zoom:50%;" />

![截屏2024-09-28 09.07.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-28%2009.07.42.png)

**直接用{} 括起来的是实例代码块 在每次创建类的实例的时候都会执行 且在构造器之前执行**

![截屏2024-09-28 09.12.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-28%2009.12.27.png)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-28%2009.21.37.png" alt="截屏2024-09-28 09.21.37" style="zoom:50%;" />

- 直接访问父类的静态变量，不会触发子类的初始化
- 子类的初始化clinit调用之前，会调用父类的clinit初始化方法

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-28%2009.25.16.png" alt="截屏2024-09-28 09.25.16" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-28%2009.26.02.png" alt="截屏2024-09-28 09.26.02" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-28%2009.27.36.png" alt="截屏2024-09-28 09.27.36" style="zoom:33%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-28%2009.28.05.png" alt="截屏2024-09-28 09.28.05" style="zoom:33%;" />





### 类加载器

**ClassLoader** - Java虚拟机提供给应用程序去实现获取类和字节码数据的技术

 ![截屏2024-09-28 09.31.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-28%2009.31.48.png)



![截屏2024-09-28 09.33.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-28%2009.33.23.png)

#### 类加载器的分类

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-28%2009.38.04.png" alt="截屏2024-09-28 09.38.04" style="zoom: 33%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-28%2009.39.57.png" alt="截屏2024-09-28 09.39.57" style="zoom:33%;" />





**arthas可以查看所有的类加载器**

![截屏2024-09-28 09.42.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-28%2009.42.08.png)

##### 启动类加载器

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2008.28.01.png" alt="截屏2024-09-29 08.28.01" style="zoom: 50%;" />

```java
public static void main(String[] args) {
    //在堆上获取的对象
    ClassLoader classLoader = String.class.getClassLoader();
    System.out.println(classLoader); // 结果为null
}
```

**因为String由底层虚拟机的类加载器加载，涉及下层应用的操作视为危险操作，不会显示真正的名称，作了隐藏，其实就是Bootstrap加载器**

arthas中

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2008.38.37.png" alt="截屏2024-09-29 08.38.37" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2008.40.06.png" alt="截屏2024-09-29 08.40.06" style="zoom:50%;" />

![截屏2024-09-29 08.43.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2008.43.58.png)

##### 扩展和应用程序类加载器

-  扩展类加载器和应用程序类加载器都是JDK提供的，使用java编写的类加载器
- 源码都位于sun.misc.Launcher中，是一个静态内部类，继承自URLClassLoader
- 具备通过目录或者指定jar包将字节码文件加载到内存

![截屏2024-09-29 08.47.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2008.47.11.png)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2008.48.45.png" alt="截屏2024-09-29 08.48.45" style="zoom:50%;" />

![截屏2024-09-29 08.49.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2008.49.28.png)



<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2008.50.45.png" alt="截屏2024-09-29 08.50.45" style="zoom:50%;" />

**目录要先写上本来的目录再追加自己的jar包目录，不然原本的jdk的jar就都不能用了(加载目录被覆盖)** 

路径中有空格等特殊字符无法被正确加载，可以对路径加一个双引号

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2008.51.41.png" alt="截屏2024-09-29 08.51.41" style="zoom:50%;" />



应用程序类加载器 - 加载classpath下的类文件

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2008.55.13.png" alt="截屏2024-09-29 08.55.13" style="zoom:50%;" />









**先获取到所有的类加载器的哈希码**

**然后通过哈希码获取到这个类加载器加载的所有类**

![截屏2024-09-29 08.56.07](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2008.56.07.png)

**从这边发现 应用程序类加载器竟然加载了所有类(包括启动类加载器和扩展类加载器应该加载的所有东西)**

#### 双亲委派机制

**Java虚拟机中有多个类加载器，双亲委派机制的核心是解决一个类到底由谁加载的问题**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2009.25.01.png" alt="截屏2024-09-29 09.25.01" style="zoom:50%;" />



- 向上查找，如果其中一个类加载器发现加载过，就直接返回
- 向下加载，判断这个类是不是自己的加载范畴，否则向下去加载
- 向下加载起到了类加载优先级的作用，优先由启动类加载器加载

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2009.26.10.png" alt="截屏2024-09-29 09.26.10" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2009.31.21.png" alt="截屏2024-09-29 09.31.21" style="zoom:50%;" />

**使用应用程序类加载器加载String，最后得到启动类加载器**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2009.34.16.png" alt="截屏2024-09-29 09.34.16" style="zoom:50%;" />





<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2009.33.01.png" alt="截屏2024-09-29 09.33.01" style="zoom: 33%;" />





**每个java实现的类加载器中保存了一个成员变量叫 "父 parent" 类加载器，可以理解为上级而不是继承关系**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2009.37.22.png" alt="截屏2024-09-29 09.37.22" style="zoom:50%;" />

![截屏2024-09-29 09.37.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2009.37.53.png)

**classLoader的父子关系**

![截屏2024-09-29 09.38.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2009.38.58.png)



  

![截屏2024-09-29 09.39.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2009.39.39.png)





**三个类加载器都无法加载的类 会抛出类无法被找到的错误**

#### 打破双亲委派机制

##### 自定义类加载器

- 自定义类加载器并且重写loadClass方法，可以将双亲委派机制的代码去除
- tomcat通过这种方式实现应用之间类隔离，面试篇分享做法

![截屏2024-09-29 10.43.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2010.43.25.png)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2010.44.42.png" alt="截屏2024-09-29 10.44.42" style="zoom:50%;" />

**ClassLoader**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-21%2015.29.18.png" alt="截屏2024-10-21 15.29.18" style="zoom:50%;" />



loadClass **默认是不会连接的** 把resolve的bool值设成了false



- `loadClass` 是加载类的入口，负责委托父类加载器，并在找不到类时调用 `findClass`。 双亲委派机制的核心代码
- `findClass` 是自定义逻辑的实现，**用于找到类的字节码并定义类**。实现一个自定义类加载器，不破坏双亲委派机制的前提下，只重写这个方法，实现拓展字节码获取的渠道

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2010.48.02.png" alt="截屏2024-09-29 10.48.02" style="zoom:50%;" />

**双亲委派机制的核心代码**

![截屏2024-09-29 10.52.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2010.52.51.png)

**自定义**

java底层 对于java.开头的报名会做出报错 认为只有自己系统底层才可以使用

![截屏2024-09-29 11.01.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2011.01.00.png)

```java
public class BreakClassLoader extends ClassLoader{
    private String basePath;
    private final static String FILE_EXTENSION = ".class";

    public void setBasePath(String basePath) {
        this.basePath = basePath;
    }
    private byte[] loadClassData(String name) {
        return null;//省略了
    }

    @Override
    public Class<?> loadClass(String name) throws ClassNotFoundException {
        // 每个类默认父类为Object 如果你指定的文件夹没有这个类 就会报错
        if (name.startsWith("java.")) {
            super.loadClass(name);
        }
        byte[] data = loadClassData(name);
        return defineClass(name , data , 0 , data.length);
    }

    public static void main(String[] args) throws InstantiationException, IllegalAccessException, ClassNotFoundException {
        BreakClassLoader classLoader1 = new BreakClassLoader();
        classLoader1.setBasePath("");

        Class<?> clazz1 = classLoader1.loadClass("java.lang.String");
        clazz1.newInstance();

    }
}
```



<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2011.07.21.png" alt="截屏2024-09-29 11.07.21" style="zoom: 33%;" />





- 自定义类加载器默认会选择应用程序类加载器作为parent（可以通过修改上述构造函数参数来更改classLader）
- 只有**相同类名 + 相同类限定名** 才会被认为是同一个类
- 重写findClass方法(不破坏双亲委派机制，自定义方法从多种渠道去获取字节码，可以实现从数据库中获取类二进制文件等)，验证加载路径后调用findClass
- 自定义类加载器注意 在路径中去添加Object类或者对于其调用super的方法
- 即时使用自定义类加载器，也不能使用java.开头的包名，因为系统会判断，作出安全性报错



##### 线程上下文类加载器

- 利用上下文类加载器加载类 - JDBC JNDI

引入不同的驱动，DriverManager加载不同驱动达到 统一代码对接多种数据库

![截屏2024-09-29 11.18.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2011.18.15.png)

![截屏2024-09-29 11.25.24](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2011.25.24.png)

![截屏2024-09-29 11.28.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2011.28.16.png)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2011.30.10.png" alt="截屏2024-09-29 11.30.10" style="zoom:33%;" /> 

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2011.30.37.png" alt="截屏2024-09-29 11.30.37" style="zoom:50%;" />



![截屏2024-09-29 11.31.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2011.31.44.png)

![截屏2024-09-29 11.34.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2011.34.50.png)



<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-29%2011.37.22.png" alt="截屏2024-09-29 11.37.22" style="zoom:33%;" />



**这个例子中举的DriverManager 是 (线程上下文类加载器 Thread Context ClassLoader TCCL ) 的具体应用，使用spi发现注册的驱动后使用线程上下文类加载器来加载他**



对于一般的线程上下文类加载器的应用

**概念**

- 每个线程都有一个上下文类加载器（TCCL），这个类加载器可以在运行时被设置和获取。

- 使用 `Thread.currentThread().setContextClassLoader()` 方法可以设置上下文类加载器，而 `Thread.currentThread().getContextClassLoader()` 则可以获取。





**线程可以使用指定的类加载器加载类，而不必总是依赖父加载器**

```java
ClassLoader originalClassLoader = Thread.currentThread().getContextClassLoader();
try {
    // 设置线程上下文类加载器为插件的类加载器
    Thread.currentThread().setContextClassLoader(pluginClassLoader);
    
    // 使用当前线程的上下文类加载器加载类
    Class<?> clazz = Thread.currentThread().getContextClassLoader().loadClass("com.example.PluginClass");
    // ...使用clazz实例化对象等操作
} catch (ClassNotFoundException e) {
    e.printStackTrace();
} finally {
    // 恢复原来的上下文类加载器
    Thread.currentThread().setContextClassLoader(originalClassLoader);
}

```



##### Osgi框架的类加载器

- 历史上Osgi框架实现类一套新的类加载机制，允许同级之间委托进行类的加载

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2015.23.02.png" alt="截屏2024-10-04 15.23.02" style="zoom:50%;" />





**arthas的热部署思路**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2015.24.43.png" alt="截屏2024-10-04 15.24.43" style="zoom:50%;" />

![截屏2024-10-04 15.26.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2015.26.04.png)

 注意要指定类加载器的hashCode 不然与这个类关联的类无法被加载

![截屏2024-10-04 15.27.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2015.27.06.png)

我只能说6翻了



**注意**

- 程序重启后，字节码文件会恢复，除非将class文件放入jar包中去跟新
- 使用retransform不能**添加**方法或者字段，也不能去跟新**正在执行中**的方法

#### JDK9之后的类加载器

都继承自URLCLassLoader - 通过特定的目录去找到jar包和字节码文件 （即按照jar包位置去加载字节码文件）

![截屏2024-10-04 15.43.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2015.43.20.png)





![截屏2024-10-04 15.46.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2015.46.05.png)

![截屏2024-10-04 15.46.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2015.46.49.png)

**应用类加载器 的继承结构也同样从  URLCLassLoader 变成了 BuiltinClassLoader 其他没有多大的变化**

这个模块化在干嘛也不知道 后面再说



#### 总结

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2015.49.33.png" alt="截屏2024-10-04 15.49.33" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2016.00.48.png" alt="截屏2024-10-04 16.00.48" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2016.01.32.png" alt="截屏2024-10-04 16.01.32" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2016.02.37.png" alt="截屏2024-10-04 16.02.37" style="zoom:50%;" />

## JVM内存区域

**Java虚拟机在运行Java程序过程中管理的内存区域称为运行时数据区**

![截屏2024-10-04 16.05.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2016.05.45.png)

  ![截屏2024-10-04 16.08.59](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2016.08.59.png)

### 程序计数器

**Program Counter Register - PC寄存器**

**每个线程会通过程序计数器记录当前要执行的字节码指令的地址**





**an example**

![截屏2024-10-04 16.14.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2016.14.36.png)

- 在加载阶段 虚拟机字节码文件中的指令读取到内存之后，会将原文件中的**偏移量转换为内存地址**。每一条字节码指令都会拥有一个内存地址
- 程序计数器会**记录下一行字节码指令的地址**，执行完当前指令后，虚拟机的执行引擎**根据程序计数器执行下一行指令**
- 程序计数器可以**控制程序指令的进行**，实现**分支，跳转，异常**等逻辑
- 在**多线程**执行情况下，java虚拟机需要通过程序计数器**记录CPU切换前**解释执行到哪一句指令**并继续解释运行**

  

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2016.23.02.png" alt="截屏2024-10-04 16.23.02" style="zoom:50%;" />



### 栈

**Java虚拟机栈** - 保存在java中实现的方法

**本地方法栈** - 带有本地关键字的，用C++实现的方法

hotspot中认为这两个栈都是保存方法的，没什么本质区别，所以**只使用了统一的一个栈**







**栈帧 - 保存方法基本信息**

执行方法 入栈

执行结束后会出栈

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2016.28.38.png" alt="截屏2024-10-04 16.28.38" style="zoom:50%;" />

**frames 标签页**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2016.30.28.png" alt="截屏2024-10-04 16.30.28" style="zoom:50%;" />

**Java虚拟机栈随着线程的创建而创建，而回收则会在线程的销毁时进行，由于方法可能会在不同线程中执行，每个线程都会包含一个自己的虚拟机栈**

![截屏2024-10-04 16.34.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2016.34.27.png)

#### 局部变量表

![截屏2024-10-04 16.38.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2016.38.39.png)

![截屏2024-10-04 16.40.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2016.40.25.png)

![截屏2024-10-04 16.41.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2016.41.53.png)



**方法参数在局部变量表中的位置在this(0) 和 临时变量之间**

![截屏2024-10-04 16.42.26](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2016.42.26.png)





<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2016.49.23.png" alt="截屏2024-10-04 16.49.23" style="zoom:50%;" />

![截屏2024-10-04 16.49.07](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2016.49.07.png)

#### 操作数栈

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2016.50.58.png" alt="截屏2024-10-04 16.50.58" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2016.53.21.png" alt="截屏2024-10-04 16.53.21" style="zoom:50%;" />



#### 帧数据

**访问其他类的属性或者方法** 在连接阶段 不会把符号引用转换为直接引用

![截屏2024-10-04 16.54.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2016.54.40.png)

**在当前方法的栈帧中去记录上一个方法运行到的位置**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2016.58.08.png" alt="截屏2024-10-04 16.58.08" style="zoom:50%;" />

**7 astore_1中 存储的变量为存储的Exception e**

![截屏2024-10-04 17.01.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-04%2017.01.25.png)

 

#### 内存溢出

- Java虚拟机栈如果栈帧过多，占用内存超过栈内存可以分配的最大大小就会出现内存溢出
- Java虚拟机栈内存溢出会出现StackOverflowError错误

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.00.57.png" alt="截屏2024-10-05 13.00.57" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.01.42.png" alt="截屏2024-10-05 13.01.42" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.04.07.png" alt="截屏2024-10-05 13.04.07" style="zoom:50%;" />

![截屏2024-10-05 13.07.21](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.07.21.png)

#### 本地方法栈

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.08.31.png" alt="截屏2024-10-05 13.08.31" style="zoom:50%;" />

报错信息中方法栈也包含本地方法

![截屏2024-10-05 13.09.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.09.05.png)



### 堆

![截屏2024-10-05 13.13.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.13.16.png) 

![截屏2024-10-05 13.14.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.14.48.png)

![截屏2024-10-05 13.15.33](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.15.33.png)

![截屏2024-10-05 13.16.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.16.06.png)



max 会 < total    (还要注意dashboard的刷新频率能不能赶上堆内存占满的速度)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.22.41.png" alt="截屏2024-10-05 13.22.41" style="zoom:50%;" />

![截屏2024-10-05 13.24.24](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.24.24.png)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.25.28.png" alt="截屏2024-10-05 13.25.28" style="zoom:50%;" />





<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.26.39.png" alt="截屏2024-10-05 13.26.39" style="zoom: 50%;" />

![截屏2024-10-05 13.27.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.27.47.png)

### 方法区

#### 元信息和运行时常量池

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.29.26.png" alt="截屏2024-10-05 13.29.26" style="zoom:50%;" />  





**常量池和方法是单独摘出来的，用一块内存区域去存放，在InstanceKlass中存放的是他的引用**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.30.16.png" alt="截屏2024-10-05 13.30.16" style="zoom:50%;" />



![截屏2024-10-05 13.32.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.32.45.png)



![截屏2024-10-05 13.34.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.34.46.png)

- 永久代在堆上，有max内存

- 元空间在直接内存上，取决于系统本身，没有标注max内存

![截屏2024-10-05 13.35.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.35.20.png)

 ![截屏2024-10-05 13.39.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.39.58.png)

#### 字符串常量池

![截屏2024-10-05 13.43.21](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.43.21.png)

![截屏2024-10-05 13.44.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.44.51.png)

字符串常量池应该是在解析字节码文件时候就有了，程序运行之前(**通过字节码信息生成的**)

![截屏2024-10-05 13.47.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.47.48.png)

**编译阶段直接把 "1" + "2" 变成"12" 相当于c d 是一模一样的**

![截屏2024-10-05 13.49.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.49.46.png)



![截屏2024-10-05 13.53.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.53.08.png)

![截屏2024-10-05 13.54.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.54.56.png)

**java还是会被提前放入**

![截屏2024-10-05 13.56.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.56.03.png)

#### 静态变量的存储

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.58.10.png" alt="截屏2024-10-05 13.58.10" style="zoom:50%;" />



<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2013.58.31.png" alt="截屏2024-10-05 13.58.31" style="zoom:50%;" />

### 直接内存 

![截屏2024-10-05 14.01.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2014.01.49.png)

![截屏2024-10-05 14.02.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2014.02.27.png)

![截屏2024-10-05 14.04.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2014.04.20.png)















## JVM垃圾回收

### 自动垃圾回收

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2014.43.17.png" alt="截屏2024-10-05 14.43.17" style="zoom:50%;" />

![截屏2024-10-05 14.45.10](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2014.45.10.png)



![截屏2024-10-05 14.45.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2014.45.42.png)

![截屏2024-10-05 14.47.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2014.47.16.png)

### 方法区的回收

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2014.51.45.png" alt="截屏2024-10-05 14.51.45" style="zoom:50%;" />

![截屏2024-10-05 14.52.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2014.52.47.png)

![截屏2024-10-05 14.53.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2014.53.15.png)

 ![截屏2024-10-05 14.53.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2014.53.23.png)

![截屏2024-10-05 14.54.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2014.54.55.png)



![截屏2024-10-05 14.56.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2014.56.57.png)





### 堆回收

**只有无法通过引用获取到对象时，该对象才能被回收**

引用通俗讲就是 可以通过变量或者对象的属性访问到这个对象

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2015.19.09.png" alt="截屏2024-10-05 15.19.09" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-05%2015.25.51.png" alt="截屏2024-10-05 15.25.51" style="zoom:50%;" />



#### 引用计数法

引用计数法会为每个对象维护一个引用计数器，当对象被引用时加1，取消引用时减1

**缺点**

- 每次引用和取消引用都需要**维护计数器**，**对系统性能会有一定的影响**
- 存在**循环引用问题**，AB互相引用的时候会出现对象无法被回收的问题



#### 可达性分析法

在可达性算法中，把对象分为两类 - **垃圾回收的根对象**(GC Root) 和 **普通对象** ， 对象与对象之间存在**引用关系**

GC Root对象一般是不可以被回收的，java虚拟机会持有一个所有GC root 的列表

![截屏2024-10-08 14.14.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2014.14.11.png)

**GC Root 对象**

- 线程Thread对象 - **线程栈帧中的方法参数，局部变量**
- 系统类加载器加载的java.lang.Class对象
- 监视器对象，用来保存同步锁synchronized关键字存储的对象
- 本地方法调用时使用的全局对象

**线程对象**

![截屏2024-10-08 14.19.13](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2014.19.13.png)

- **sun.misc.Launcher 是系统类加载器加载的内容**
- **应用程序类加载器中包含创建类的class对象，而class对象中就包含这个静态变量**

![截屏2024-10-08 14.22.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2014.22.00.png)

**监视器对象**

![截屏2024-10-08 14.25.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2014.25.23.png)

![截屏2024-10-08 14.27.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2014.27.29.png)

**将结果输出**

![截屏2024-10-08 14.29.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2014.29.00.png)

**使用Memory Analyzer来查看(懒得下了)**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2014.31.21.png" alt="截屏2024-10-08 14.31.21" style="zoom:50%;" />

**HotSpot虚拟机使用这种算法判断对象是否可以被回收**

#### 五种对象引用

可达性算法中描述的对象引用，一般指的是**强引用** - GCRoot对象跟普通对象有引用关系，只要这层关系存在，普通对象就不会被回收

除了强引用，还有几种引用方式

- **软引用**
- **弱引用**
- **虚引用**
- **终结器引用**

##### 软引用

软引用相对于强引用是一种**比较弱的引用关系**，如果一个对象**只有软引用可以关联到它**，**当程序内存不足**的时候，就会将软引用中的数据进行回收



防止软引用把重要数据回收，它主要用于**缓存** - 用于提升性能，数据还可以通过一定的方式访问到

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2014.43.56.png" alt="截屏2024-10-08 14.43.56" style="zoom:50%;" />



<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2014.49.16.png" alt="截屏2024-10-08 14.49.16" style="zoom:50%;" />



![截屏2024-10-08 14.49.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2014.49.43.png)

```java
Cache<Object, Object> buzld = Caffeine.newBuilder().softValues().build();
//java常用的缓存框架 - 内部数据会用SoftReference封装
```

![截屏2024-10-08 15.10.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2015.10.34.png)

![截屏2024-10-08 15.15.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2015.15.41.png)



<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2015.19.37.png" alt="截屏2024-10-08 15.19.37" style="zoom:50%;" />

清理SoftReference 的实例代码

```java
import java.lang.ref.ReferenceQueue;  
import java.lang.ref.SoftReference;  
  
public class SoftReferenceExample {  
    public static void main(String[] args) {  
        // 创建一个ReferenceQueue  
        ReferenceQueue<byte[]> queue = new ReferenceQueue<>();  
  
        // 创建一个SoftReference，并关联到ReferenceQueue  
        SoftReference<byte[]> softRef = new SoftReference<>(new byte[1024 * 1024], queue);  
  
        // 模拟内存紧张的情况，触发垃圾回收  
        // 注意：这里的垃圾回收是模拟的，实际情况下JVM的垃圾回收是自动进行的  
        System.gc();  
  
        // 检查ReferenceQueue中是否有被回收的SoftReference对象  
        SoftReference<?> ref = (SoftReference<?>) queue.poll();  
        while (ref != null) {  
            // 清理SoftReference对象  
            ref = null;  
          //在while的判断条件中去用ref接收队列中的参数，每次指向都会发生变化，不用ref = null就可以使对象回收
            // 从队列中继续获取下一个被回收的SoftReference对象  
            ref = (SoftReference<?>) queue.poll();  
        }  
  
        // 此时，如果softRef指向的对象已经被回收，那么softRef本身也将不再有用  
        // 可以在适当的时候将softRef置为null，以便垃圾回收器回收它  
        if (softRef.get() == null) {  
            softRef = null;  
        }  
    }  
}
```

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2015.40.40.png" alt="截屏2024-10-08 15.40.40" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2015.42.15.png" alt="截屏2024-10-08 15.42.15" style="zoom:50%;" />

##### 弱引用

**弱引用对象只要GC运行，就会被回收，GC不依赖可达性分析判断其回收与否。即使在GC Roots能引用到弱引用对象，该对象也可能在GC时被回收**

![截屏2024-10-08 16.19.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2016.19.45.png)



![截屏2024-10-08 16.21.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2016.21.30.png)











##### 虚引用和终结器引用

虚引用 对象被回收 MyPhantomReference 加入到que队列中

MyPhantomReference 继承自 PhantomReference 实现其中的clean方法，实现清理

![截屏2024-10-21 16.29.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-21%2016.29.04.png)

![截屏2024-10-08 16.22.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2016.22.27.png)

ByteBuffer向操作系统申请直接内存，当将变量赋值为null，只是删除了强引用，但是这块内存空间没有被释放

使用虚引用，当这个垃圾被回收，代码删除直接内存空间

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2016.24.54.png" alt="截屏2024-10-08 16.24.54" style="zoom:50%;" />

**独立线程监听对象是否被回收，回收后调用制定的方法**

![截屏2024-10-08 16.27.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2016.27.36.png)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2016.28.27.png" alt="截屏2024-10-08 16.28.27" style="zoom:50%;" />

终结器引用的finalize方法调用不确定，由垃圾回收器决定，基本不使用

![截屏2024-10-08 16.32.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2016.32.30.png)

#### 垃圾回收算法评价标准

![截屏2024-10-08 16.35.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2016.35.27.png)

![截屏2024-10-08 16.36.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2016.36.22.png)



![截屏2024-10-08 16.37.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2016.37.28.png)

![截屏2024-10-08 16.39.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2016.39.58.png)

![截屏2024-10-08 16.40.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2016.40.28.png)

![截屏2024-10-08 16.40.52](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2016.40.52.png)

 三种评价标准不能兼得

一般来说，堆内存越大，最大暂停时间越长，想要减少最大暂停时间，就会降低吞吐量

**不同的垃圾回收算法，适用于不同的场景**

#### 垃圾回收算法

##### 标记清除算法

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2016.49.15.png" alt="截屏2024-10-08 16.49.15" style="zoom:50%;" />

**优点** - 实现简单，只需要在第一阶段给每个对象维护标志位，第二阶段删除对象就可以了

**缺点** - 

**碎片化问题**- -内存连续，在对象被删除后，内存中会出现很多细小可用内存单元，如果我们需要一个比较大的空间，有可能这些内存单元大小过小无法分配

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2016.52.17.png" alt="截屏2024-10-08 16.52.17" style="zoom:50%;" />

**分配速度慢** --还是由于内存碎片的问题，需要维护一个空闲链表，有可能发生每次需要遍历到链表的最后才能获得合适的内存空间

![截屏2024-10-08 16.54.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2016.54.19.png)

##### 复制算法

![截屏2024-10-08 16.56.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2016.56.09.png)

![截屏2024-10-08 16.57.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2016.57.02.png)

##### 标记整理算法

![截屏2024-10-08 16.57.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2016.57.57.png)

![截屏2024-10-08 16.58.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2016.58.15.png)

##### 分代GC算法

![截屏2024-10-08 17.00.07](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2017.00.07.png)

![截屏2024-10-08 17.02.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2017.02.58.png)

![截屏2024-10-08 17.03.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2017.03.43.png)

![截屏2024-10-08 17.08.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2017.08.03.png)

![截屏2024-10-08 17.11.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2017.11.58.png)

 **特殊情况 - 在年轻代中不断Minor GC 但是内存还是一直不够用  即时一些对象年龄没有达到设定的阈值，也会被加入到老年代中去**

**所以在老年代空间不足的时候，先Minor GC 尝试不把没到年龄的对象放入老年代**

**Full GC 持续的时间比较长**

![截屏2024-10-08 17.16.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2017.16.18.png)

#### 垃圾回收器

**为什么分代GC算法要把堆分为年轻代和老年代**

![截屏2024-10-08 17.25.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2017.25.56.png)

 ![截屏2024-10-08 17.28.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2017.28.03.png)

![截屏2024-10-08 17.30.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2017.30.00.png)

##### 垃圾回收器1

**cpu核少 服务器资源匮乏** 适用于单核处理器

![截屏2024-10-08 17.31.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2017.31.49.png)

![截屏2024-10-08 17.32.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2017.32.55.png)



##### 垃圾回收器2

![截屏2024-10-08 18.51.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2018.51.06.png)

某些场景下会退化成Serial Old 单线程

浮动垃圾- 有些垃圾可能无法及时回收

![截屏2024-10-08 18.53.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2018.53.01.png)·

![截屏2024-10-08 18.56.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2018.56.17.png)   



- 在并发清理的阶段，可能同期会产生一些垃圾对象，无法在这次清理中被清理
- 无法按时清理的垃圾如果导致老年代内存依然不足，CMS直接退化(变成单线程，时间特别长)

![截屏2024-10-08 18.58.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2018.58.16.png)

**CMS FULL GC 的原因**

![截屏2024-10-12 09.37.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-12%2009.37.44.png)



##### 垃圾回收器3

![截屏2024-10-08 19.02.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2019.02.44.png)



![截屏2024-10-08 19.04.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2019.04.27.png)

**。。 标记清除 （做了整理的优化）**

**PO GC 在标记阶段完成后，会进行内存压缩（即紧凑），将存活对象移动到内存的一侧，清除中间的空闲块，减少碎片。**

![截屏2024-10-08 19.11.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2019.11.50.png)

Parallel GC 的 Full GC 触发情况

1. **老年代空间不足**： 当老年代无法容纳新对象时，会触发 Full GC。特别是在进行 Minor GC 时，如果新生代的对象需要晋升到老年代，而老年代空间不足，Parallel GC 会触发 Full GC 对整个堆进行回收。



##### ![截屏2024-10-12 09.36.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-12%2009.36.50.png)G1垃圾回收器

![截屏2024-10-08 19.39.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2019.39.39.png)

![截屏2024-10-08 19.41.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2019.41.18.png)

垃圾回收方式

- 年轻代回收 - Young GC
- 混合回收 - Mixed GC

young Gc 不会每次回收所有区域(**这是特性**)

![截屏2024-10-08 19.42.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2019.42.39.png)

![截屏2024-10-08 19.43.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2019.43.34.png)

![截屏2024-10-08 19.45.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2019.45.11.png)

![截屏2024-10-08 19.45.38](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2019.45.38.png)

![截屏2024-10-08 19.46.21](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2019.46.21.png) 

![截屏2024-10-08 19.46.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2019.46.46.png)

- 在最终标记阶段，只会标记引用改变而**漏标**的对象，其余的会在下一次回收完成

- 存活度最低 - Region中存活的对象最少 - 提高复制效率

![截屏2024-10-08 19.48.59](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2019.48.59.png)

![截屏2024-10-08 19.51.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2019.51.22.png)

![截屏2024-10-08 19.52.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2019.52.06.png)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-08%2019.55.04.png" alt="截屏2024-10-08 19.55.04" style="zoom:50%;" />

# 实战篇

## 内存调优

### 什么是内存泄漏

**内存泄漏** - **memory leak**

- 在Java中一个对象不再使用，但是该对象依然在GC ROOT 的引用链上，这个对象就不会被垃圾回收器回收，这种情况就是内存泄漏
- 内存泄漏绝大多数情况都是**  **泄漏引起的，所以一般的内存泄漏都是堆内存泄漏
- 少量的内存泄漏可以容忍，但是如果持续发生内存泄漏，不管多大内存迟早被消耗完，最终导致**内存溢出**，但是产生内存溢出的原因不止内存泄漏这一种





![截屏2024-10-09 09.19.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2009.19.03.png)

![截屏2024-10-09 09.19.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2009.19.43.png)

### 监控内存的常用工具

#### top命令

**load average** (过去一分钟) (过去五分钟) (过去十五分钟) 的**系统负载**(计算机系统在一定时间内处理的工作量，通常用来衡量系统的繁忙程度)

**SHR** -  进程使用过程中共享的第三方库，只需要加载一次，可以实现共享

**进程列表默认按照CPU使用率来排序**，按下大写M，使用内存排序

![ ](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2009.24.32.png)

#### VisualVM

**集群环境要对每一个模块添加访问端口，一一配置**

 ![截屏2024-10-09 09.37.07](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2009.37.07.png)



 **idea插件版**

![截屏2024-10-09 09.41.37](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2009.41.37.png)

**需要在本地配置VisualVM的地址**（我这个电脑他妈是openJDK不是OracleJDK 默认没有VisualVM）

![截屏2024-10-09 16.35.59](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2016.35.59.png)

在远程服务器上开启visualVM监控 指定hostname，访问端口号(获取监控信息)，关闭一系列东西

![截屏2024-10-09 16.56.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2016.56.39.png)

![截屏2024-10-09 16.58.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2016.58.35.png)



![截屏2024-10-09 16.59.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2016.59.19.png)

**生产环境一般不用visual VM连接，其一系列功能很多需要整个线程停止，影响用户使用**



#### arthas

![截屏2024-10-09 17.05.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2017.05.19.png)

 



**多java实例共同注册到Tunnel服务，用户访问Tunnel实现全局管理**

![截屏2024-10-09 17.06.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2017.06.51.png)







![截屏2024-10-09 17.07.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2017.07.46.png)

```xml
<!-- https://mvnrepository.com/artifact/com.taobao.arthas/arthas-spring-boot-starter -->
<dependency>
    <groupId>com.taobao.arthas</groupId>
    <artifactId>arthas-spring-boot-starter</artifactId>
    <version>3.5.4</version>
</dependency>
```

```yml
arthas:
	#tunnel地址
	tunnel-server: ws://localhost:7777/ws
	#tunnel显示的应用名字
	app-name: #{spring.application.name}
	#arthas http访问端口和远程连接的端口
	http-port: 8888
	telnet-port: 9999
```

- **Telnet 端口**：用于通过命令行连接 Arthas，进行交互式操作。

- **HTTP 端口**：用于通过 Web 浏览器访问 Arthas 的图形化控制台，或通过 REST API 执行命令。

这些端口是每一个应用独立的arthas web控制台

当你使用 Arthas 的 `tunnel-server` 功能时，需要指定一个 **Tunnel Server** 的地址。这个地址应该是一个 **WebSocket** 的 URL，格式为 `ws://` 或 `wss://`（带有 SSL 的安全 WebSocket）

下载一个jar包，启动运行 页面访问路径默认为8080 java实例访问tunnel的端口为7777

![截屏2024-10-09 17.25.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2017.25.29.png)

在这个8080总的控制台上，显示所有的tunnel中的引用服务，点击就可以进入操作状态

![截屏2024-10-09 18.55.32](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2018.55.32.png)

#### prometheus + grafana

![截屏2024-10-09 18.58.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2018.58.01.png)

通过http的方式，将一些指标向外暴露

```xml
<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-actuator -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
    <version>3.3.0</version>
</dependency>

```

**配置**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2019.10.32.png" alt="截屏2024-10-09 19.10.32" style="zoom:50%;" />

将java里面的基本信息，java虚拟机信息，数据库连接池信息，以及磁盘信息收集上来，将格式转换为prometheus可以识别的模式

```xml
<!-- https://mvnrepository.com/artifact/io.micrometer/micrometer-registry-prometheus -->
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
    <version>1.12.4</version>
</dependency>
```

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2019.19.22.png" alt="截屏2024-10-09 19.19.22" style="zoom: 33%;" />

**收集间隔15s**

![截屏2024-10-09 19.29.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2019.29.25.png)



#### 堆内存状况对比

**内存泄漏后期，FULL GC的效果越来越不明显**



![截屏2024-10-09 20.24.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2020.24.40.png)

**通过采样功能直观看到哪些数据占据内存的绝大部份**

![截屏2024-10-09 20.28.24](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2020.28.24.png)

###  内存泄漏的常见场景（代码）

**这一些导致内存泄露的场景是比较容易被发现的，通过压力测试容易被排查到**

![截屏2024-10-09 20.30.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2020.30.16.png)

####   **1 - equals hashCode**

- hashCode不正确，使得应该一样的key被分配到了不同的map插槽
- equals不正确，即使hashcode一样，在做比对的时候，还是会被认为是不同key，作链表或红黑树的延伸
- 长期运行，导致HashMap中冗余严重

![截屏2024-10-09 20.33.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2020.33.47.png)

- 定义实体，重写equals和hashCode方法
- 重写时，确保用唯一标识去区分不同的对象，比如id
- hashmap使用尽量编号id等数据作为key，不要将整个实体作为key存放

#### 2 - 内部类引用外部类

-  大量创建内部类，使得大量其外部类无法被回收
- 

![截屏2024-10-09 20.47.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2020.47.23.png)



**创建一个内部类的实例，发现其内部维护了外部类的引用，使得外部类和GC ROOT 强连接 无法被垃圾回收**

静态内部类不会维护对外部类的引用而且可以访问到外部类的private 静态变量

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2020.55.24.png" alt="截屏2024-10-09 20.55.24" style="zoom:50%;" />





**每次调用newList 都要创建Outer的实例**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2021.01.41.png" alt="截屏2024-10-09 21.01.41" style="zoom:50%;" />



**非静态方法中创建的匿名内部类会持有调用者的引用 导致无法垃圾回收**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2021.04.42.png" alt="截屏2024-10-09 21.04.42" style="zoom: 50%;" />







![截屏2024-10-09 21.06.37](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-09%2021.06.37.png)

#### 3 - ThreadLocal

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2015.48.34.png" alt="截屏2024-10-10 15.48.34" style="zoom:50%;" />



**线程池的使用**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2015.49.36.png" alt="截屏2024-10-10 15.49.36" style="zoom: 50%;" />

#### 4 - String的intern方法

![截屏2024-10-10 15.51.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2015.51.51.png)

**大量的字符串放入字符串常量池但是没有被引用，也会被垃圾回收器回收**

**但是大量字符串在字符串常量池且被引用，就容易造成永久代内存溢出**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2015.54.50.png" alt="截屏2024-10-10 15.54.50" style="zoom:50%;" />

#### 5 - 静态字段保存对象

![截屏2024-10-10 15.57.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2015.57.30.png)

**缓存设置过期删除**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2016.00.01.png" alt="截屏2024-10-10 16.00.01" style="zoom:50%;" />

####  6 - 资源没有正常关闭

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2016.01.22.png" alt="截屏2024-10-10 16.01.22" style="zoom:50%;" />

没有使用连接池，没有保存静态数据 方法调用完，conn就不再GCROOT引用链上了

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2016.04.15.png" alt="截屏2024-10-10 16.04.15" style="zoom:50%;" />

### 内存泄漏(并发请求问题)

![截屏2024-10-10 16.08.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2016.08.43.png)

- 并发量大
- 请求数据内存大
- 请求时间长 - 处理慢



**使用Jmeter模拟并发环境**  

### 内存泄漏的解决方案

#### 堆内存快照和MAT分析

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2016.20.15.png" alt="截屏2024-10-10 16.20.15" style="zoom:50%;" />

 **生成内存快照的Java虚拟机参数**

```jvm
-XX+HeapDumpOnOutOfMemoryError - 发生OutOfMemoryError时，自动生成hprof内存快照文件
-XX:HeapDumpPath=<path> - 指定hprof文件的输出路径
```

![截屏2024-10-10 16.28.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2016.28.28.png)

eclipse memory analyzer 使用这个分析hprof文件

#### MAT内存检测的原理

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2016.49.36.png" alt="截屏2024-10-10 16.49.36" style="zoom:50%;" />



<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2016.51.03.png" alt="截屏2024-10-10 16.51.03" style="zoom:50%;" />



**在不内存溢出情况下生成堆内存快照** 在FullGC之前生成内存快照

```
-XX:+HeapDumpBeforeFullGC
```

在`System.gc()`之前生成一次内存快照

![截屏2024-10-10 17.00.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2017.00.53.png)



![截屏2024-10-10 17.01.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2017.01.31.png)

#### 服务器内存快照和MAT技巧

![截屏2024-10-10 17.12.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2017.12.23.png)

```linux
jmap -dump:live,format=b,file=/usr/local/jvm/dump/jmap.hprof 2544893
```

```linux
[arthas@2545177]$ heapdump --live /usr/local/jvm/dump/arthas.hprof
```

**问题**

- MAT需要堆内存快照1.2-1.5倍的内存，对系统的要求高
- 当堆内存快照过大，从服务器下载到本机的速度慢

![截屏2024-10-10 17.19.14](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2017.19.14.png)

https://eclipse.dev/mat/downloads.php



**生成内存泄漏检测图 + 总览图 + 组件图**

![截屏2024-10-10 17.20.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2017.20.17.png)

![截屏2024-10-10 17.21.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2017.21.42.png)

## 项目案例实战

### 查询大数据量导致的内存溢出

**分页查询文章接口的内存溢出**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2019.09.56.png" alt="截屏2024-10-10 19.09.56" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2019.10.41.png" alt="截屏2024-10-10 19.10.41" style="zoom:50%;" />

![截屏2024-10-10 19.20.33](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2019.20.33.png)

![截屏2024-10-10 19.21.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2019.21.34.png)

**根据内存快照图的怀疑对象定位到方法位置不太明白！！！**

### Mybatis导致的内存溢出

 ![截屏2024-10-10 19.32.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2019.32.51.png)

### 导出大文件内存溢出

![截屏2024-10-10 19.36.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2019.36.03.png)

![截屏2024-10-10 19.36.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2019.36.20.png)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2019.45.22.png" alt="截屏2024-10-10 19.45.22" style="zoom:50%;" />

**hutool**

![截屏2024-10-10 19.45.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2019.45.51.png)

**easy Excel** - 分批写入(导致时间变长 )

![截屏2024-10-10 19.47.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2019.47.39.png)

### ThreadLocal使用时占用大量内存

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2019.50.32.png" alt="截屏2024-10-10 19.50.32" style="zoom: 33%;" />

在拦截器中为ThreadLocal添加模拟用户信息，但是由于controller层的报错，导致postHandle无法执行，数据无法被释放，导致内存泄漏

应该把消除内存的代码放到**afterCompletion**中去，不管怎么样最后都会执行

![截屏2024-10-10 19.58.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2019.58.08.png)

**手动配置了tomcat的线程数，使用小于核心线程数(最小为10)的线程不会被回收，导致数据内存一直滞留，无法被释放**

![截屏2024-10-10 20.00.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2020.00.01.png)

### 文章内容审核接口的内存问题

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2020.06.03.png" alt="截屏2024-10-10 20.06.03" style="zoom:50%;" />



 调用外部接口可能等待时间长，使用异步处理，提前返回给用户结果



**使用线程池的弊端**

- 线程池参数设置不合理，导致**大量线程**的创建(核心线程和临时线程)或者**消息队列中保存大量的数据**
- **任务没有持久化**，一旦触发拒绝策略或者服务宕机，可能出现任务丢失



![截屏2024-10-10 20.07.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2020.07.09.png)



**调用异步线程池**

![截屏2024-10-10 20.09.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2020.09.01.png)

**线程池配置**

![截屏2024-10-10 20.09.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2020.09.17.png)



**生产者消费者模型**

![截屏2024-10-10 20.16.14](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2020.16.14.png)

  

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2020.19.11.png" alt="截屏2024-10-10 20.19.11" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2020.21.16.png" alt="截屏2024-10-10 20.21.16" style="zoom:50%;" />

 这个设计模式的失败原因可能是

消息消化速度慢，导致队列中积压大量的数据，超出容量的上限

可以将一部分数据加入到数据库中去作持久化，再通过一定机制将数据库中的数据加入到任务队列中去(实现复杂)









![截屏2024-10-10 20.25.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2020.25.50.png)

![截屏2024-10-10 20.27.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2020.27.08.png)

**可以指定并发处理量concurrency**

![截屏2024-10-10 20.27.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2020.27.45.png)

![截屏2024-10-10 20.28.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2020.28.35.png)

### btrace和arthas在线定位问题

![截屏2024-10-10 20.44.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2020.44.34.png)

生成堆内存快照的时候，用户访问请求时间变长，程序无法对外提供服务

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2020.46.33.png" alt="截屏2024-10-10 20.46.33" style="zoom:50%;" />

![截屏2024-10-10 20.48.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2020.48.28.png)

![截屏2024-10-10 20.51.10](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2020.51.10.png)

**监控方法** 定位对象

![截屏2024-10-10 20.52.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2020.52.50.png)

![截屏2024-10-10 20.54.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2020.54.27.png)

**在脚本依赖中引入btrace安装完后目录中的三个依赖**

![截屏2024-10-10 20.55.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2020.55.45.png)

**脚本内容**

**jstack - 打印所有栈信息**

![截屏2024-10-10 20.56.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2020.56.43.png)

**将btrace bin目录 加入到环境变量**<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2020.58.28.png" alt="截屏2024-10-10 20.58.28" style="zoom:50%;" />

 

**上传运行脚本**

![截屏2024-10-10 21.01.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2021.01.25.png)

## GC调优

GC调优指的是对**垃圾回收(Garbage Collection)** 进行调优，GC调优的主要目标是**避免垃圾回收引起程序性能下降**



**分为三部分**

- 通用JVM参数设置
- 特定垃圾回收器的Jvm参数设置
- 解决由频繁FULL GC引起的程序性能问题



GC调优**没有唯一标准答案**，如何**调优和硬件**，**程序本身**，**使用情况**有关，学习调优**工具和方法**



### GC调优核心指标

![截屏2024-10-10 21.14.21](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2021.14.21.png)



![截屏2024-10-10 21.16.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2021.16.44.png)

 ![截屏2024-10-10 21.17.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2021.17.51.png)



### GC调优方法

![截屏2024-10-10 21.19.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2021.19.34.png)

#### GC调优常用工具

##### jstat ![截屏2024-10-10 21.20.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2021.20.15.png)



##### visualVM插件

![截屏2024-10-10 21.22.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2021.22.48.png)



**安装插件**

![截屏2024-10-10 21.23.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2021.23.41.png)

##### prometheus + Grafana

![截屏2024-10-10 21.26.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2021.26.27.png)



##### GC日志

![截屏2024-10-10 21.29.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-10%2021.29.27.png)



**将GC日志可视化的一些工具**

![截屏2024-10-11 08.29.10](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2008.29.10.png)



![截屏2024-10-11 08.32.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2008.32.20.png)

![截屏2024-10-11 08.33.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2008.33.02.png)



#### 常见的GC模式

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2008.38.16.png" alt="截屏2024-10-11 08.38.16" style="zoom:50%;" />



![截屏2024-10-11 08.39.21](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2008.39.21.png)



![截屏2024-10-11 08.40.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2008.40.45.png)

![截屏2024-10-11 08.41.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2008.41.27.png)

![截屏2024-10-11 08.44.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2008.44.11.png)

#### GC问题手段

![截屏2024-10-11 08.47.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2008.47.22.png)

#### 基础JVM参数设置

##### 初始和最大堆内存

![截屏2024-10-11 08.56.26](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2008.56.26.png)

![截屏2024-10-11 08.59.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2008.59.20.png)

#####   元空间设置![截屏2024-10-11 09.13.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2009.13.42.png)

##### 虚拟机栈

![截屏2024-10-11 09.23.24](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2009.23.24.png)

##### 不建议的参数

![截屏2024-10-11 09.24.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2009.24.36.png)

![截屏2024-10-11 09.26.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2009.26.19.png)

![截屏2024-10-11 09.26.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2009.26.58.png)

![截屏2024-10-11 09.28.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2009.28.03.png)

##### jvm参数模版

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2009.28.34.png" alt="截屏2024-10-11 09.28.34" style="zoom:50%;" />

#### 垃圾回收器的选择

![截屏2024-10-11 09.30.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2009.30.31.png)

![截屏2024-10-11 09.31.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2009.31.42.png)

 **变更垃圾回收器 不断做压力测试**

#### 优化垃圾回收器参数

![截屏2024-10-11 09.44.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2009.44.04.png)

![ ](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2009.46.13.png)



**参考官方文档，难度大，效果差**





### 案例实战  

#### 内存调优+GC调优

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2015.35.03.png" alt="截屏2024-10-11 15.35.03" style="zoom:50%;" />

使用heaphero.io进行堆内存快照的在线分析



使用visualVM类似的内存监控工具导出的堆内存快照，在导出前会做一次Full GC 导致部分数据。。

使用命令行导出，去掉live指令，使得不在GC ROOT引用链上的对象也会被保留

![截屏2024-10-11 15.44.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2015.44.54.png)

![截屏2024-10-11 15.42.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2015.42.54.png)



<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2015.46.40.png" alt="截屏2024-10-11 15.46.40" style="zoom:50%;" />



<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2015.47.26.png" alt="截屏2024-10-11 15.47.26" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2015.48.52.png" alt="截屏2024-10-11 15.48.52" style="zoom:50%;" />



<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2015.49.34.png" alt="截屏2024-10-11 15.49.34" style="zoom:50%;" />



![截屏2024-10-11 15.50.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2015.50.20.png)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2015.53.27.png" alt="截屏2024-10-11 15.53.27" style="zoom:50%;" />

## 性能调优 

![截屏2024-10-11 16.19.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2016.19.29.png)

### 性能调优解决的问题

![截屏2024-10-11 16.22.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2016.22.08.png)

![截屏2024-10-11 16.22.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2016.22.49.png)

**线程被耗尽**

![截屏2024-10-11 16.23.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2016.23.09.png)

### 性能调优方法

#### 线程转储的查看方式

![截屏2024-10-11 16.24.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2016.24.08.png)

**jstack**

![截屏2024-10-11 16.26.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2016.26.15.png)



**visualVM - Threads -> Thread Dump**

![截屏2024-10-11 16.26.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2016.26.54.png)

![截屏2024-10-11 16.28.52](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2016.28.52.png)

![截屏2024-10-11 16.31.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2016.31.41.png)

#### 定位线程CPU占有率高的问题

![截屏2024-10-11 16.34.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2016.34.57.png)

**top -c 按照cpu占有率排序**

![截屏2024-10-11 16.35.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2016.35.05.png)

![截屏2024-10-11 16.36.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2016.36.04.png)

 ![截屏2024-10-11 16.37.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2016.37.01.png)

![截屏2024-10-11 16.37.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2016.37.47.png)

![截屏2024-10-11 16.39.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2016.39.15.png)

![截屏2024-10-11 16.40.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2016.40.36.png)

#### 接口响应时间长的问题

![截屏2024-10-11 16.41.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2016.41.46.png)

#####  arthas - trace![截屏2024-10-11 16.45.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2016.45.11.png)

![截屏2024-10-11 16.49.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2016.49.02.png)

**显示详细信息**

![截屏2024-10-11 16.54.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2016.54.31.png)

**过滤认为正常的监控信息**

![截屏2024-10-11 16.55.26](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2016.55.26.png)

**代码中含有大量循环，使用arthas监控，arthas对程序的运行速度影响(底层使用动态代理对程序监控) 会变大**



##### arthas - watch

![截屏2024-10-11 16.58.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2016.58.31.png)

![截屏2024-10-11 16.59.52](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2016.59.52.png)





![截屏2024-10-11 17.00.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2017.00.53.png)

#### 火焰图定位接口响应时间长的问题

**定位偏底层的性能问题**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2017.02.53.png" alt="截屏2024-10-11 17.02.53" style="zoom:50%;" />

![截屏2024-10-11 17.06.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2017.06.22.png)

​	**在java栈 方法调用中 比较宽的方法，就是调用时间长的方法**

![截屏2024-10-11 17.11.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2017.11.54.png)

#### 死锁问题的检测

![截屏2024-10-11 17.19.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2017.19.31.png)

 



**死锁**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2017.21.13.png" alt="截屏2024-10-11 17.21.13" style="zoom:50%;" />



死锁把线程耗尽，导致别的正常业务无法执行

**-l 打印锁的信息**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2017.23.40.png" alt="截屏2024-10-11 17.23.40" style="zoom:50%;" />

 ![截屏2024-10-11 17.24.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2017.24.56.png)

 ![截屏2024-10-11 17.25.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2017.25.54.png)



![截屏2024-10-11 17.26.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2017.26.36.png)

**上传线程转储文件**

**解决死锁问题**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-11%2017.28.21.png" alt="截屏2024-10-11 17.28.21" style="zoom:50%;" />

#### 基准测试框架JMH 

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-12%2020.33.12.png" alt="截屏2024-10-12 20.33.12" style="zoom:50%;" />

![截屏2024-10-12 20.35.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-12%2020.35.00.png)





![截屏2024-10-12 20.36.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-12%2020.36.22.png)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-12%2020.39.27.png" alt="截屏2024-10-12 20.39.27" style="zoom:50%;" />

**运行方法**

![截屏2024-10-12 20.39.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-12%2020.39.53.png)





**正确编写测试方法，几个需要注意的点**

- 死代码问题，如果一个测试方法中的变量没有被返回，jit出于优化作用，会将相关代码忽略掉
- 黑洞的用法，使用黑洞消费变量，避免jit忽略掉这个变量的相关代码



<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-12%2020.46.18.png" alt="截屏2024-10-12 20.46.18" style="zoom:50%;" />





如果要在程序中运行，在main方法中去指定运行的类，fork是对main用 ，jar包线程数还是由注解决定的

![截屏2024-10-12 20.42.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-12%2020.42.56.png)



**为测试结果生成json文件**

![截屏2024-10-12 20.47.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-12%2020.47.23.png)

上传json结果文件到在线网站，进行结果分析

![截屏2024-10-12 20.48.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-12%2020.48.00.png)

![截屏2024-10-12 20.54.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-12%2020.54.04.png)

### 案例实战

![截屏2024-10-12 20.55.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-12%2020.55.27.png)

![截屏2024-10-12 21.07.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-12%2021.07.06.png)

 ![截屏2024-10-12 21.08.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-12%2021.08.19.png)

## 高级篇

### GraalVM

#### 简介

##### 特性

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2009.12.10.png" alt="截屏2024-10-14 09.12.10" style="zoom:50%;" />

![截屏2024-10-14 09.13.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2009.13.55.png)

![截屏2024-10-14 09.14.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2009.14.47.png)

##### 版本

![截屏2024-10-14 09.15.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2009.15.18.png)



##### 环境搭建

![截屏2024-10-14 09.16.33](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2009.16.33.png)

#### GraalVM的两种运行模式

![ ](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2009.20.54.png)

 关闭Graal编译器  和普通jdk的jit 速度差不多

![截屏2024-10-14 09.24.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2009.24.53.png)

![截屏2024-10-14 09.27.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2009.27.25.png)

**优化 - 还是先将java文件编译成字节码文件，再通过AOT变成native image本地镜像文件**

 <img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2009.30.09.png" alt="截屏2024-10-14 09.30.09" style="zoom:50%;" />

**先将代码编译，aot class文件**

![截屏2024-10-14 09.33.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2009.33.15.png)

#### 存在问题

![截屏2024-10-14 18.21.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2018.21.46.png)

#### 应用场景

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2018.42.13.png" alt="截屏2024-10-14 18.42.13" style="zoom:50%;" />

##### SpringBoot3构建GraalVM应用

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2018.22.52.png" alt="截屏2024-10-14 18.22.52" style="zoom:50%;" />

**加入GraalVM依赖**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2018.24.13.png" alt="截屏2024-10-14 18.24.13" style="zoom:50%;" />

**maven需要下载并配置**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2018.27.15.png" alt="截屏2024-10-14 18.27.15" style="zoom:50%;" />

 **AOT 在 启动速度 cup占有率 内存占有率上都优于 JIT**

#####  将GraalVM应用部署到函数计算

![截屏2024-10-14 18.45.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2018.45.30.png)

![截屏2024-10-14 18.47.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2018.47.00.png)

 ![截屏2024-10-14 18.48.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2018.48.34.png)

**编写Dockerfile文件**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2018.50.51.png" alt="截屏2024-10-14 18.50.51" style="zoom:50%;" />



**创建镜像** 将代码放到github仓库上

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2018.53.25.png" alt="截屏2024-10-14 18.53.25" style="zoom:50%;" />

![截屏2024-10-14 18.54.33](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2018.54.33.png)

**创建函数**

![截屏2024-10-14 18.55.52](/Users/sunnyday/Library/Application Support/typora-user-images/截屏2024-10-14 18.55.52.png)

##### 将GraalVM应用部署到Serverless

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2018.43.11.png" alt="截屏2024-10-14 18.43.11" style="zoom:50%;" />

![截屏2024-10-14 18.44.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2018.44.11.png)

![截屏2024-10-14 19.02.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2019.02.04.png)

![截屏2024-10-14 19.03.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2019.03.50.png)





#### 参数优化和故障诊断

![截屏2024-10-14 19.08.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2019.08.58.png)

![截屏2024-10-14 19.13.10](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2019.13.10.png)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2019.14.48.png" alt="截屏2024-10-14 19.14.48" style="zoom:50%;" />

![截屏2024-10-14 19.17.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2019.17.16.png)

![截屏2024-10-14 19.18.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2019.18.56.png)

### 新一代GC

#### 垃圾回收器的技术演进

![截屏2024-10-14 19.28.26](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2019.28.26.png)

**Shenandoah ZGC 早期不分代**

![截屏2024-10-14 19.30.52](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2019.30.52.png)

![截屏2024-10-14 19.32.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2019.32.23.png)

#### Shenandoah GC

![截屏2024-10-14 19.35.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2019.35.45.png)

![截屏2024-10-14 19.37.38](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2019.37.38.png) 

![截屏2024-10-14 19.39.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2019.39.12.png)

**shenandoah GC 适合处理小对象，大对象的处理效率非常差**

![截屏2024-10-14 19.47.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2019.47.25.png)



#### ZGC

![截屏2024-10-14 19.48.59](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2019.48.59.png)



![截屏2024-10-14 20.01.14](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2020.01.14.png)

![截屏2024-10-14 20.02.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2020.02.04.png)



ZGC**参数设置**

![截屏2024-10-14 20.03.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2020.03.16.png)

![截屏2024-10-14 20.03.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2020.03.48.png)

**相比shenandoah 对于大对象的回收出色多了**

 ![截屏2024-10-14 20.05.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2020.05.03.png)

**Huge Pages是Linux内核提供的一种内存管理优化技术，它通过使用较大的内存页（如2MB或1GB），减少了页表条目数量和页表查找次数，从而提高内存使用效率和系统性能。Huge Pages非常适合那些需要大量连续内存的大型应用程序，如数据库、大数据处理、高性能计算等**

![ ](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2020.05.58.png)

**配置系统大页后，会直接分配内存，结束使用后一定要echo 0 ..... 结束大页的内存分配**

![截屏2024-10-14 20.11.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2020.11.03.png)

#### 实战案例

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2020.13.53.png" alt="截屏2024-10-14 20.13.53" style="zoom: 33%;" />

![截屏2024-10-14 20.18.26](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2020.18.26.png)

### Java工具介绍

![截屏2024-10-14 20.37.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2020.37.01.png)

###  Java Agent

**Java Agent 技术是JDK提供的用来编写java工具的技术，使用这种技术生成一种特殊类型的jar包，这种jar包可以让Java程序运行其中的代码**

![截屏2024-10-14 20.43.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2020.43.08.png)

#### 两种加载模式

**Java Agent技术实现让Java程序执行独立的Java Agent程序中的代码**

- 静态加载模式
- 动态加载模式

![截屏2024-10-14 20.46.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2020.46.41.png)





![截屏2024-10-14 20.48.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2020.48.15.png)

#### 环境搭建

##### 静态加载

![截屏2024-10-14 20.50.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2020.50.08.png)

```xml
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-assembly-plugin</artifactId>
      <configuration>
        <!-- 将所有依赖都打入同一个jar包中-->
        <descriptorRefs>
          <descriptorRef>jar-with-dependencies</descriptorRef>
        </descriptorRefs>
        <!--指定java agent相关配置文件-->
        <archive>
          <manifestFile>src/main/resources/MANIFEST.MF</manifestFile>
        </archive>
      </configuration>
    </plugin>
  </plugins>
</build>
```

```java
public class AgentMain {
    //premain
    public static void premain(String agentArgs, Instrumentation inst) {
        System.out.println("premain functions");
    }

    //agentmain
    public static void agentmain(String agentArgs, Instrumentation inst) {
        System.out.println("agentmain functions");
    }
}
```

```MANIFEST.MF
Manifest-Version: 1.0
Premain-Class: com.sjc.javaAgent.AgentMain
Agent-Class: com.sjc.javaAgent.AgentMain
Can-Redefine-Classes: true
Can-Retransform-Classes: true
Can-Set-Native-Method-Prefix: true
```

##### 动态加载

![截屏2024-10-14 21.16.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-14%2021.16.22.png)

```java
public class AttachMain {

    public static void main(String[] args) throws IOException, AttachNotSupportedException, AgentLoadException, AgentInitializationException {
        //获取进程虚拟机对象
        VirtualMachine vm = VirtualMachine.attach("37845");
        //执行java agent里边的agentmain方法
        vm.loadAgent("/Users/sunnyday/Library/Mobile Documents/com~apple~CloudDocs/mass/agentDemo1/target/agentDemo1-1.0-SNAPSHOT-jar-with-dependencies.jar");
    }
}
```

**这个VirtualMachine类是从JDK 9 开始默认就存在的**

#### 简化版arthas

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2009.23.04.png" alt="截屏2024-10-16 09.23.04" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2009.24.27.png" alt="截屏2024-10-16 09.24.27" style="zoom:50%;" />

假如你的javaAgent代码变更，需要重启springboot项目，并要重新查看他的进程id，比较麻烦

优化代码

实现在javaAgent控制台中，可以输出jps列表，并手动输入需要监控的java进程ID

```java
public class AttachMain {

    public static void main(String[] args) throws IOException, AttachNotSupportedException, AgentLoadException, AgentInitializationException {
        //获取进程列表，让用户手动进行输入
        //执行jps命令，打印所有进程列表
        Process jps = Runtime.getRuntime().exec("jps");
        //包装缓存流
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(jps.getInputStream()));
        try {
            String line;
            while ((line = bufferedReader.readLine()) != null) {
                System.out.println(line);
            }
        } finally {
            if (bufferedReader != null) {
                bufferedReader.close();
            }
        }
        //输入进程id
        Scanner scanner = new Scanner(System.in);
        String processId = scanner.next();
        //获取进程虚拟机对象
        VirtualMachine vm = VirtualMachine.attach(processId);
        //执行java agent里边的agentmain方法
        vm.loadAgent("/Users/sunnyday/Library/Mobile Documents/com~apple~CloudDocs/mass/agentDemo1/target/agentDemo1-1.0-SNAPSHOT-jar-with-dependencies.jar");
    }
}
```

##### 内存使用情况

- jvm封装这些Mbean对象
- 远程获取  可以调用其暴露的接口

![截屏2024-10-16 09.37.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2009.37.17.png)

![截屏2024-10-16 09.38.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2009.38.40.png)

```java
public class MemoryCommand {
    //打印所有内存信息
    public static void printMemory () {
        List<MemoryPoolMXBean> memoryPoolMXBeans = ManagementFactory.getMemoryPoolMXBeans();

        System.out.println("堆内存 ： ");
        //堆内存
        getMemoryInfo(memoryPoolMXBeans , MemoryType.HEAP);

        System.out.println("非堆内存 ： ");
        //非堆内存
        getMemoryInfo(memoryPoolMXBeans , MemoryType.NON_HEAP);

    }

    private static void getMemoryInfo (List<MemoryPoolMXBean> memoryPoolMXBeans , MemoryType type) {
        memoryPoolMXBeans.stream().filter(x -> x.getType().equals(type))
                .forEach(x -> {
                    StringBuilder sb = new StringBuilder();
                    sb.append("name:")
                            .append(x.getName())
                            .append("used:")
                            .append(x.getUsage().getUsed() / 1024 / 1024)
                            .append("m")

                            .append(" committed:")
                            .append(x.getUsage().getCommitted() / 1024 / 1024)
                            .append("m")

                            .append(" max:")
                            .append(x.getUsage().getMax() / 1024 /1024)
                            .append("m");
                    System.out.println(sb);
                });
    }
}
```

```java
//agentmain
public static void agentmain(String agentArgs, Instrumentation inst) {
    MemoryCommand.printMemory();
}
```

##### 生成堆内存快照

**查看直接内存使用情况，生成堆内存快照**

![截屏2024-10-16 15.11.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2015.11.47.png)

 **获取直接内存**

```java
System.out.println("打印nio 相关内存");
//打印nio相关内存
try {
    Class clazz = Class.forName("java.lang.management.BufferPoolMXBean");
    List<BufferPoolMXBean> bufferPoolMXBeans = ManagementFactory.getPlatformMXBeans(clazz);

    //打印内容
    for (BufferPoolMXBean bufferPoolMXBean : bufferPoolMXBeans) {
        StringBuilder sb = new StringBuilder();
        sb.append("name:")
                .append(bufferPoolMXBean.getName())
                .append("used:")
                .append(bufferPoolMXBean.getMemoryUsed() / 1024 / 1024)
                .append("m")

                .append(" capacity:")
                .append(bufferPoolMXBean.getTotalCapacity()/ 1024 / 1024)
                .append("m");

        System.out.println(sb);
    }
} catch (ClassNotFoundException e) {
    throw new RuntimeException(e);
}
```

![截屏2024-10-16 15.22.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2015.22.56.png)

**生成堆内存快照**

```java
//agentmain
public static void agentmain(String agentArgs, Instrumentation inst) {
    //MemoryCommand.printMemory();
    MemoryCommand.heapDump();
}
```

```java
public static void heapDump () {
    SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd-HH-mm");
    HotSpotDiagnosticMXBean platformMXBean = ManagementFactory.getPlatformMXBean(HotSpotDiagnosticMXBean.class);

    try {
        platformMXBean.dumpHeap(simpleDateFormat.format(new Date() + ".hprof") , true);//只保存存活对象
    } catch (IOException e) {
        throw new RuntimeException(e);
    }
}
```

##### 栈信息和类加载器

**打印栈信息**

```java
//获取线程运行信息
public static void printThreadInfo () {
    ThreadMXBean threadMXBean = ManagementFactory.getThreadMXBean();
    //是否支持监视器和同步器
    //重载方法 有第三个参数 指定栈的深度
    ThreadInfo[] threadInfos = threadMXBean.dumpAllThreads(threadMXBean.isObjectMonitorUsageSupported(),
            threadMXBean.isSynchronizerUsageSupported());
    for (ThreadInfo threadInfo : threadInfos) {
        StringBuilder sb = new StringBuilder();
        sb.append("name :")
                .append(threadInfo.getThreadName())
                .append( "threadId: ")
                .append(threadInfo.getThreadId())
                .append(" threadState: ")
                .append(threadInfo.getThreadState()
                );

        System.out.println(sb);
        //打印栈信息
        StackTraceElement[] stackTrace = threadInfo.getStackTrace();
        for (StackTraceElement stackTraceElement : stackTrace) {
            System.out.println(stackTraceElement);
        }
    }
}
```

![截屏2024-10-16 15.39.38](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2015.39.38.png)

```java
public class ClassCommand {

    //打印所有类加载器
     public static void printAllClassLoader(Instrumentation inst) {
         HashSet<ClassLoader> classLoaders = new HashSet<>();
         //获取所有的类
         Class[] allLoadedClasses = inst.getAllLoadedClasses();
         Arrays.stream(allLoadedClasses).forEach(
                 loadClass -> {
                     ClassLoader classLoader = loadClass.getClassLoader();
                     classLoaders.add(classLoader);
                 }
         );
          //打印类加载器
         String collect = classLoaders.stream().map(x -> {
             if (x == null) {
                 //启动类加载器
                 return "BootStrapClassLoader";
             }
             return x.getName();
         }).filter(Objects::nonNull).distinct().sorted(String::compareTo).collect(Collectors.joining(","));
         //去重过滤掉相同的反射优化的类加载器
         System.out.println(collect);
     }
}
```

```java
//agentmain
public static void agentmain(String agentArgs, Instrumentation inst) {
    //MemoryCommand.printMemory();
    //MemoryCommand.heapDump();
    //ThreadCommend.printThreadInfo();
    ClassCommand.printAllClassLoader(inst);
}
```

##### 打印类的源码

- 获得内存中的类的字节码信息，利用Instrumentation提供的转换器来获取字节码信息
- 通过反编译工具将字节码信息还原成源代码信息

![截屏2024-10-16 16.04.38](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2016.04.38.png)

![截屏2024-10-16 16.05.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2016.05.36.png)

**使用jd-core 反编译工具**

```xml
<!-- https://mvnrepository.com/artifact/org.jd/jd-core -->
<dependency>
  <groupId>org.jd</groupId>
  <artifactId>jd-core</artifactId>
  <version>1.1.3</version>
</dependency
```

```java
//打印类的源代码
public static void printClassSourceCode (Instrumentation inst) {
    System.out.println("请输入类名: ");
    //输入类名
    Scanner scanner = new Scanner(System.in);
    String className = scanner.next();

    //根据类名找到class对象
    Class[] allLoadedClasses = inst.getAllLoadedClasses();
    for (Class allLoadedClass : allLoadedClasses) {
        if (allLoadedClass.getName().equals(className)) {
            //添加转换器
            ClassFileTransformer classFileTransformer = new ClassFileTransformer() {
                // 类的转换器本来可以增强类信息，这边是起获取到类的字节码信息的功能
                @Override
                public byte[] transform(ClassLoader loader, String className, Class<?> classBeingRedefined, ProtectionDomain protectionDomain, byte[] classfileBuffer) throws IllegalClassFormatException {
                    //通过jd-core反编译并打印源代码
                    try {
                        printJDCoreSourceCode(classfileBuffer , className);
                    } catch (Exception e) {
                        throw new RuntimeException(e);
                    }
                    return ClassFileTransformer.super.transform(loader, className, classBeingRedefined, protectionDomain, classfileBuffer);
                }
            };
            inst.addTransformer(classFileTransformer , true);

            // 触发转换器
            try {
                inst.retransformClasses(allLoadedClass);
            } catch (UnmodifiableClassException e) {
                throw new RuntimeException(e);
            }finally {
                //删除转换器
                inst.removeTransformer(classFileTransformer);
            }
        }
    }
}
//通过jd-core打印源代码
private static void printJDCoreSourceCode (byte[] bytes , String className) throws Exception {
     //load对象
    Loader loader = new Loader() {
        @Override
        public boolean canLoad(String s) {
            return true;
        }

        @Override
        public byte[] load(String s) throws LoaderException {
            return bytes;
        }
    };
    Printer printer = new Printer() {
        protected static final String TAB = "  ";
        protected static final String NEWLINE = "\n";

        protected int indentationCount = 0;
        protected StringBuilder sb = new StringBuilder();

        @Override public String toString() { return sb.toString(); }

        @Override public void start(int maxLineNumber, int majorVersion, int minorVersion) {}
        @Override public void end() {
            // 打印源代码
            System.out.println(sb);
        }

        @Override public void printText(String text) { sb.append(text); }
        @Override public void printNumericConstant(String constant) { sb.append(constant); }
        @Override public void printStringConstant(String constant, String ownerInternalName) { sb.append(constant); }
        @Override public void printKeyword(String keyword) { sb.append(keyword); }
        @Override public void printDeclaration(int type, String internalTypeName, String name, String descriptor) { sb.append(name); }
        @Override public void printReference(int type, String internalTypeName, String name, String descriptor, String ownerInternalName) { sb.append(name); }

        @Override public void indent() { this.indentationCount++; }
        @Override public void unindent() { this.indentationCount--; }

        @Override public void startLine(int lineNumber) { for (int i=0; i<indentationCount; i++) sb.append(TAB); }
        @Override public void endLine() { sb.append(NEWLINE); }
        @Override public void extraLine(int count) { while (count-- > 0) sb.append(NEWLINE); }

        @Override public void startMarker(int type) {}
        @Override public void endMarker(int type) {}
    };
    //通过jd-core 方法打印
    ClassFileToJavaSourceDecompiler decompiler = new ClassFileToJavaSourceDecompiler();

    decompiler.decompile(loader, printer, className);

}
```

##### 字节码增强框架

![截屏2024-10-16 16.49.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2016.49.28.png)

![截屏2024-10-16 16.50.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2016.50.31.png)

###### ASM

![截屏2024-10-16 16.51.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2016.51.18.png)

**ASM 添加字节码指令的一个DEMO**

```java
public class ASMDemo {
    static String fileNmae = "";
    public static void main(String[] args) throws IOException {
        //从本地读取一个字节码文件byte[]
        byte[] bytes = FileUtils.readFileToByteArray(new File(fileNmae));

        //通过ASM修改字节码文件
        //将二进制文件转换成可以解析的内容
        ClassReader classReader = new ClassReader(bytes);

        //创建vistor对象，修改字节码信息
        ClassWriter classWriter = new ClassWriter(0);
        ClassVisitor classVisitor = new ClassVisitor(ASM7 , classWriter){

            // methodVistor 字节码中读取到method进行运行
            @Override
            public MethodVisitor visitMethod(int access, String name, String descriptor, String signature, String[] exceptions) {
                //原始对象
                MethodVisitor mv = super.visitMethod(access, name, descriptor, signature, exceptions);
                //返回自定义MethodVistor
                MethodVisitor methodVisitor = new MethodVisitor(this.api , mv) {
                    //修改字节码指令

                    @Override
                    public void visitCode() {
                        // 插入一个字节码指令 ICONST_0
                        visitInsn(ICONST_0);
                    }
                };
                return methodVisitor;
            }
        };

        classReader.accept(classVisitor , 0);

        //将修改完的字节码信息写入文件，进行替换
        FileUtils.writeByteArrayToFile(new File(fileNmae ) , classWriter.toByteArray());
    }
}
```

**ASM应用**

```java
public static void enhanceClass (Instrumentation inst) {
    System.out.println("请输入类名: ");
    //输入类名
    Scanner scanner = new Scanner(System.in);
    String className = scanner.next();

    //根据类名找到class对象
    Class[] allLoadedClasses = inst.getAllLoadedClasses();
    for (Class allLoadedClass : allLoadedClasses) {
        if (allLoadedClass.getName().equals(className)) {
            //添加转换器
            ClassFileTransformer classFileTransformer = new ClassFileTransformer() {
                // 类的转换器本来可以增强类信息，这边是起获取到类的字节码信息的功能
                @Override
                public byte[] transform(ClassLoader loader, String className, Class<?> classBeingRedefined, ProtectionDomain protectionDomain, byte[] classfileBuffer) throws IllegalClassFormatException {
                   //通过asm对类进行增强，返回字节码信息
                    return AsmEnhancer.enhanceCLass(classfileBuffer);
                }
            };
            inst.addTransformer(classFileTransformer , true);

            // 触发转换器
            try {
                inst.retransformClasses(allLoadedClass);
            } catch (UnmodifiableClassException e) {
                throw new RuntimeException(e);
            }finally {
                //删除转换器
                inst.removeTransformer(classFileTransformer);
            }
        }
    }
}
```

```Java
public class AsmEnhancer {

    public static byte[] enhanceCLass (byte[] bytes) {
        ClassWriter classWriter = new ClassWriter(0);
        ClassVisitor classVisitor = new ClassVisitor(ASM7, classWriter){
            @Override
            public MethodVisitor visitMethod(int access, String name, String descriptor, String signature, String[] exceptions) {
                MethodVisitor methodVisitor = super.visitMethod(access, name, descriptor, signature, exceptions);

                //这个增强方法不屑了，太难了
                //return new MyMethodVisitor(this.api , methodVisitor);
                return null;
            }
        };
        ClassReader classReader = new ClassReader(bytes);
        classReader.accept(classVisitor , 0);
        return classWriter.toByteArray();
    }
}
```

![截屏2024-10-16 17.28.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2017.28.01.png)

###### ByteBuddy打印方法执行参数和耗时

![截屏2024-10-16 19.00.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2019.00.03.png)

https://bytebuddy.net/#/

![截屏2024-10-16 19.01.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2019.01.15.png)

**byteBuddy 和 JavaAgent作了整合，所以也需要导入对应的依赖**

```xml
<!-- https://mvnrepository.com/artifact/net.bytebuddy/byte-buddy -->
<dependency>
  <groupId>net.bytebuddy</groupId>
  <artifactId>byte-buddy</artifactId>
  <version>1.14.10</version>
</dependency>
<!-- https://mvnrepository.com/artifact/net.bytebuddy/byte-buddy-agent -->
<dependency>
  <groupId>net.bytebuddy</groupId>
  <artifactId>byte-buddy-agent</artifactId>
  <version>1.14.10</version>
  <scope>test</scope>
</dependency>
```

固定代码

 

```java
public static void enhanceClass (Instrumentation inst) {
    System.out.println("请输入类名: ");
    //输入类名
    Scanner scanner = new Scanner(System.in);
    String className = scanner.next();

    //根据类名找到class对象
    Class[] allLoadedClasses = inst.getAllLoadedClasses();
    for (Class allLoadedClass : allLoadedClasses) {
        if (allLoadedClass.getName().equals(className)) {
           // 使用byteBuddy
            new AgentBuilder.Default()
                    // 禁止byteBuddy 处理时修改类名
                    .disableClassFormatChanges()
                    //处理时使用retransform增强
                    .with(AgentBuilder.RedefinitionStrategy.RETRANSFORMATION)
                    //打印错误日志
                    .with(new AgentBuilder.Listener.WithTransformationsOnly(AgentBuilder.Listener.StreamWriting.toSystemOut()))
                    //匹配哪些类
                    .type(ElementMatchers.named(className))
                    //增强，使用MyAdvice通知，对所有方法都进行增强
                    .transform((builder, typeDescription, classLoader, moudle, protectionDomain) ->
                        builder.visit(Advice.to(MyAdvice.class).on(ElementMatchers.any()))
                    )
                    .installOn(inst);

        }
    }
}
```

```java
public class AsmEnhancer {

    public static byte[] enhanceCLass (byte[] bytes) {
        ClassWriter classWriter = new ClassWriter(0);
        ClassVisitor classVisitor = new ClassVisitor(ASM7, classWriter){
            @Override
            public MethodVisitor visitMethod(int access, String name, String descriptor, String signature, String[] exceptions) {
                MethodVisitor methodVisitor = super.visitMethod(access, name, descriptor, signature, exceptions);

                //这个增强方法不屑了，太难了，写不了一点
                //return new MyMethodVisitor(this.api , methodVisitor);
                return null;
            }
        };
        ClassReader classReader = new ClassReader(bytes);
        classReader.accept(classVisitor , 0);
        return classWriter.toByteArray();
    }
}
```

##### APM系统和数据采集

![截屏2024-10-16 19.28.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2019.28.12.png)

![截屏2024-10-16 19.30.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2019.30.04.png)

![截屏2024-10-16 19.45.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2019.45.57.png)

![截屏2024-10-16 19.47.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2019.47.06.png)

![截屏2024-10-16 19.47.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2019.47.46.png)

**协助文件输出的依赖**

```xml
<dependency>
  <groupId>commons-io</groupId>
  <artifactId>commons-io</artifactId>
  <version>2.11.0</version>
</dependency>
```

```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.PARAMETER, ElementType.TYPE_PARAMETER})
public @interface AgentParam {
    String value();
}
```

```java
//premain
public static void premain(String agentArgs, Instrumentation inst) {
    // 使用byteBuddy
    new AgentBuilder.Default()
            // 禁止byteBuddy 处理时修改类名
            .disableClassFormatChanges()
            //处理时使用retransform增强
            .with(AgentBuilder.RedefinitionStrategy.RETRANSFORMATION)
            //打印错误日志
            .with(new AgentBuilder.Listener.WithTransformationsOnly(AgentBuilder.Listener.StreamWriting.toSystemOut()))
            //匹配哪些类 - controller 层
            .type(ElementMatchers.isAnnotatedWith(ElementMatchers.named("org.springframework.web.bind.annotation.RestController"))
                    .or(ElementMatchers.named("org.springframework.web.bind.annotation.Controller")))
            //增强，使用MyAdvice通知，对所有方法都进行增强
            .transform((builder, typeDescription, classLoader, moudle, protectionDomain) ->
                    builder.visit(Advice
                            .withCustomMapping()
                                    .bind(AgentParam.class , agentArgs)
                            .to(TimingAdvice.class).on(ElementMatchers.any()))
            )
            .installOn(inst);
}
```

```java
//统计耗时，打印方法名，类名
public class TimingAdvice {

    /**
     * 方法进入时候执行
     * @return 返回方法开始时候的时间
     */
    @Advice.OnMethodEnter
    public static long enter ( ) {
        return System.nanoTime();
    }

    /**
     * 方法退出时候执行
     * @param value 上面enter中的返回值
     */
    @Advice.OnMethodExit
    public static void exit (@Advice.Enter long value,
                             @Advice.Origin("#t") String className,
                             @Advice.Origin("#m") String methodName,
                             @AgentParam("agent.log") String logName){
        String str = methodName + "@" + className + "耗时为" + (System.nanoTime() - value) + "纳秒\n";
        try {
            FileUtils.writeStringToFile(new File(logName) , str , StandardCharsets.UTF_8 , true);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

## 原理篇

![截屏2024-10-16 20.20.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2020.20.34.png)

### 栈上数据的存储

这里的内存占用，是在堆上或者数组中分配的空间大小，**栈上的实现更加复杂**

![截屏2024-10-16 20.23.38](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2020.23.38.png)

![截屏2024-10-16 20.27.10](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2020.27.10.png)

**这里的设计是不完美的，为了同时适配32 和 64 位操作系统，统一对long 和 double 占用2个slot**

**对于64位操作系统会有8个字节的空虚，没办法，从设计中作出让步**

![截屏2024-10-16 20.29.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2020.29.22.png)

![截屏2024-10-16 20.29.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2020.29.49.png)

#### boolean 在栈上的存储

- bool类型当作int来处理 01

![截屏2024-10-16 20.34.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2020.34.29.png)

  ![截屏2024-10-16 20.40.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2020.40.20.png)

![截屏2024-10-16 20.40.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2020.40.57.png)

![截屏2024-10-16 20.41.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2020.41.36.png)

![截屏2024-10-16 20.43.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2020.43.46.png)

### 对象在堆上的存储

![截屏2024-10-16 20.47.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2020.47.56.png)

#### 标记字段

**分代年龄只有4位，所以分代晋升年龄不能超过15**

![截屏2024-10-16 20.53.26](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2020.53.26.png)

![截屏2024-10-16 20.55.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2020.55.56.png)

![截屏2024-10-16 20.58.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2020.58.46.png)



![截屏2024-10-16 20.59.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2020.59.00.png)

#### 元数据指针

![截屏2024-10-16 21.06.32](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2021.06.32.png)

**指针压缩**

![截屏2024-10-16 21.06.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2021.06.46.png)

![截屏2024-10-16 21.09.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2021.09.28.png)

![截屏2024-10-16 21.10.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2021.10.54.png)

![截屏2024-10-16 21.11.14](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2021.11.14.png)

#### 对象数据

**空白填充**

![截屏2024-10-16 21.15.52](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2021.15.52.png)

**提升系统内存行的使用效率**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2021.16.37.png" alt="截屏2024-10-16 21.16.37" style="zoom:50%;" />

**如果一个对象不是 8字节的倍数，那么会存在一个缓存行有多个数据的情况，如果A的数据边跟，要删除所有的缓存，其中一部分缓存和b在同一个缓存条中，删除后，再获取B的数据，需要重新从内存中去读取**

![截屏2024-10-16 21.20.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2021.20.49.png)

字段重排列，保证数据在同一个缓存行，**不和其他数据共用一个缓存行** 减少夸缓存行



**例如，一个 `long` 字段（8 字节）的偏移量必须是 8 的倍数，这样可以保证它起始于 8 字节边界。**

![截屏2024-10-16 21.21.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2021.21.17.png)

 ![截屏2024-10-16 21.23.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2021.23.03.png)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-16%2021.26.16.png" alt="截屏2024-10-16 21.26.16" style="zoom:50%;" />



### 方法调用的原理

**方法调用的本质是通过字节码指令的执行，能在栈上创建栈帧，并执行调用方法中的字节码执行**

![截屏2024-10-17 14.17.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2014.17.48.png)

![截屏2024-10-17 14.19.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2014.19.27.png)

- **Invoke方法的核心作用就是找到字节码指令并执行**

- **Invoke指令执行时，需要找到方法区中instanceKlass中保存的方法相关的字节码信息**

- **方法区中有很多类，类中有很多方法，invoke 精确定位到方法位置 分为 静态绑定 和 动态绑定**

#### 静态绑定

![截屏2024-10-17 14.30.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2014.30.53.png)

 **静态绑定只能适用于处理静态方法，私有方法，或者使用final修饰的方法，因为这些方法不能被继承后重写**

#### 动态绑定

![截屏2024-10-17 14.35.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2014.35.23.png)

![截屏2024-10-17 14.36.37](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2014.36.37.png)

**子类虚方法表 复制父类 修改自己重写方法的地址 增加自己增加方法的地址**

![截屏2024-10-17 14.39.24](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2014.39.24.png)

![截屏2024-10-17 14.44.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2014.44.28.png)

### 异常捕获的原理

![截屏2024-10-17 15.36.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2015.36.53.png)

![截屏2024-10-17 15.37.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2015.37.19.png)

![截屏2024-10-17 15.38.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2015.38.41.png)

![截屏2024-10-17 15.39.37](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2015.39.37.png)

![截屏2024-10-17 15.39.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2015.39.58.png)

![截屏2024-10-17 15.40.38](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2015.40.38.png)

![截屏2024-10-17 15.41.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2015.41.36.png)

### JIT即时编译技术

#### C1和C2即时编译器

![截屏2024-10-17 15.50.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2015.50.30.png)

![截屏2024-10-17 15.51.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2015.51.53.png)

 ![截屏2024-10-17 15.53.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2015.53.18.png)

![截屏2024-10-17 15.55.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2015.55.12.png)

![截屏2024-10-17 15.55.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2015.55.35.png)

![截屏2024-10-17 15.56.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2015.56.27.png)

####  JIT优化手段

**JIT编译器主要优化手段是方法内联和逃逸分析**

##### 方法内联

![截屏2024-10-17 16.09.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2016.09.31.png)

![截屏2024-10-17 16.10.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2016.10.48.png) 

![截屏2024-10-17 16.15.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2016.15.02.png)

![截屏2024-10-17 16.15.33](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2016.15.33.png)

##### 逃逸分析

![截屏2024-10-17 16.20.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2016.20.09.png)

 **锁消除**

![截屏2024-10-17 16.21.26](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2016.21.26.png)

**标量替换**

![截屏2024-10-17 16.21.52](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2016.21.52.png)

#### JIT优化建议

![截屏2024-10-17 16.25.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2016.25.01.png)

### 垃圾回收器原理

#### G1垃圾回收器

**流程略**

##### 年轻代回收

![截屏2024-10-17 16.52.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2016.52.17.png)



- 精确
- 需要大量的对象扫描效率太低

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2016.54.49.png" alt="截屏2024-10-17 16.54.49" style="zoom:33%;" />



- 如果对象太多会占用很大的内存空间
- 存在错标，eden中的对象被一个 非GC ROOT 对象引用的 Old区对象引用 其实两个对象都应该被回收，但都回收不了

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2016.56.11.png" alt="截屏2024-10-17 16.56.11" style="zoom:33%;" />

**只记录非收集区的对象**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2016.58.37.png" alt="截屏2024-10-17 16.58.37" style="zoom:33%;" />



<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2017.01.25.png" alt="截屏2024-10-17 17.01.25" style="zoom:33%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2017.02.34.png" alt="截屏2024-10-17 17.02.34" style="zoom:33%;" /> 

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2017.04.11.png" alt="截屏2024-10-17 17.04.11" style="zoom:33%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2017.05.19.png" alt="截屏2024-10-17 17.05.19" style="zoom:33%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2017.06.32.png" alt="截屏2024-10-17 17.06.32" style="zoom:33%;" />

**写后屏障指令由用户线程完成，大量用户线程同时去修改记忆集，需要加锁，这样用户线程会受到阻塞，影响业务代码**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2017.10.15.png" alt="截屏2024-10-17 17.10.15" style="zoom:33%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2017.10.32.png" alt="截屏2024-10-17 17.10.32" style="zoom:33%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2017.10.51.png" alt="截屏2024-10-17 17.10.51" style="zoom:33%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2017.11.25.png" alt="截屏2024-10-17 17.11.25" style="zoom:33%;" />





![截屏2024-10-17 17.13.26](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2017.13.26.png)



##### 混合回收

![截屏2024-10-17 17.15.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2017.15.47.png)

**灰色对象会被加入到灰色队列中**

![截屏2024-10-17 17.17.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2017.17.53.png)

 ![截屏2024-10-17 17.19.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2017.19.42.png)

**并发标记**

![截屏2024-10-17 17.20.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2017.20.56.png)

![截屏2024-10-17 17.22.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2017.22.36.png)

![截屏2024-10-17 17.23.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2017.23.31.png)

保证标记一开始存在于引用链的对象即使在并发状态被断开，也能挺过这次垃圾回收

![截屏2024-10-17 17.24.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2017.24.56.png)

这是复制过程

![截屏2024-10-17 17.28.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2017.28.15.png)



**SATB** 在并发标记期间，应用线程**每次写入对象引用时**，都会触发 SATB 写屏障，**将旧的对象引用加入一个 “处理队列” 中**，以确保原先指向的对象被正确地标记为存活。写屏障的主要目的是保证快照的完整性(保证快照中的存活对象还会存活)，即使对象引用发生了变化，也不会遗漏某个活跃对象。

SATB 是一种写屏障机制，保证了在并发垃圾收集过程中，即使对象引用发生变化，也能确保所有活跃对象都被正确标记。

#### ZGC原理

![截屏2024-10-17 19.22.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.22.27.png)

![截屏2024-10-17 19.24.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.24.48.png)

![截屏2024-10-17 19.25.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.25.29.png)

![截屏2024-10-17 19.26.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.26.48.png)

通过着色指针判断对象处于垃圾回收的哪一个阶段

![截屏2024-10-17 19.27.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.27.34.png)

![截屏2024-10-17 19.27.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.27.49.png)

**ZGC 让操作系统使用地址的后几位去寻找对象** 

![截屏2024-10-17 19.29.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.29.25.png)

![截屏2024-10-17 19.30.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.30.42.png)

**红色由0 - 1**

![截屏2024-10-17 19.31.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.31.01.png)

![截屏2024-10-17 19.31.37](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.31.37.png)

 ![截屏2024-10-17 19.32.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.32.31.png)

![截屏2024-10-17 19.33.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.33.25.png)

![截屏2024-10-17 19.34.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.34.01.png)

并发处理阶段删除转移映射表

![截屏2024-10-17 19.34.26](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.34.26.png)

mark0 - 这一轮的垃圾回收标记阶段

mark1 - 上一轮的垃圾回收标记阶段 交替使用

![截屏2024-10-17 19.43.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.43.36.png)

![截屏2024-10-17 19.43.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.43.53.png)

![截屏2024-10-17 19.44.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.44.15.png)

![ ](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.44.27.png)

**读屏障**

- 提升速度
- 保证用户的数据是准确的

####  Shenandoah GC

![截屏2024-10-17 19.46.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.46.54.png)

![截屏2024-10-17 19.47.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.47.27.png)

![截屏2024-10-17 19.48.38](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.48.38.png)

![截屏2024-10-17 19.49.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.49.34.png)

![截屏2024-10-17 19.49.59](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.49.59.png)

![截屏2024-10-17 19.52.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.52.09.png)

![截屏2024-10-17 19.52.24](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-17%2019.52.24.png)

## 面试小记

### tomcat的自定义类加载器

![截屏2024-10-21 16.13.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-21%2016.13.02.png)

**common类加载器主要加载tomcat自身使用以及应用使用的jar包，默认配置在catalina.properties文件中。**

**common.loader="${catalina.base)/lib", "${catalina.base}/lib/*jar"**

![截屏2024-10-21 15.41.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-21%2015.41.39.png)





**catalina类加载器主要加载tomcat自身使用的jar包，不让应用使用，默认配置在catalina.properties文件中。**

***server.loader=***

**默认配置为空，为空时catalina加载器和common加载器是同一个。**	

**自己配置的tomcat插件可以使用这个类加载器**

![截屏2024-10-21 15.50.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-21%2015.50.57.png)



**shared类加载器主要加载应用使用的jar包，不让tomcat使用，默认配置在catalina.properties文件中。**

**shared.loader=**

**默认配置为空，为空时shared加载器和common加载器是同-一个。**

![截屏2024-10-21 15.59.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-21%2015.59.18.png)



**ParallelwebappClassLoader类加载器可以多线程并行加载应用中使用到的类，每个应用都拥有一个自己的该类加载器。**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-21%2016.02.57.png" alt="截屏2024-10-21 16.02.57" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-21%2016.04.55.png" alt="截屏2024-10-21 16.04.55" style="zoom:50%;" />

是否开启代理，默认false 打破双亲委派机制

![截屏2024-10-21 16.06.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-21%2016.06.54.png)

![截屏2024-10-21 16.08.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-21%2016.08.43.png)





**JasperLoader类加载器负责加载JSP文件编译出来的class字节码文件，为了实现热部署（不重启让修改的jsp生效），每一个jsp文件都由一个独立的JasperLoader负责加载**

每当修改jsp文件，类加载器就会重新创建一次，用于加载新的jsp文件





### ThreadLocal为什么要使用弱引用

![截屏2024-10-21 16.34.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-21%2016.34.45.png)

![截屏2024-10-21 16.35.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-21%2016.35.45.png)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-21%2016.36.59.png" alt="截屏2024-10-21 16.36.59" style="zoom:50%;" />

尽管`ThreadLocalMap`是线程私有的，但多个`ThreadLocal`实例可能会在同一个线程中存在。如果一个线程中使用了多个不同的`ThreadLocal`实例，每个实例都需要在`ThreadLocalMap`中占据一个槽位来存储它的值

![截屏2024-10-21 16.38.10](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-21%2016.38.10.png)

![截屏2024-10-21 16.39.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-21%2016.39.40.png)

执行别的threadLocal 的 set get remove 方法时，会触发条件删除 没有关联threadlocal 的Entry对象

![截屏2024-10-21 16.40.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-21%2016.40.44.png)

![ ](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-10-21%2016.41.58.png)
