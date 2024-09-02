# 概述

String是常量，创建之后不可以改变 ->地址不变?...?

字符串的产生方式

字符串字面值存储在字符串常量池中，可以共享

字符串的toString方法

# String源码

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final char value[];

    /** Cache the hash code for the string */
    private int hash; // Default to 0

    /** use serialVersionUID from JDK 1.0.2 for interoperability */
    private static final long serialVersionUID = -6849794470754667710L;

    /**
     * Class String is special cased within the Serialization Stream Protocol.
     *
     * A String instance is written into an ObjectOutputStream according to
     * <a href="{@docRoot}/../platform/serialization/spec/output.html">
     * Object Serialization Specification, Section 6.2, "Stream Elements"</a>
     */
    private static final ObjectStreamField[] serialPersistentFields =
        new ObjectStreamField[0];
     //......
     
}
```

```java
synchronized (this) {
    print(x);
    newLine();
}//
这段代码表示使用当前对象（即使用这个代码的对象实例）作为同步锁，对一段代码块进行同步控制。使用 synchronized (this) 表示当前对象实例将作为同步锁。这意味着在同一时刻只有一个线程能够进入这个同步代码块，其他线程需要等待直到当前线程执行完毕释放锁。

这种同步控制的方式可以确保在多线程环境下，同一时刻只有一个线程能够执行这段代码块，从而避免了竞态条件和并发问题。
```



```java
/**
 * This object (which is already a string!) is itself returned.
 *
 * @return  the string itself.
 */
public String toString() {
    return this;
}
```

# 三种创建方式

```java
public String(String original) {
    this.value = original.value;
    this.hash = original.hash;
}
```



```java
public String(char value[]) {
    this.value = Arrays.copyOf(value, value.length);
}
```



# hashTable

**HashTable是一种散列表，它实现了Map接口**

哈希表是根据键的哈希值决定数据在表中的存储位置，以加快查找的速度

HashTable的方法是同步的，因此它是线程安全的。这意味着在多线程环境中使用HashTable时，不需要额外的同步措施。然而，由于同步带来的开销，HashTable的性能通常低于HashMap。因此，在不需要线程安全性的场景中，HashMap通常是一个更好的选择。



**在使用HashTable时，键和值都不能为null**

**HashTable的初始容量和加载因子是固定的**

# hashcode

在 Java 中，`hashCode()` 方法用于获取对象的哈希码值。哈希码是一个 `int` 类型的数值，它可以用来快速确定对象的存储位置，或者用于在哈希表等数据结构中进行快速查找、比较等操作。

`hashCode()` 方法定义在 `Object` 类中，因此所有 Java 类都继承了这个方法。默认情况下，`hashCode()` 方法是根据对象的内存地址计算的，但是根据需要，类可以重写 `hashCode()` 方法来根据对象的内容生成哈希码。重写 `hashCode()` 方法的目的通常是使得相等的对象具有相等的哈希码，以确保对象在使用哈希数据结构时能够正确地被识别和检索。

在 Java 中，当你使用一些集合类（如 `HashMap`、`HashSet` 等）时，通常需要重写对象的 `hashCode()` 和 `equals()` 方法，以确保对象能够正确地存储和检索。这两个方法一起工作，`hashCode()` 方法用于确定对象的存储位置，而 `equals()` 方法用于在同一个哈希桶中查找相等的对象。

总而言之，`hashCode()` 方法在 Java 中主要用于快速查找和比较对象，特别是在使用哈希数据结构时更为常见和重要。



hashmap -> object -> hashcode -> index

# 编译期常量折叠的机制

Java 编译器在编译时会对字符串字面量进行特殊处理，将其直接放入字符串常量池中。这个过程称为编译期常量折叠（Compile-Time Constant Folding）。**无论这些字符串字面量是否真正被使用，它们都会在编译阶段添加到字符串常量池中。**

这个特性的优点是可以提高运行时的性能，因为字符串常量池的重用机制可以减少内存的占用和提高字符串的比较效率。但也需要注意，这可能会导致一些额外的内存开销，尤其是对于那些未被实际使用但仍存在于代码中的字符串字面量。



即使某些字符串字面量在程序运行时没有被直接引用或使用，但只要它们在代码中存在，Java 编译器就会将其在编译时添加到字符串常量池中。这就是编译期常量折叠的机制。**因此，即使这些字符串在运行时可能不会被使用到，它们仍然会占用内存空间并留在字符串常量池中。**

![截屏2024-04-02 21.02.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-02%2021.02.34.png)

![截屏2024-04-02 21.01.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-02%2021.01.11.png)



# 栈,堆,方法区

**JVM内存管理的重要组成部分**

**栈：**

存储局部变量和基本数据类型的值

每当一个方法被调用时，JVM会在栈中为该方法创建一个新的栈帧

存储局部变量表、操作数栈、动态链接、方法出口等信息

**堆：**

存放所有的对象实例

堆内存是所有线程共享的区域，因此访问堆内存中的数据需要同步机制

**方法区：**

已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据

**Java 8及之前版本的方法区被称为“永久代”（PermGen），而在Java 8及之后的版本中，方法区被移到了元空间（Metaspace），这使得JVM可以动态地扩展和收缩方法区的内存大小**（永久代大小固定）

**常量池---->是堆内存的一个逻辑部分**

# 常量池

![截屏2024-04-02 19.47.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-02%2019.47.22.png)

# StringBuilder StringBuffer

- String(JDK1.0)：不可变字符序列 ，效率低但是复用率高。

- StringBuffer(JDK1.0)：可变字符序列、效率较高、线程安全。

- StringBuilder(JDK 5.0)：可变字符序列、效率最高、线程不安全

  

- String、StringBuffer、StringBuilder的选择：

  1、如果字符串中存在大量的修改操作，可以选择StrinBuffer和StringBuilder其中之一。

  2、如果字符串中存在大量的修改操作而且在单线程的情况下，使用StringBuilder。

  3、如果字符串中存在大量的修改操作而且在多线程的情况下，使用StringBuffer。

  4、如果字符串修改很少、被多个对象引用，使用String。这个在配置信息的时候应用广泛。

# 创建对象的抽象问题

![截屏2024-04-02 20.50.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-02%2020.50.17.png)

![截屏2024-04-02 20.50.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-02%2020.50.53.png)

![截屏2024-04-02 20.59.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-02%2020.59.17.png)
