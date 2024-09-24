# 初识elasticsearch

## 了解es

**用于快速在海量数据中查找**

![截屏2024-09-12 08.40.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-12%2008.40.39.png)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-12%2008.41.37.png" alt="截屏2024-09-12 08.41.37" style="zoom: 33%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-12%2008.43.01.png" alt="截屏2024-09-12 08.43.01" style="zoom:25%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-12%2008.43.53.png" alt="截屏2024-09-12 08.43.53" style="zoom:25%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-12%2008.45.39.png" alt="截屏2024-09-12 08.45.39" style="zoom:33%;" />

## 倒排索引

**正向索引**

![截屏2024-09-12 08.47.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-12%2008.47.36.png)

**正向查找 - 根据文档 去 匹配词语 发现符合要求的数据**

**倒排 - 根据词条来获取到文档id 进而获得文档**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-12%2008.51.50.png" alt="截屏2024-09-12 08.51.50" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-12%2008.54.23.png" alt="截屏2024-09-12 08.54.23" style="zoom:50%;" />

## es概念

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-12%2008.55.32.png" alt="截屏2024-09-12 08.55.32" style="zoom:33%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-12%2008.56.50.png" alt="截屏2024-09-12 08.56.50" style="zoom:50%;" />

![截屏2024-09-12 08.57.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-12%2008.57.45.png)

![截屏2024-09-12 09.00.38](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-12%2009.00.38.png)



## 安装es，kibana

文稿中有es/文档

https://www.bilibili.com/video/BV1b8411Z7w5/?p=5&spm_id_from=pageDriver&vd_source=e836c65c041589240163d699c165a729

kibana和es安装在同一个网络下，并且kibana在加载镜像的时候指定了es的端口，是的kibana在控制台中可以直接去操作es

## IK分词器

### 安装

**es默认的分词器对中文不友好，分词效果不理想**



**将ik分词器 加入到es的插件配置的挂载目录中 重启es镜像**

- **ik_smart : 最少切分，粒度大，发现四个字可以组成成语不会再对其进行细分**
- **ik_max_word:最细且分，粒度小，会出现一个字出现在多个词语中**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-14%2009.42.35.png" alt="截屏2024-09-14 09.42.35" style="zoom:25%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-14%2009.44.20.png" alt="截屏2024-09-14 09.44.20" style="zoom: 33%;" />

### 拓展和停用词典

**拓展分词字典**

![截屏2024-09-14 09.48.07](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-14%2009.48.07.png)

**停用部分词语**

![截屏2024-09-14 09.48.37](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-14%2009.48.37.png)

**ext.dic和stopword.dic都是文件名，在配置文件同级目录下，自己创建(stopword.dic已经存在)，里面去加入所需要的词汇**

# 索引库操作

## mapping映射属性

创建mapping相当于**建表**

![截屏2024-09-14 19.09.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-14%2019.09.30.png)

- **创建index索引就是建立倒排索引，可以参与es搜索**
- **一般只有text需要使用分词器，需要分词**
- **properties在字段嵌套的时候会使用到**



<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-14%2020.04.30.png" alt="截屏2024-09-14 20.04.30" style="zoom: 33%;" />

## 索引库的crud 

### 创建索引库

```json
PUT /heima
{
  "mappings": {
    "properties": {
      "info" : {
        "type": "text",
        "analyzer": "ik_smart"
      },
      "email" : {
        "type": "keyword",
        "index": false
      },
      #嵌套字段
      "name" : {
        "type": "object",
        "properties": {
          "firstname" : {
            "type" : "keyword"
          },
          "lastname" : {
            "type" : "keyword"
          }
        }
      }
    }
  }
}
```



### 查找，删除，修改

![截屏2024-09-14 19.29.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-14%2019.29.09.png)

![截屏2024-09-14 19.29.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-14%2019.29.25.png)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-14%2019.30.34.png" alt="截屏2024-09-14 19.30.34" style="zoom:50%;" />

# 文档操作

## 新增，查询，删除

 <img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-14%2019.34.58.png" alt="截屏2024-09-14 19.34.58" style="zoom:50%;" />

![截屏2024-09-14 19.36.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-14%2019.36.40.png)

![截屏2024-09-14 19.36.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-14%2019.36.50.png)

## 修改文档

如果id不存在使用put 会直接新增

![截屏2024-09-14 19.38.21](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-14%2019.38.21.png)





<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-14%2019.40.01.png" alt="截屏2024-09-14 19.40.01" style="zoom:50%;" />

# RestClient

## 操作索引库

### 初始化JavaRestClient

- **step1 - 引入es依赖**

```xml
<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>elasticsearch-rest-high-level-client</artifactId>
</dependency>
```

- **step2 - springboot默认es版本7.6.2，client下es的依赖版本(会使用springboot定义版本)需要安装的es版本保持一致，所以要去覆盖默认的es版本**

```xml
<properties>
    <java.version>1.8</java.version>
    <elasticsearch.version>7.12.1</elasticsearch.version>
</properties>
```

- **step3 - 初始化RestHighLevelClient**

```java
private RestHighLevelClient client;
@BeforeEach
void setUp() {
    client = new RestHighLevelClient(RestClient.builder(
            HttpHost.create("http://116.198.203.111:9200")
    ));
}
@AfterEach
void tearDown() throws IOException {
    this.client.close();
}
```

### 创建索引库

**DSL语句**

```json
PUT /hotel
{
  "mappings": {
    "properties": {
      "id" : {
        "type": "keyword"
      },
      "name" : {
        "type": "text",
        "analyzer": "ik_max_word"
      },
      "address" : {
        "type": "keyword",
        "index": false
      },
      "price" : {
        "type": "integer"
      },
      "score" : {
        "type": "integer"
      },
      "brand" : {
        "type": "keyword"
      },
      "city" : {
        "type": "keyword"
      },
      "starName" : {
        "type": "keyword"
      },
      "business" : {
        "type": "keyword"
      },
      #经纬度写法
      "location" : {  
        "type": "geo_point"
      },
      "pic" : {
        "type": "keyword",
        "index": false
      }
    }
  }
}

```

### **实现多个字段联合查找**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-14%2020.09.40.png" alt="截屏2024-09-14 20.09.40" style="zoom:33%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-14%2020.42.40.png" alt="截屏2024-09-14 20.42.40" style="zoom: 33%;" />

```java
private RestHighLevelClient client;
@BeforeEach
void setUp() {
    client = new RestHighLevelClient(RestClient.builder(
            HttpHost.create("http://116.198.203.111:9200")
    ));
}
@AfterEach
void tearDown() throws IOException {
    this.client.close();
}
@Test
void creatHotelIndex() throws IOException {
    //创建request对象
    CreateIndexRequest request = new CreateIndexRequest("hotel");
    //准备请求的参数 - dsl语句
    request.source(HotelConstants.MAPPING_TEMPLATE , XContentType.JSON);
    //发送请求
    client.indices().create(request, RequestOptions.DEFAULT);
}
```

```java
public class HotelConstants {
    public static final String MAPPING_TEMPLATE = "{
      "mappings": {
        "properties": {
          "id" : {
            "type": "keyword"
          },
          "name" : {
            "type": "text",
            "analyzer": "ik_max_word",
            "copy_to": "all"
          },
          "address" : {
            "type": "keyword",
            "index": false,
            "copy_to": "all"
          },
          "price" : {
            "type": "integer"
          },
          "score" : {
            "type": "integer"
          },
          "brand" : {
            "type": "keyword"
          },
          "city" : {
            "type": "keyword"
          },
          "starName" : {
            "type": "keyword"
          },
          "business" : {
            "type": "keyword",
            "copy_to": "all"
          },
          "location" : {  
            "type": "geo_point"
          },
          "pic" : {
            "type": "keyword",
            "index": false
          },
          "all" : {
            "type": "text",
            "analyzer": "ik_max_word"
          }
        }
      }
    }";
}
```

### 删除索引库

```java
@Test
void testDeleteIndex() throws IOException {
    //创建delete的request
    DeleteIndexRequest request = new DeleteIndexRequest("hotel");
    //发送请求
    client.indices().delete(request, RequestOptions.DEFAULT);
}
```

### 判断索引库是否存在

```java
    @Test
    void testExistsIndex() throws IOException {
        GetIndexRequest request = new GetIndexRequest("hotel");
        boolean exists = client.indices().exists(request, RequestOptions.DEFAULT);
        System.out.println(exists);
    }
}
```

## 操作文档

### 新增文档

 

```java
@Autowired
private IHotelService hotelService;
private RestHighLevelClient client;
@BeforeEach
void setUp() {
    client = new RestHighLevelClient(RestClient.builder(
            HttpHost.create("http://116.198.203.111:9200")
    ));
}
@AfterEach
void tearDown() throws IOException {
    this.client.close();
}
@Test
void testAddDocument() throws IOException {
    //模拟根据id查找到酒店数据
    Hotel hotel = hotelService.getById(61083L);
    //将数据库类型的酒店封装成适合doc数据结构
    HotelDoc hotelDoc = new HotelDoc(hotel);
    //准备request对象
    IndexRequest request = new IndexRequest("hotel").id(hotelDoc.getId().toString());
    //准备json文档
    request.source(JSON.toJSONString(hotelDoc), XContentType.JSON);
    //发送请求
    client.index(request , RequestOptions.DEFAULT);
}
```

**在实体类的封装中，数据库的实体类和es的实体类可能存在差异 - 对于经纬度，数据库自己使用两个变量存储，而es中有特别的数据结构**

**解决方案是构建适合es的pojo类，并且在构建方法中由数据库的pojo对象转换而来**

```java
public HotelDoc(Hotel hotel) {
    this.id = hotel.getId();
    this.name = hotel.getName();
    this.address = hotel.getAddress();
    this.price = hotel.getPrice();
    this.score = hotel.getScore();
    this.brand = hotel.getBrand();
    this.city = hotel.getCity();
    this.starName = hotel.getStarName();
    this.business = hotel.getBusiness();
    this.location = hotel.getLatitude() + ", " + hotel.getLongitude();
    this.pic = hotel.getPic();
}
```

**添加数据的过程为从数据库中找到实体对象，看情况对对象作出转换，使用json工具类将对象转为json最后封装发送请求**

### 查询文档

```java
@Test
void testGetDocument() throws IOException {
    //准备request
    GetRequest request = new GetRequest("hotel", "61083");
    //发送请求得到响应
    GetResponse documentFields = client.get(request, RequestOptions.DEFAULT);
    //解析结果
    String json = documentFields.getSourceAsString();
    HotelDoc hotelDoc = JSON.parseObject(json, HotelDoc.class);
    System.out.println(hotelDoc);
}
```

### 删除文档

```java
@Test
void testDeleteDocument() throws IOException {
    //准备request
    DeleteRequest request = new DeleteRequest("hotel", "61083");
    //发送请求
    client.delete(request, RequestOptions.DEFAULT);
}
```

### 修改文档

**全量更新相当于写入一个新的doc 就是新增的代码 实现覆盖**



**局部跟新 ：**

```java
@Test 
void testUpdateDocument() throws IOException {
    //准备request
    UpdateRequest hotel = new UpdateRequest("hotel", "61083");
    //准备请求参数
    hotel.doc(
            "price" , "952",
            "starName" , "9"
    );
    //发送请求
    client.update(hotel, RequestOptions.DEFAULT);
}
```

### 批量导入文档

```java
@Test
void testBulkRequest() throws IOException {
    //批量从数据库中查找数据
    List<Hotel> list = hotelService.list();
    //创建request
    BulkRequest request = new BulkRequest();
    //准备参数，添加多个request
    list.forEach(hotel -> {
        //对象转换
        HotelDoc hotelDoc = new HotelDoc(hotel);
        //加入request
        request.add(new IndexRequest("hotel")
                .id(hotelDoc.getId().toString())
                .source(JSON.toJSONString(hotelDoc),XContentType.JSON));
    });

    //发送请求
    client.bulk(request , RequestOptions.DEFAULT);
}
```

# DSL查询文档

## DSL查询分类

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2016.32.42.png" alt="截屏2024-09-18 16.32.42" style="zoom:50%;" />

​	<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2016.33.41.png" alt="截屏2024-09-18 16.33.41" style="zoom:33%;" />



## 全文检索查询

```json
#match查询
#"根据哪个字段的倒排索引查找" ："用于匹配的词语"
GET /hotel/_search
{
  "query": {
    "match": {
      "all": "外滩"
    }
  }
}
#multi_match查询
GET /hotel/_search
{
  "query": {
    "multi_match": {
      "query": "外滩",
      "fields": ["brand" , "name"]
    }
  }
}
```

**普通的match查询只能查询一个关键字(一般使用聚合的all关键字)**

**根据匹配词语分词结果去做倒排索引的匹配**

**分词后匹配度越高的词条结果会在更前面**

**多个字段共同查找的all聚合索引 效率会更高**

## 精准查询

- **term - 根据词条精确值查询**
- **range - 根据值的范围查询**

```json
#term查询
#根据字段精确查询
GET /hotel/_search
{
  "query": {
    "term": {
      "city": {
        "value": "上海"
      }
    }
  }
}

#range范围查询
#gte >=   gt >
GET /hotel/_search
{
  "query": {
    "range": {
      "price": {
        "gte": 100,
        "lte": 200
      }
    }
  }
}
```

## 地理坐标查询

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2016.56.49.png" alt="截屏2024-09-18 16.56.49" style="zoom: 33%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2016.58.19.png" alt="截屏2024-09-18 16.58.19" style="zoom:33%;" />

**FIELD位置为 geo_point(经纬度点)数据类型的字段**

## 组合查询

### 相关性算分

![截屏2024-09-18 17.09.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2017.09.34.png)



<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2017.10.27.png" alt="截屏2024-09-18 17.10.27" style="zoom:25%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2017.10.10.png" alt="截屏2024-09-18 17.10.10" style="zoom:50%;" />

### FunctionScoreQuery

**修改算分**

![截屏2024-09-18 17.14.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2017.14.01.png)

 <img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2017.17.37.png" alt="截屏2024-09-18 17.17.37" style="zoom:50%;" />

### BooleanQuery

**多个子查询的组合**

- **must - 必须匹配每个子查询 - 与**
- **should - 选择性匹配子查询 - 或**
- **must_not - 必须不匹配，不参与算分 - 非**
- **filter - 必须匹配，不参与算分**



<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2017.26.10.png" alt="截屏2024-09-18 17.26.10" style="zoom: 33%;" />

**把不必要的条件放到must_not或者filter**

**减少需要算分的字段 增加搜索的效率**

# 搜索结果处理

## 排序

- **放弃打分**

- **keyword 使用字母字典顺序排序(使用少)**

- **地理坐标排序 是指定一个位置和某个geo_point类型的索引字段的距离的排序**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2017.32.02.png" alt="截屏2024-09-18 17.32.02" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2017.34.40.png" alt="截屏2024-09-18 17.34.40" style="zoom: 33%;" />

## 分页



<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2019.32.54.png" alt="截屏2024-09-18 19.32.54" style="zoom:33%;" />

**es 分页方式**

![截屏2024-09-18 19.38.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2019.38.01.png)

![截屏2024-09-18 19.37.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2019.37.43.png)

​	<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2019.40.21.png" alt="截屏2024-09-18 19.40.21" style="zoom:50%;" />

- **search after 每次从上一次排序位置开始 无法往前获取数据**
- **scroll 对内存消耗大而且无法跟新快照(在数据跟新的时候)**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2019.45.02.png" alt="截屏2024-09-18 19.45.02" style="zoom: 33%;" />

## 高亮

**把搜索结果中的关键字突出显示出来**

- 将搜索结果中的关键字用标签标记出来
- 在页面中给标签添加css样式

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2019.49.16.png" alt="截屏2024-09-18 19.49.16" style="zoom: 33%;" />

可以在fields下的field标签下添加其他的标签默认是em

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2019.51.15.png" alt="截屏2024-09-18 19.51.15" style="zoom:50%;" />

# RestClient查询文档

## 快速入门

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2019.55.41.png" alt="截屏2024-09-18 19.55.41" style="zoom:50%;" />



 **解析结果**

![截屏2024-09-18 20.02.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2020.02.06.png)





```java
@Test
void testMatchAll() throws IOException {
    SearchRequest request = new SearchRequest("hotel");
    request.source().query(QueryBuilders.matchAllQuery());
    SearchResponse response = client.search(request, RequestOptions.DEFAULT);
    //  解析response
    //总的数据体
    SearchHits searchHits = response.getHits();
    //获取总条数
    long value = searchHits.getTotalHits().value;

    //真正的数据
    SearchHit[] hits = searchHits.getHits();
    for (SearchHit hit : hits) {
        String json = hit.getSourceAsString();
        //反序列化
        HotelDoc hotelDoc = JSON.parseObject(json, HotelDoc.class);
    }
    
}
```



## 查询

**match和multi_match**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2020.12.02.png" alt="截屏2024-09-18 20.12.02" style="zoom:50%;" />

**精确查询**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2020.13.58.png" alt="截屏2024-09-18 20.13.58" style="zoom: 33%;" />

**复合查询**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2020.14.52.png" alt="截屏2024-09-18 20.14.52" style="zoom:33%;" />



## 排序，分页，高亮

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2020.18.51.png" alt="截屏2024-09-18 20.18.51" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2020.22.01.png" alt="截屏2024-09-18 20.22.01" style="zoom:50%;" />



**处理高亮结果**

![截屏2024-09-18 20.23.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-18%2020.23.53.png)



## 地理坐标排序

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2009.34.15.png" alt="截屏2024-09-19 09.34.15" style="zoom:50%;" />

## function score

![截屏2024-09-19 09.54.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2009.54.34.png)

# 黑马旅游案例

## 搜索和分页

**封装获取参数和返回对象**

```java
@Autowired
private RestHighLevelClient client;
@Override
public PageResult search(RequestParams params) {
    //准备request
    SearchRequest request = new SearchRequest("hotel");


    //关键字 分词匹配
    String key = params.getKey();
    if (key == null || key.isEmpty()) {
        request.source().query(QueryBuilders.matchAllQuery());
    }else{
        request.source().query(QueryBuilders.matchQuery("all" , key));
    }

    //分页
    int page = params.getPage();
    int size = params.getSize();
    request.source().from((page - 1) * size).size(size);

    //发送请求
    SearchResponse response = null;
    try {
        response = client.search(request, RequestOptions.DEFAULT);
    } catch (IOException e) {
        throw new RuntimeException(e);
    }
    return handleResponse(response);
}

private PageResult handleResponse(SearchResponse response) {
    SearchHits searchHit = response.getHits();
    long value = searchHit.getTotalHits().value; //总条数

    SearchHit[] hits = searchHit.getHits();
    List<HotelDoc> result = new ArrayList<>();
    for (SearchHit hit : hits) {
        String json = hit.getSourceAsString();
       result.add(JSON.parseObject(json, HotelDoc.class));
    }
    return new PageResult(result , value);
}
```





## 酒店结果过滤

对于搜索框和固定栏过滤 (分词搜索和精准查询的复合) - 都封装**booleanQuery**

```java
public PageResult search(RequestParams params) {
    //准备request
    SearchRequest request = new SearchRequest("hotel");

    //要复合分词搜索和精确搜索，直接封装booleanQuery
    BoolQueryBuilder boolQuery = QueryBuilders.boolQuery();
    //关键字分词搜索
    String key = params.getKey();
    if (key == null || key.isEmpty()) {
        boolQuery.must(QueryBuilders.matchAllQuery());
    }else{
        boolQuery.must(QueryBuilders.matchQuery("all" , key));
    }
    //条件过滤
    if (params.getCity() != null && !params.getCity().isEmpty()) {
        boolQuery.filter(QueryBuilders.termQuery("city", params.getCity()));
    }
    if (params.getBrand() != null && !params.getBrand().isEmpty()) {
        boolQuery.filter(QueryBuilders.termQuery("brand", params.getBrand()));
    }
    if (params.getStarName() != null && !params.getStarName().isEmpty()) {
        boolQuery.filter(QueryBuilders.termQuery("starName", params.getStarName()));
    }
    //价格范围过滤
    if (params.getMinPrice() != null && params.getMaxPrice() != null) {
        boolQuery.filter(QueryBuilders.rangeQuery("price").gte(params.getMinPrice()).lte(params.getMaxPrice()));
    }
    request.source().query(boolQuery);
    
    //分页
    int page = params.getPage();
    int size = params.getSize();
    request.source().from((page - 1) * size).size(size);

    //发送请求
    SearchResponse response = null;
    try {
        response = client.search(request, RequestOptions.DEFAULT);
    } catch (IOException e) {
        throw new RuntimeException(e);
    }
    return handleResponse(response);
}

/**
 *
 * @param response 结果体
 * @return 结果处理封装对象
 */
private PageResult handleResponse(SearchResponse response) {
    SearchHits searchHit = response.getHits();
    long value = searchHit.getTotalHits().value; //总条数

    SearchHit[] hits = searchHit.getHits();
    List<HotelDoc> result = new ArrayList<>();
    for (SearchHit hit : hits) {
        String json = hit.getSourceAsString();
        result.add(JSON.parseObject(json, HotelDoc.class));
    }
    return new PageResult(result , value);
}
```

## 周边酒店

```java
//排序(地理坐标)
String location = params.getLocation();
if (location != null && !location.isEmpty()) {
    request.source().sort(SortBuilders.geoDistanceSort("location" , new GeoPoint(location))
            .order(SortOrder.ASC)
            .unit(DistanceUnit.KILOMETERS));
}
```

```java
//排序字段依据会在sortValues中保留，需要解析并返回前端
private PageResult handleResponse(SearchResponse response) {
    SearchHits searchHit = response.getHits();
    long value = searchHit.getTotalHits().value; //总条数

    SearchHit[] hits = searchHit.getHits();
    List<HotelDoc> result = new ArrayList<>();
    for (SearchHit hit : hits) {
        String json = hit.getSourceAsString();
        HotelDoc hotelDoc = JSON.parseObject(json, HotelDoc.class);
        Object[] sortValues = hit.getSortValues();
        if (sortValues.length > 0) {
            hotelDoc.setDistance(sortValues[0]);
        }
        result.add(hotelDoc);
    }
    return new PageResult(result , value);
}
```

## 酒店竞价排名

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2009.46.42.png" alt="截屏2024-09-19 09.46.42" style="zoom: 33%;" />

 

```java
//改变算分
FunctionScoreQueryBuilder ad = QueryBuilders.functionScoreQuery(
        //原始查询
        boolQuery,
        //function score 数组
        new FunctionScoreQueryBuilder.FilterFunctionBuilder[]{
                //其中一个function score
                new FunctionScoreQueryBuilder.FilterFunctionBuilder(
                        //过滤条件
                        QueryBuilders.termQuery("isAD", true),
                        //算分函数
                        ScoreFunctionBuilders.weightFactorFunction(10))
        });
request.source().query(ad);
```

# 数据聚合

## 聚合种类

**对文档数据的统计，分析，计算**

![截屏2024-09-19 15.36.38](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2015.36.38.png)



**参与聚合 - keyword 数值 bool date 不能是分词的text**

## DSL实现聚合

### Bucket聚合

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2015.41.48.png" alt="截屏2024-09-19 15.41.48" style="zoom:33%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2015.43.18.png" alt="截屏2024-09-19 15.43.18" style="zoom: 33%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2015.44.00.png" alt="截屏2024-09-19 15.44.00" style="zoom:33%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2015.45.09.png" alt="截屏2024-09-19 15.45.09" style="zoom:33%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2015.46.30.png" alt="截屏2024-09-19 15.46.30" style="zoom:25%;" />

### Metric聚合

 <img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2015.49.02.png" alt="截屏2024-09-19 15.49.02" style="zoom: 33%;" />



**一个桶中按照度量聚合后的结果来做排序 需要在定义桶的terms中去指定排序规则**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2015.51.04.png" alt="截屏2024-09-19 15.51.04" style="zoom:33%;" />



## RestAPI实现聚合

**请求**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2015.54.00.png" alt="截屏2024-09-19 15.54.00" style="zoom: 33%;" />

```java
@Test
void testAggregation() throws IOException {
    //准备 request
    SearchRequest request = new SearchRequest("hotel");
    //准备Dsl
    //设置size 去除文档数据
    request.source().size(0);
    //聚合
    request.source().aggregation(AggregationBuilders
            .terms("brandAgg")
            .field("brand")
            .size(20));
    //发出请求
    SearchResponse response = client.search(request, RequestOptions.DEFAULT);
    //解析结果
    System.out.println(response);
}
```

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2016.02.24.png" alt="截屏2024-09-19 16.02.24" style="zoom:33%;" />

```java
//解析结果
Aggregations aggregations = response.getAggregations();
//根据聚合名称获得聚合结果
Terms brandTerms = aggregations.get("brandAgg");
//获取桶
List<? extends Terms.Bucket> buckets = brandTerms.getBuckets();
for (Terms.Bucket bucket : buckets) {
    //获取key
    String key = bucket.getKeyAsString();
    System.out.println(key);
}
```

## 多条件聚合

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2016.17.57.png" alt="截屏2024-09-19 16.17.57" style="zoom:33%;" />

```java
@Override
public Map<String, List<String>> filters() throws IOException {
    //准备 request
    SearchRequest request = new SearchRequest("hotel");
    //准备Dsl
    //设置size 去除文档数据
    request.source().size(0);
    //聚合
    request.source().aggregation(AggregationBuilders
            .terms("brandAgg")
            .field("brand")
            .size(100));
    request.source().aggregation(AggregationBuilders
            .terms("cityAgg")
            .field("city")
            .size(100));
    request.source().aggregation(AggregationBuilders
            .terms("starAgg")
            .field("starName")
            .size(100));


    //发出请求
    SearchResponse response = client.search(request, RequestOptions.DEFAULT);
    //解析结果
    Map<String , List<String>> res = new HashMap<>();
    Aggregations aggregations = response.getAggregations();
    res.put("城市" , getAggByName(aggregations , "cityAgg"));
    res.put("星级" , getAggByName(aggregations , "starAgg"));
    res.put("品牌" , getAggByName(aggregations , "brandAgg"));
    return res;
}

private List<String> getAggByName (Aggregations aggregations , String name){
    //根据聚合名称获得聚合结果
    Terms brandTerms = aggregations.get(name);
    //获取桶
    List<? extends Terms.Bucket> buckets = brandTerms.getBuckets();
    List<String> temp = new ArrayList<>();
    for (Terms.Bucket bucket : buckets) {
        //获取key
        String key = bucket.getKeyAsString();
        temp.add(key);
    }
    return temp;
}
```

## 带过滤条件的聚合

**当我搜索一个关键字，下方固定栏中的内容应该根据关键字发生变化**

![截屏2024-09-19 16.40.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2016.40.17.png)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2016.41.35.png" alt="截屏2024-09-19 16.41.35" style="zoom:33%;" />

```java
@Override
public Map<String, List<String>> filters(RequestParams params) {
    //准备 request
    SearchRequest request = new SearchRequest("hotel");
    //准备Dsl
    //query - 限定查询信息
    //要复合分词搜索和精确搜索，直接封装booleanQuery
    BoolQueryBuilder boolQuery = QueryBuilders.boolQuery();
    //关键字分词搜索
    String key = params.getKey();
    if (key == null || key.isEmpty()) {
        boolQuery.must(QueryBuilders.matchAllQuery());
    }else{
        boolQuery.must(QueryBuilders.matchQuery("all" , key));
    }
    //条件过滤
    if (params.getCity() != null && !params.getCity().isEmpty()) {
        boolQuery.filter(QueryBuilders.termQuery("city", params.getCity()));
    }
    if (params.getBrand() != null && !params.getBrand().isEmpty()) {
        boolQuery.filter(QueryBuilders.termQuery("brand", params.getBrand()));
    }
    if (params.getStarName() != null && !params.getStarName().isEmpty()) {
        boolQuery.filter(QueryBuilders.termQuery("starName", params.getStarName()));
    }
    //价格范围过滤
    if (params.getMinPrice() != null && params.getMaxPrice() != null) {
        boolQuery.filter(QueryBuilders.rangeQuery("price").gte(params.getMinPrice()).lte(params.getMaxPrice()));
    }

    //改变算分
    FunctionScoreQueryBuilder ad = QueryBuilders.functionScoreQuery(
            //原始查询
            boolQuery,
            //function score 数组
            new FunctionScoreQueryBuilder.FilterFunctionBuilder[]{
                    //其中一个function score
                    new FunctionScoreQueryBuilder.FilterFunctionBuilder(
                            //过滤条件
                            QueryBuilders.termQuery("isAD", true),
                            //算分函数
                            ScoreFunctionBuilders.weightFactorFunction(10))
            });
    request.source().query(ad);

    //分页
    int page = params.getPage();
    int size = params.getSize();
    request.source().from((page - 1) * size).size(size);

    //排序(地理坐标)
    String location = params.getLocation();
    if (location != null && !location.isEmpty()) {
        request.source().sort(SortBuilders.geoDistanceSort("location" , new GeoPoint(location))
                .order(SortOrder.ASC)
                .unit(DistanceUnit.KILOMETERS));
    }

    //设置size 去除文档数据
    request.source().size(0);
    //聚合
    request.source().aggregation(AggregationBuilders
            .terms("brandAgg")
            .field("brand")
            .size(100));
    request.source().aggregation(AggregationBuilders
            .terms("cityAgg")
            .field("city")
            .size(100));
    request.source().aggregation(AggregationBuilders
            .terms("starAgg")
            .field("starName")
            .size(100));


    //发出请求
    SearchResponse response = null;
    try {
        response = client.search(request, RequestOptions.DEFAULT);
    } catch (IOException e) {
        throw new RuntimeException(e);
    }
    //解析结果
    Map<String , List<String>> res = new HashMap<>();
    Aggregations aggregations = response.getAggregations();
    res.put("城市" , getAggByName(aggregations , "cityAgg"));
    res.put("星级" , getAggByName(aggregations , "starAgg"));
    res.put("品牌" , getAggByName(aggregations , "brandAgg"));
    return res;
}

private List<String> getAggByName (Aggregations aggregations , String name){
    //根据聚合名称获得聚合结果
    Terms brandTerms = aggregations.get(name);
    //获取桶
    List<? extends Terms.Bucket> buckets = brandTerms.getBuckets();
    List<String> temp = new ArrayList<>();
    for (Terms.Bucket bucket : buckets) {
        //获取key
        String key = bucket.getKeyAsString();
        temp.add(key);
    }
    return temp;
}
```

**数据过滤的代码和搜索是完全一样的，可以抽取出来**

**其实就是在过滤出的数据上去做聚合**

# 自动补全

## 拼音分词器

https://github.com/medcl/elasticsearch-analysis-pinyin

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2016.57.20.png" alt="截屏2024-09-19 16.57.20" style="zoom:33%;" />

**document - es - py**

## 自定义分词器

<u>原始的拼音分词器存在一点问题</u>

- **只能拆分单个字，不能分词**
- **只保留拼音，没有了汉字**

![截屏2024-09-19 19.08.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2019.08.56.png)

**在setting中自定义分词器**

```json
PUT /test
{
  "settings": {
    "analysis": {
      "analyzer": {
        "my_analyzer" : { 
          "tokenizer" : "ik_max_word",
          "filter" : "pinyin"
        }
      }
    }
  }
}
```

```json
PUT /test
{
  "settings": {
    "analysis": {
      "analyzer": {
        "my_analyzer" : { 
          "tokenizer" : "ik_max_word",
          "filter" : "py"
        }
      },
      "filter": {
        "py" : {
          "type" : "pinyin",
          "keep_full_pinyin": false,
          "keep_joined_full_pinyin": true,
          "keep_original": true,
          "limit_ first_letter_length": 16,
          "remove_duplicated_term": true,
          "none_chinese_pinyin_tokenize": false
        }
      }
    }
  }
}
```

![截屏2024-09-19 19.19.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2019.19.54.png)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2019.20.24.png" alt="截屏2024-09-19 19.20.24" style="zoom:33%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2019.21.45.png" alt="截屏2024-09-19 19.21.45" style="zoom:33%;" />

## 自动补全查询

设置completion来存储多个字符的数组

![截屏2024-09-19 19.24.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2019.24.20.png)



<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2019.25.26.png" alt="截屏2024-09-19 19.25.26" style="zoom:33%;" />

![截屏2024-09-19 19.27.26](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2019.27.26.png)

## 实现酒店搜索框自动补全

### 修改索引库数据结构

- **定义两个分词器，一个用于分词拼音，一个用于keyword拼音(不分词)**
- **name和all设置普通analyzer(拼音分词)和search_analyzer(精准查找避免同音字用拼音匹配 找到过多结果) --- 实现可以用拼音查找**
- **设置suggestion 用于自动补全 (keyword拼音分词器)**

```java
// 酒店数据索引库
PUT /hotel
{
  "settings": {
    "analysis": {
      "analyzer": {
        "text_anlyzer": {
          "tokenizer": "ik_max_word",
          "filter": "py"
        },
        "completion_analyzer": {
          "tokenizer": "keyword",
          "filter": "py"
        }
      },
      "filter": {
        "py": {
          "type": "pinyin",
          "keep_full_pinyin": false,
          "keep_joined_full_pinyin": true,
          "keep_original": true,
          "limit_first_letter_length": 16,
          "remove_duplicated_term": true,
          "none_chinese_pinyin_tokenize": false
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "id":{
        "type": "keyword"
      },
      "name":{
        "type": "text",
        "analyzer": "text_anlyzer",
        "search_analyzer": "ik_smart",
        "copy_to": "all"
      },
      "address":{
        "type": "keyword",
        "index": false
      },
      "price":{
        "type": "integer"
      },
      "score":{
        "type": "integer"
      },
      "brand":{
        "type": "keyword",
        "copy_to": "all"
      },
      "city":{
        "type": "keyword"
      },
      "starName":{
        "type": "keyword"
      },
      "business":{
        "type": "keyword",
        "copy_to": "all"
      },
      "location":{
        "type": "geo_point"
      },
      "pic":{
        "type": "keyword",
        "index": false
      },
      "all":{
        "type": "text",
        "analyzer": "text_anlyzer",
        "search_analyzer": "ik_smart"
      },
      "suggestion":{
          "type": "completion",
          "analyzer": "completion_analyzer"
      }
    }
  }
}
```

**在doc实体类中增加suggestion字段，在构造函数中把需要匹配的字符加入到集合中去**

```java
private List<String> suggestion;

public HotelDoc(Hotel hotel) {
    this.id = hotel.getId();
    this.name = hotel.getName();
    this.address = hotel.getAddress();
    this.price = hotel.getPrice();
    this.score = hotel.getScore();
    this.brand = hotel.getBrand();
    this.city = hotel.getCity();
    this.starName = hotel.getStarName();
    this.business = hotel.getBusiness();
    this.location = hotel.getLatitude() + ", " + hotel.getLongitude();
    this.pic = hotel.getPic();
   	if (this.business.contains("/")){
            this.suggestion = new ArrayList<>();
            this.suggestion.add(this.name);
            String[] split = business.split("/");
            Collections.addAll(this.suggestion , split);
        }else{
            this.suggestion = Arrays.asList(this.name , this.business);
        }
}
```

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2019.57.16.png" alt="截屏2024-09-19 19.57.16" style="zoom: 67%;" />

### RestAPI实现自动补全

![截屏2024-09-19 19.59.21](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2019.59.21.png)

```java
@Test
void testSuggestion() throws IOException {
    SearchRequest request = new SearchRequest("hotel");
    request.source().suggest(new SuggestBuilder().addSuggestion(
            "suggestions" ,
            SuggestBuilders.completionSuggestion("suggestion")
                    .prefix("h")
                    .skipDuplicates(true)
                    .size(10)
    ));

    SearchResponse response = client.search(request, RequestOptions.DEFAULT);
    System.out.println(response);
}
```

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2020.05.04.png" alt="截屏2024-09-19 20.05.04" style="zoom:33%;" />

```java
@Test
void testSuggestion() throws IOException {
    SearchRequest request = new SearchRequest("hotel");
    request.source().suggest(new SuggestBuilder().addSuggestion(
            "suggestions" ,
            SuggestBuilders.completionSuggestion("suggestion")
                    .prefix("h")
                    .skipDuplicates(true)
                    .size(10)
    ));

    SearchResponse response = client.search(request, RequestOptions.DEFAULT);
    //解析响应结果
    Suggest suggest = response.getSuggest();
    CompletionSuggestion suggestions = suggest.getSuggestion("suggestions");
    //获取options
    for (CompletionSuggestion.Entry.Option option : suggestions.getOptions()) {
        String string = option.getText().toString();
        System.out.println(string);
    }

}
```

### 实现搜索框自动补全

```java
@Override
public List<String> getSuggestions(String prefix) {
    SearchRequest request = new SearchRequest("hotel");
    request.source().suggest(new SuggestBuilder().addSuggestion(
            "suggestions" ,
            SuggestBuilders.completionSuggestion("suggestion")
                    .prefix(prefix)
                    .skipDuplicates(true)
                    .size(10)
    ));

    SearchResponse response = null;
    try {
        response = client.search(request, RequestOptions.DEFAULT);
    } catch (IOException e) {
        throw new RuntimeException(e);
    }
    //解析响应结果
    Suggest suggest = response.getSuggest();
    CompletionSuggestion suggestions = suggest.getSuggestion("suggestions");
    List<String> res = new ArrayList<>();
    //获取options
    for (CompletionSuggestion.Entry.Option option : suggestions.getOptions()) {
        String string = option.getText().toString();
        res.add(string);
    }
    return res;
}
```



# 数据同步

## 思路分析

**es数据来自mysql数据库，当mysql数据发生变化，es必须跟着变 ------- 数据同步**

![截屏2024-09-19 20.37.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2020.37.45.png)



![截屏2024-09-19 20.41.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2020.41.49.png)

![截屏2024-09-19 20.44.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2020.44.23.png)

![截屏2024-09-19 20.51.07](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2020.51.07.png)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2020.53.09.png" alt="截屏2024-09-19 20.53.09" style="zoom: 50%;" />

## 实现es和数据库数据同步

  <img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-19%2020.56.35.png" alt="截屏2024-09-19 20.56.35" style="zoom:50%;" />

### 声明队列交换机

![截屏2024-09-20 09.34.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-20%2009.34.17.png)

**在消费者一侧建立**

**加入amqp依赖**

```xml
<!--amqp-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

```yml
spring:
  rabbitmq:
    host: 116.198.203.111
    port: 5672
    username: hmall
    password: 123
    virtual-host: /hmall
```

```java
public class MqConstants {
    /**
     * 交换机
     */
    public final static String HOTEL_EXCHANGE = "hotel.topic";
    /**
     * 监听新增和修改的队列
     */
    public final static String HOTEL_INSERT_QUEUE = "hotel.insert.queue";
    /**
     * 监听删除的队列
     */
    public final static String HOTEL_DELETE_QUEUE = "hotel.delete.queue";
    /**
     * 新增或者修改的routingKey
     */
    public final static String HOTEL_INSERT_KEY = "hotel.insert";
    /**
     * 删除的routingKey
     */
    public final static String HOTEL_DELETE_KEY = "hotel.delete";
    
}
```

```java
@Configuration
public class MqConfig {

    @Bean
    public TopicExchange topicExchange () {
        return new TopicExchange(HOTEL_EXCHANGE , true , false);
    }

    @Bean
    public Queue insertQueue() {
        return new Queue(HOTEL_INSERT_QUEUE , true);
    }

    @Bean
    public Queue deleteQueue() {
        return new Queue(HOTEL_DELETE_QUEUE , true);
    }

    @Bean
    public Binding insertQueueBinding() {
        return BindingBuilder.bind(insertQueue()).to(topicExchange()).with(HOTEL_INSERT_KEY);
    }

    @Bean
    public Binding deleteQueueBinding() {
        return BindingBuilder.bind(deleteQueue()).to(topicExchange()).with(HOTEL_DELETE_KEY);
    }

}
```

### 发送mq消息

**在生产者一侧 加入amqp依赖，在配置文件中配置rabbitMQ**

在增改和删的逻辑中加入rabbitTamplate依赖 在完成请求后，增加发送消息的代码

```java
@Autowired
private RabbitTemplate rabbitTemplate;
@PostMapping
public void saveHotel(@RequestBody Hotel hotel){
    hotelService.save(hotel);
    //避免把整个hotel都传过去，rabbitMQ基于内存存储的话，内存冗余严重
    rabbitTemplate.convertAndSend(MqConstants.HOTEL_EXCHANGE , MqConstants.HOTEL_INSERT_KEY , hotel.getId());
}

@PutMapping()
public void updateById(@RequestBody Hotel hotel){
    if (hotel.getId() == null) {
        throw new InvalidParameterException("id不能为空");
    }
    hotelService.updateById(hotel);
    rabbitTemplate.convertAndSend(MqConstants.HOTEL_EXCHANGE , MqConstants.HOTEL_INSERT_KEY , hotel.getId());
}

@DeleteMapping("/{id}")
public void deleteById(@PathVariable("id") Long id) {
    hotelService.removeById(id);
    rabbitTemplate.convertAndSend(MqConstants.HOTEL_EXCHANGE , MqConstants.HOTEL_DELETE_KEY , id);
}
```

### 监听mq消息

**在消费者中监听消息**

```java
@Component
public class HotelListener {
    @Autowired
    private IHotelService hotelService;
    /**
     * 监听酒店新增或者修改的业务
     * @param id 酒店id
     */
    @RabbitListener(queues = MqConstants.HOTEL_INSERT_QUEUE)
    public void listenHotelInsertOrUpdate(Long id) {
        hotelService.insertById(id);
    }

    /**
     * 监听酒店删除的业务
     * @param id 酒店id
     */
    @RabbitListener(queues = MqConstants.HOTEL_DELETE_QUEUE)
    public void listenHotelDelete(Long id) {
        hotelService.deleteById(id);
    }

}
```

```java
@Override
public void deleteById(Long id) {
    DeleteRequest deleteRequest = new DeleteRequest("hotel" , id.toString());
    try {
        client.delete(deleteRequest , RequestOptions.DEFAULT);
    } catch (IOException e) {
        throw new RuntimeException(e);
    }
}

@Override
public void insertById(Long id) {
    // 在数据库中查找修改后的酒店数据
    Hotel hotel = getById(id);
    //类型封装
    HotelDoc hotelDoc = new HotelDoc(hotel);
    //准备request
    IndexRequest request = new IndexRequest("hotel").id(hotelDoc.getId().toString());
    request.source(JSON.toJSONString(hotelDoc) , XContentType.JSON);
    try {
        client.index(request , RequestOptions.DEFAULT);
    } catch (IOException e) {
        throw new RuntimeException(e);
    }
}
```

### 测试同步功能

**ez**

# 集群

![](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-24%2019.15.44-20240924192051972-20240924192056156.png)

## 搭建es集群

**首先编写一个docker-compose文件，内容如下：**

```sh
version: '2.2'
services:
  es01:
    image: elasticsearch:7.12.1
    container_name: es01
    environment:
      - node.name=es01
      - cluster.name=es-docker-cluster
      - discovery.seed_hosts=es02,es03
      - cluster.initial_master_nodes=es01,es02,es03
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    volumes:
      - data01:/usr/share/elasticsearch/data
    ports:
      - 9200:9200
    networks:
      - elastic
  es02:
    image: elasticsearch:7.12.1
    container_name: es02
    environment:
      - node.name=es02
      - cluster.name=es-docker-cluster
      - discovery.seed_hosts=es01,es03
      - cluster.initial_master_nodes=es01,es02,es03
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    volumes:
      - data02:/usr/share/elasticsearch/data
    ports:
      - 9201:9200
    networks:
      - elastic
  es03:
    image: elasticsearch:7.12.1
    container_name: es03
    environment:
      - node.name=es03
      - cluster.name=es-docker-cluster
      - discovery.seed_hosts=es01,es02
      - cluster.initial_master_nodes=es01,es02,es03
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    volumes:
      - data03:/usr/share/elasticsearch/data
    networks:
      - elastic
    ports:
      - 9202:9200
volumes:
  data01:
    driver: local
  data02:
    driver: local
  data03:
    driver: local

networks:
  elastic:
    driver: bridge
```





es运行需要修改一些linux系统权限，修改`/etc/sysctl.conf`文件

```sh
vi /etc/sysctl.conf
```

添加下面的内容：

```sh
vm.max_map_count=262144
```

然后执行命令，让配置生效：

```sh
sysctl -p
```



通过docker-compose启动集群：

```sh
docker-compose up -d
```



### 集群监控

kibana可以监控集群 但是一次只能监控一个集群 多节点需要来回切换

使用cerebro来监控es集群

双击其中的cerebro.bat文件即可启动服务。

![image-20210602220941101](https://typora---------image.oss-cn-beijing.aliyuncs.com/image-20210602220941101.png)



访问http://localhost:9000 即可进入管理界面：

![image-20210602221115763](https://typora---------image.oss-cn-beijing.aliyuncs.com/image-20210602221115763.png)

输入你的elasticsearch的任意节点的地址和端口，点击connect即可：

绿色的条，代表集群处于绿色（健康状态）

### 创建索引库

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-24%2019.30.49.png" alt="截屏2024-09-24 19.30.49" style="zoom:33%;" />

**在cerebro中创建**

![截屏2024-09-24 19.32.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-24%2019.32.08.png)

### 节点角色

![截屏2024-09-24 19.35.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-24%2019.35.04.png)

![截屏2024-09-24 19.37.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-24%2019.37.11.png)

## 集群脑裂问题

**当node1 和 其他node发生了网络阻塞 但是node1没有宕机 其他node认为没有了master 就自己候选了一个master**

**此时当网络恢复的时候就会出翔两个master 出现数据不一致的问题**

![截屏2024-09-24 19.38.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-24%2019.38.34.png)



![截屏2024-09-24 19.40.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-24%2019.40.30.png)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-24%2019.41.43.png" alt="截屏2024-09-24 19.41.43" style="zoom:33%;" />

## 集群故障转移

![截屏2024-09-24 19.50.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-24%2019.50.41.png)



 

![截屏2024-09-24 19.52.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-24%2019.52.29.png)



## 集群分布式存储

**一定不能更改分片数量**

![](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-24%2019.46.11.png)



![截屏2024-09-24 19.47.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-24%2019.47.35.png)







## 集群分布式查询

![截屏2024-09-24 19.48.24](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-24%2019.48.24.png)
