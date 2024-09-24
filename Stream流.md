**集合进阶**

# 不可变集合

## 定义

**不可变集合的应用场景**

- 如果数据不能被修改，把它防御性地拷贝到不可变集合中是个很好的实践
- 当集合对象被不可信的库调用时，不可变形式是安全的

## 创建

since 9

![截屏2024-09-13 15.49.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-13%2015.49.09.png)

**获取唯一的set集合的时候要保证数据的唯一性**![截屏2024-09-13 15.55.33](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-13%2015.55.33.png)



**获取唯一的map集合，要保证键不能重复，并且参数有上限，不能超过10个键值对**

set和list中的of方法都是可变参数，只有map是从1-10都是一个限定参数的of方法

![截屏2024-09-13 16.00.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-13%2016.00.01.png)

**因为map中键和值的类型可能不同，泛型需要两个，在方法的参数传递中，可变参数要位于参数列表的最后，多个可变参数不能共存**

**除非把键和值看作整体变成一个泛型，使用新的方法**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-13%2016.03.13.png" alt="截屏2024-09-13 16.03.13" style="zoom:33%;" />

使用流程 - 创建普通的map，获取键值对的集合并转成数组作为参数使用上述的方法

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-13%2016.08.41.png" alt="截屏2024-09-13 16.08.41" style="zoom:25%;" />

**jdk10后 使用copyoOf可以直接把一个普通的map变成一个不可变的map集合**	底层就是上述的ofEntries

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-13%2016.11.13.png" alt="截屏2024-09-13 16.11.13" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-13%2016.20.07.png" alt="截屏2024-09-13 16.20.07" style="zoom:50%;" />

# Stream流的思想和获取

![截屏2024-09-13 16.46.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-13%2016.46.12.png)



## 得到steam流

![截屏2024-09-13 16.46.59](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-13%2016.46.59.png)

**双列集合  需要使用keySet和entrySet转为单列集合使用**

```java
public static void main(String[] args) {
    System.out.println("--------------------------");
    ArrayList<String> list = new ArrayList<>();
    Collections.addAll(list,"1","2","3");
    //ArrayList是Collection的实现类，直接调用stream方法
    Stream<String> stream1 = list.stream();
    System.out.println("--------------------------");
    Map<String,Integer> map = new HashMap<>();
    map.put("a",1);
    map.put("b",2);
    map.put("c",3);
    //第一种
    map.keySet().stream();
    //第二种
    map.entrySet().stream();
    System.out.println("--------------------------");
    int[] arr = {1,2,3};
    Arrays.stream(arr);
    //第四种的stream.of参数列表是可变参数T...  ---- 只有引用数据类型的数组才能被正确使用 基本数据类型的数组会被整体看作一个元素
    System.out.println("--------------------------");
    Stream.of(1,2,3,4).forEach(System.out::println);

}
```

 <img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-13%2017.10.24.png" alt="截屏2024-09-13 17.10.24" style="zoom:25%;" />

# Stream流的中间方法

![截屏2024-09-13 17.12.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-13%2017.12.03.png)

- 中间方法，返回新的steam流，**原来的steam流只能使用一次**，使用链式编程
- 修改steam流中的数据，不会影响原来，集合或者数组中的数据

**过滤 - 为函数式接口**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-13%2017.22.42.png" alt="截屏2024-09-13 17.22.42" style="zoom:50%;" />

- **distinct底层使用的是hashSet**

- **concat合并a和b，如果a，b类型不同，那么合并后的结果为ab的共同父类，无法调用子类方法**

![截屏2024-09-24 20.24.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-24%2020.24.01.png)

- **map**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-24%2020.26.19.png" alt="截屏2024-09-24 20.26.19" style="zoom:33%;" />

![截屏2024-09-24 20.27.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-24%2020.27.34.png)

# Stream流终结方法

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-24%2020.28.21.png" alt="截屏2024-09-24 20.28.21" style="zoom:50%;" />

- **forEach便利** 

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-24%2020.30.59.png" alt="截屏2024-09-24 20.30.59" style="zoom:33%;" />

- **toArray**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-24%2020.32.09.png" alt="截屏2024-09-24 20.32.09" style="zoom:33%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-24%2020.36.17.png" alt="截屏2024-09-24 20.36.17" style="zoom:50%;" />



# collect收集方法

**收集流中的数据，放到list map 和 Set中去**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-24%2020.43.38.png" alt="截屏2024-09-24 20.43.38" style="zoom: 50%;" />

**map**

收集到map中，键不能重复，不然就会报错

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-24%2020.50.39.png" alt="截屏2024-09-24 20.50.39" style="zoom: 33%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-24%2020.45.16.png" alt="截屏2024-09-24 20.45.16" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-24%2020.47.53.png" alt="截屏2024-09-24 20.47.53" style="zoom:33%;" />
