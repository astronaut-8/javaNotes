# 1.入门案例

## 1.1**引入依赖**

集成了Mybatis和Mybatis-plus的所有功能，并且实现自动装配

可以用MybatisPlus的starter替代Mybatis的starter

```xml
<dependency>
  <groupId>com.baomidou</groupId>
  <artifactId>mybatis-plus-boot-starter</artifactId>
  <version>3.5.3.1</version>
</dependency>
```

## 1.2定义Mapper

![截屏2024-03-31 16.54.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2016.54.19.png)

```java
@Mapper
public interface BookMapper extends BaseMapper<Book> {//指定泛型
    Book getBookById(@Param("id")Integer id);
    List<Book> getAllBooks();
    int addBook(Book book);
    int deleteBookById(@Param("id")int id);
    int updateBookById(Book book);
}
```

```java
//service层举例
@Override
public boolean addBook(Book book) {
    bookMapper.insert(book);//代替

    return bookMapper.addBook(book) == 1;
}
```

## 1.3常见注解

MybatisPlus通过扫描实体类，并基于反射获取实体类信息作为数据库表信息：

**类名驼峰转下划线作为表明**

**名为id的字段作为主键**

**变量名驼峰转下划线作为表的字段名**



**@TableName：用来指定表名**

**@Tabled：用来指定表中主键字段信息**

**@TabledField：用来指定表中的普通字段信息**

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@TableName("books")
public class Book {
    //若主键名字转化不符合规则
    @TableId(value = "id",type = IdType.AUTO) // 主键不指定auto默认为❄️算法
    private Integer id;
    private String type;
    @TableField("name")
    private String name;
    private String description;
    @TableField("is_Married")//is开头的布尔型，mp转换的时候会把is去掉，导致无法匹配成功，一定要加注解
    private Boolean isMarried;
    //与数据库关键字相同
    @TableField("`order`")//使用转义字符
    private Integer order;
    @TableField(exist = false)//数据库中不存在
    private String Address;
}
```



![截屏2024-03-31 17.20.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2017.20.39.png)

## 1.4常见配置

```yml
mybatis-plus:
  type-aliases-package: com.example.mapper #别名扫描包
  mapper-locations: "classpath*:/mapper/**/*.xml"
  configuration:
    map-underscore-to-camel-case: true #是否开启下划线和驼峰映射
    cache-enabled: false #是否开启二级缓存
  global-config:
    db-config:
    	tb-prefix: tbl_  #为表加前缀
      id-type: auto
      update-strategy: not_null #跟新策略
```

# 2.核心功能

## 2.1条件构造器

### 2.1.1QueryWrapper

#### 2.1.1.1select

```java
@SpringBootTest()
public class ControllerTest {
    @Autowired
    private BookMapper bookMapper;
    @Test
    void test() {
        QueryWrapper<Book> wrapper = new QueryWrapper<Book>()
                .select("id","name","type")
                .like("name","o")//name中有o的
                .ge("id",2); // id >= 2
        bookMapper.selectList(wrapper);
    }

}
```

#### 2.1.1.2update

```java
@Test
void testUpdate(){
    //把表中名字为jack的数据的type修改成qwe
    Book book = new Book();
    book.setType("qwe");
    QueryWrapper<Book> wrapper = new QueryWrapper<Book>().eq("name","jack");
    bookMapper.update(book,wrapper);
}
```

![截屏2024-03-31 19.32.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2019.32.12.png)

### 2.1.2UpdateWrapper

```java
@Test
void testUpdate2(){
    //把id为1，2的余额-200
    List<Long> ids = new ArrayList<>();
    ids.add(1L);
    ids.add(2L);

    UpdateWrapper<Book> wrapper = new UpdateWrapper<Book>()
            .setSql("balance = balance-200")
            .in("id",ids);
    bookMapper.update(null,wrapper);
}
```

## 2.2自定义sql

**我们可以利用mp的Wrapper来构建复杂的where条件，然后自己定义sql语句剩下的部分**

![截屏2024-03-31 19.39.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2019.39.09.png)

## 2.3service接口

![截屏2024-03-31 19.47.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2019.47.16.png)

### 2.3.1基本用法

```java
public interface IBookService extends IService<Book> {
}
```

```java
@Service
public class IBookServiceImpl extends ServiceImpl<BookMapper,Book> implements IBookService {

//ServiceImpl是IService的一个实现类

}
```

### 2.3.2开发基础业务接口

![截屏2024-03-31 20.00.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2020.00.25.png)

![截屏2024-03-31 20.08.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2020.08.04.png)

![截屏2024-03-31 20.09.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2020.09.00.png)

![截屏2024-03-31 20.09.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2020.09.43.png)

![截屏2024-03-31 20.12.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2020.12.40.png)

### 2.3.3开发复杂业务接口

![截屏2024-03-31 20.13.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2020.13.54.png)

![截屏2024-03-31 20.15.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2020.15.56.png)

![截屏2024-03-31 20.19.14](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2020.19.14.png)

![截屏2024-03-31 20.19.59](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2020.19.59.png)

### 2.3.4Lambda方法

![截屏2024-03-31 20.27.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2020.27.36.png)

![截屏2024-03-31 20.30.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2020.30.11.png)

![截屏2024-03-31 20.33.14](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2020.33.14.png)

### 2.3.5批量新增

**在yml中增加**

**rewriteBatchedStatements=true参数**![截屏2024-03-31 20.42.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2020.42.05.png)

开启后，把批量的sql语句变成一条

insert into 。。 values (),(),()....

![截屏2024-03-31 20.38.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2020.38.05.png)

# 3.拓展业务

## 3.1代码生成

![截屏2024-03-31 20.51.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2020.51.20.png)

## 3.2静态工具

**这个性能非常不错，可以模仿**

![截屏2024-03-31 21.04.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2021.04.56.png)

![截屏2024-03-31 20.58.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2020.58.12.png)

![截屏2024-03-31 21.04.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2021.04.08.png)

![截屏2024-03-31 21.10.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2021.10.49.png)

## 3.3逻辑删除

**逻辑删除就是基于代码逻辑模拟删除效果，但不会真正删除数据：**

**在表中添加一个字段标记数据是否删除**	

**当删除数据时，把标记设置为1**

**查询时只查询标记为0的数据**

![截屏2024-03-31 21.15.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2021.15.19.png)

*mp提供逻辑删除的功能，无需改变方法调用的方式，而是在底层帮我们自动修改CRUD语句，我们要做的就是在yml配置文件中配置逻辑删除的字段名称和值就可以了*

```yml
mybatis-plus:
  type-aliases-package: com.example.mapper #别名扫描包
  mapper-locations: "classpath*:/mapper/**/*.xml"
  configuration:
    map-underscore-to-camel-case: true #是否开启下划线和驼峰映射
    cache-enabled: false #是否开启二级缓存
  global-config:
    db-config:
      id-type: auto
      update-strategy: not_null #跟新策略
      #逻辑删除
      logic-delete-field: flag
      logic-delete-value: 1
      logic-not-delete-value: 0
```

![截屏2024-03-31 21.21.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2021.21.30.png)

## 3.4枚举处理器

**配置全局枚举处理器**

```yml
mybatis-plus:
	configuration:
		default-enum-type-handler: com.baomidou.mybatisplus.core.handlers.MybatisEnumTypeHandler
```

```java
@Getter
public enum UserStatus {
    NORMAL(1,"normal"),
    FROZEN(2,"freeze");

    @EnumValue
    private final int value;
   	@JsonValue // 修改springmvc数据返回，现在枚举类返回desc，默认返回枚举类名字
    private final String desc;

    UserStatus(int value, String desc) {
        this.value = value;
        this.desc = desc;
    }
}
```

![截屏2024-03-31 21.35.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2021.35.00.png)

![截屏2024-03-31 21.34.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2021.34.34.png)

## 3.5JSON处理器

![截屏2024-03-31 21.40.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2021.40.09.png)

```java
@Data
@TableName(value = "user",autoResultMap = true)
public class User {
    private Long id;
    private String name;
    @TableField(typeHandler = JacksonTypeHandler.class)
    private Book book;
}//把user表中json格式的book直接封装成Book对象
```

# 4.插件功能

## 4.1分页插件的基本使用

### 4.1.1注册核心插件

```java
@Configuration
public class MybatisConfig {
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
void testPageQuery() {
    int pageNo = 1;
    int pageSize = 5;
    Page<Book> page = Page.of(pageNo,pageSize);
    page.addOrder(new OrderItem("id",false)); // 以id排序，降序 
    Page<Book> p = bookService.page(page);
    p.getTotal();//总条数
    p.getPages();//总页数
    List<Book> books = p.getRecords();
}
```

### 4.1.2通用分页实体

![截屏2024-03-31 22.16.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2022.16.28.png)

![截屏2024-03-31 22.26.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2022.26.20.png)

![截屏2024-03-31 22.26.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-03-31%2022.26.08.png)
