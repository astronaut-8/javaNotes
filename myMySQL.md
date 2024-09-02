# MySQL基础

**远程连接3步骤 开放端口 用户权限 bind地址(mysqld.cnf)**

![image-20221230150720513](https://typora---------image.oss-cn-beijing.aliyuncs.com/image-20221230150720513.png)

## MySQL概述

#### 数据库相关概念

| 名称           | 全称                                                         | 简称                             |
| -------------- | ------------------------------------------------------------ | -------------------------------- |
| 数据库         | 存储数据的仓库，数据是有组织的进行存储                       | DataBase(DB)                     |
| 数据库管理系统 | 操纵和管理数据库的大型软件                                   | DateBase Management System(DBMS) |
| SQL            | 操作**关系型数据库**的编程语言，定义了一套操作关系型数据库**统一**标准。 | Structured Query Language(SQL)   |

#### MySQL数据库

###### 启动与停止

**==win 搜索 cmd 以管理员身份运行==**

这个名字为mysql安装的时候注册的名字

![image-20221230154843473](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221230154843473.png)

**==MySQL默认开机自启==**



###### 客户端连接

**方式一：MySQL提供的客户端命令行工具**

![image-20221230155801043](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221230155801043.png)

![image-20221230155838067](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221230155838067.png)

**方式二：系统自带的命令行工具执行指令**

![image-20221230160228487](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221230160228487.png)

==注意：**使用这种方式时，需要配置PATH环境变量**==





###### 关系型数据库（RDBMS）

*使用表存储数据*

MySQL数据库就是关系型数据库

**概念：建立在关系模型基础上、由多张相互连接的==二维表==组成的数据库**

特点：

- **使用表存储数据**，**格式统一**，便于维护。
- 使用SQL语言操作，标准统一，使用方便。

![image-20221230202627796](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221230202627796.png)

###### 数据模型

![image-20221230203154723](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221230203154723.png)

**DBMS创建操作数据库**

客户段连接DBMS

## SQL

#### SQL通用语法及分类

###### SQL通用语法

1. SQL语句可以单行或多行书写，以分号结尾
2. SQL语句可以使用空格/缩进来增强语句的可读性
3. MySQL数据库的SQL语句不区分大小写，关键字建议使用大写
4. 注释：
   - 单行注释：--注释内容或         #注释内容（MySQL特有）
   - 多行注释：/\*注释内容/\*

###### SQL分类

![image-20230103111245840](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230103111245840.png)

#### DDL

**Data Definition Language**

###### 数据库操作

![image-20230104093245445](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104093245445.png)

**utf-8一个字符三个字节，数据库中有些字符是四个字节，所以字符集一般为utf-8mb4**



```sql
create database itheima default charset utf8mb4//指定字符集
```



###### 表操作

**==查询==**

![image-20230104093415662](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104093415662.png)

**==创建==**

![image-20230104104158130](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104104158130.png)

例子：

![image-20230104105106161](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104105106161.png)

![image-20230104105123267](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104105123267.png)

**==修改==**

![image-20230104112421101](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104112421101.png)

![image-20230104112652574](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104112652574.png)

![image-20230104112919744](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104112919744.png)

![image-20230104113003326](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104113003326.png)

![image-20230104114723636](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104114723636.png)

**重写创建的表是一个新表，原来表中的数据被删除。**

###### 数据类型

**==数值类型==**

![image-20230104105434292](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104105434292.png)

`age int unsigned//无符号的age`

**精度表示整个数据的长度，标度表示小数的位数**

![image-20230104110535441](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104110535441.png)

**==字符串类型==**

![image-20230104110627032](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104110627032.png)

**char是固定长度，括号中是多少就只能是多少。#长度不到，空格补全，大小固定**，定长

**varchar的括号中指定的是最长长度，因为每次存入时要计算实际的长度，所以性能较差。**变长

![image-20230104110855598](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104110855598.png)



**==日期时间类型==**

![image-20230104111139056](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104111139056.png)

#### DML

**DML英文全称是Data  Manipulation Language（数据操作语言），用来对数据库中表的数据进行增删改操作。**

###### 添加数据（INSERT）

![image-20230104142449405](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104142449405.png)

**注意：**

- **插入数据时，指定的字段顺序需要与值的顺序是一一对应的。**
- **字符串和日期型数据应该包含在引号中。**
- **插入的数据大小，应该在字段的规定范围内。**

![image-20230104151344727](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104151344727.png)

###### 修改数据（UPDATA)

![image-20230104151834631](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104151834631.png)

**示范：**

![](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104152052230.png)

###### 删除数据（DELETE）

![image-20230104152209770](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104152209770.png)

**示范：**

![image-20230104152251801](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104152251801.png)

#### DQL

**DQL英文全称是Data Query Language（数据查询语言），数据查询语言，用来查询数据库中表的记录。**

**==DQL编写顺序==**

![image-20230104152921859](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104152921859.png)

###### 基本查询

![image-20230104153040758](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104153040758.png)

**示例：**

![image-20230104154022336](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154022336.png)

![image-20230104154034224](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154034224.png)



![image-20230104154106550](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154106550.png)

![image-20230104154118384](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154118384.png)



![image-20230104154138951](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154138951.png)

![image-20230104154147859](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154147859.png)



![image-20230104154240003](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154240003.png)

![image-20230104154250694](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154250694.png)

###### 条件查询（WHERE）

![image-20230104154401966](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154401966.png)

![image-20230104154439641](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154439641.png)

![image-20230104154451124](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154451124.png)

**示范：**

![image-20230104154620167](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154620167.png)

![image-20230104154722630](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154722630.png)

![image-20230104154903802](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154903802.png)

![image-20230104155026874](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104155026874.png)

![image-20230104155253429](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104155253429.png)

###### 聚合函数

**将一列数据作为一个整体，进行纵向计算**

**==常见的聚合函数==**

![image-20230104155407740](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104155407740.png)

![image-20230104155442498](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104155442498.png)

**==null值不参与聚合函数运算==**

![image-20230104161533446](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104161533446.png)

![image-20230104161641355](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104161641355.png)

###### 分组查询（GROUP BY）

![image-20230104161914571](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104161914571.png)

![image-20230104162031618](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104162031618.png)

![image-20230104162041067](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104162041067.png)



![image-20230104162119984](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104162119984.png)

![image-20230104162132771](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104162132771.png)



![image-20230104162320948](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104162320948.png)

![image-20230104162337921](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104162337921.png)

![image-20230104162430898](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104162430898.png)

###### 排序查询

![image-20230104164812961](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104164812961.png)

![image-20230104164835215](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104164835215.png)

![image-20230104164854289](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104164854289.png)



![image-20230104164938032](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104164938032.png)

![image-20230104164954889](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104164954889.png)



![image-20230104165045970](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104165045970.png)

![image-20230104165104238](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104165104238.png)

###### DQL分页查询

![image-20230104165209152](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104165209152.png)

![image-20230104165233298](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104165233298.png)

![image-20230104165404509](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104165404509.png)

###### DQL执行顺序

![image-20230105101006669](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230105101006669.png)

#### DCL

DCL英文全称是**Data Control Language** （数据控制语言），用来**(1.)管理数据库用户，(2.)控制数据库的访问权限**。

###### 用户管理

![image-20230108102832634](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230108102832634.png)

![image-20230108155559909](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230108155559909.png)

![image-20230108155724758](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230108155724758.png)

![image-20230108155829947](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230108155829947.png)

![image-20230108155920116](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230108155920116.png)

###### 权限控制

![image-20230108160039908](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230108160039908.png)

![image-20230108160155187](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230108160155187.png)

![image-20230108160237824](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230108160237824.png)

![image-20230108160402682](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230108160402682.png)

![image-20230108160427559](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230108160427559.png)

![image-20230108160510730](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230108160510730.png)

**==*放在数据库名表示所有数据库，放在表名表示所有表==**





## 函数

**函数是指一段可以直接被另一段程序调用的程序或代码**

### 字符串函数

![image-20230109111708395](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230109111708395.png)

```sql
concat('hello','MySQL');
lower('hello');
upper('hello');
lpad('01',5,'-');
rpad('01',5,'-');
trim('  hello  ');
substring('hello MySQL',1,5);#索引是从1开始的!!!
```

```sql
#把企业员工的工号统一改为5位数，不足5位在前面补0，比如1号员工变为00001
update emp set workno = lpad(workno,5,'0');
```

### 数值函数

![截屏2024-04-16 16.39.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2016.39.12.png)

```sql
ceil(1.5);#2
floor(1.1);#1
mod(3,4);#3余数
rand();
round(2.345,2);#2.35
```

```sql
#通过数据库函数生成一个六位数验证码
select lpad(round(rand()*1000000,0),6,'0');
```

### 日期函数

![截屏2024-04-16 16.45.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2016.45.29.png)

```sql
curdate();#2021-10-11
curtime();#23:48:23
now();#2021-10-11 23:48:23

year(now());#2021
month(now());#10
day(now());#11

date_add(now(),interval 70 day);

datediff('2021-12-01','2023-10-01');#61第一个时间减去第二个时间，会出现负数

```

```sql
#查询所有员工的入职天数，并根据入职天数倒序排序
select name,datediff(curdate(),entrydate) as 'entryDays' from emp order by entryDays desc;
```

### 流程函数

![截屏2024-04-16 16.58.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2016.58.00.png)

```sql
if (true,'ok','error');

ifnull('ok','default');#第一个参数不为空返回第一个参数，否则返回第二个

#需求：查询emp表的员工姓名和工作地址(北京和上海-->一线城市，其他-->二线城市)
select name ,
	case workspace when '北京' then '一线城市' when '上海' then '一线城市' else '二线城市' end
	from emp;
	
	
#需求：展示学生的成绩 --- >= 85,展示优秀  >= 60展示及格 否则展示不及格
select 
	id,
	name,
	(case when math >= 85 then '优秀' when math >= 60 then '及格' else '不及格' end) '数学',
	english,
	chinese
from score;
```

## 约束

### 概述

**约束是作用于表中字段上的规则，用于限制存储在表中的数据**

**目的是保证数据库中的数据正确，有效性，完整性**

![截屏2024-04-16 17.11.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2017.11.06.png)

### 演示

```sql
create table user(
	id int primary key auto_increment comment '主键',
  name varchar(10) not null unique comment '姓名',
  age int check (age > 0 && age <= 120) comment '年龄'，
  status char(1) default '1' comment '状态',
  gander char(1) comment '性别'
) comment '用户表';
```

### 外键约束

#### 定义

**外键让两张表的数据之间建立连接，从而保证数据的一致性和完整性**



具有外键的表为**子表**(从表)，外键所关联的那张表为**父表**（主表）

不做数据库方面的外键约束，那么就只是**逻辑外键**

#### 添加外键

```sql
create table 表明(
	字段 数据类型,
	[constraint] [外键名称] foreign key (外键字段名) references 主表(主表列名)
);
```

```sql
alter table 表名 add constraint 外键名称 foreign key(外键字段名) references 主表(主表列名);
```

```sql
alter table emp add constraint fk_emp_dept_id foreigh key (dept_id) refereneces dept(id);
```

#### 删除外键

```sql
alter table 表名 drop foreign key 外键名称;
```

```sql
alter table emp drop foreign key fk_emp_dept_id;
```

#### 删除和跟新行为

![截屏2024-04-16 17.38.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2017.38.41.png)

![截屏2024-04-16 20.00.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2020.00.29.png)

## 多表查询

[MySQL --- 多表查询_mysql多表查询_Surpass余sheng军的博客-CSDN博客](https://blog.csdn.net/weixin_53622554/article/details/130642217?ops_request_misc=&request_id=&biz_id=102&utm_term=mysql多&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-130642217.142^v94^chatsearchT3_1&spm=1018.2226.3001.4187)

### 多表关系

![截屏2024-04-16 20.07.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2020.07.02.png)

**一对多**：在多的一方建立外键，指向一的一方的主键

**多对多**：

![截屏2024-04-16 20.08.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2020.08.46.png)

**一对一**：

![截屏2024-04-16 20.10.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2020.10.44.png)

### 多表查询概述

`select * from emp,dept;//会出现笛卡尔积`

笛卡尔积：在数学中，两个集合AB的所有组合情况---->在多表查询的时候，需要消除无效的笛卡尔积

![截屏2024-04-16 20.15.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2020.15.47.png)

`select * from emp,dept where emp.dept_id = dept.id//消除没用的数据`

![截屏2024-04-16 20.19.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2020.19.42.png)

### 内连接

**内连接查询两张表交集的部分**

#### 隐式内连接

`select 字段列表 from 表1 , 表2 where 条件...;    `

```sql
select * from emp,dept where emp.dept_id = dept.id
```

#### 显式内连接

```sql
select 字段列表 from 表1 [inner] join 表2 on 连接条件...;
```

**表起了别名后就不能再使用原来的名字了**(因为执行顺序中，先执行了from，已经把名字给改了)

![截屏2024-04-16 20.29.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2020.29.00.png)

### 外连接

#### 左外连接

```sql
select 字段列表 from 表1 left [outer] join 表2 on 条件...;
```

**相当于查询表1(左)的所有数据和包含表1表2交集部分的数据**

#### 右外连接

```sql
select 字段列表 from 表1 right [outer] join 表2 on 条件...;
```

**相当于查询表2(右)的所有数据和包含表1表2交集部分的数据**

*左外可以相互转换*

```sql
#查询emp表的所有数据和对应的部门信息
select e.*,d.name from emp e left outer join dept d on e.dept_id = d.id;
```

### 自连接

**一定要起别名!!!**

```sql
select 字段列表 from 表A 别名A join 表A 别名B on 条件...;
```

**自连接查询，可以是内连接查询，也可以是外连接查询**

```sql
#在公司员工总表中，查询员工及其所属领导的名字(领导和员工在同一张表中)
select a.name,b.name from emp a join emp b on a.managerid = b.id;#内连接
```

```sql
#查询所有员工，及其领导的名字，员工没有领带也要查出来
select a.name , b.name from emp a left outer join emp b on a.managerid = b.id;#左外连接
```

### 联合查询

**联合查询union查询，就是把多次查询的结果合并起来，形成一个新的查询结果集**

```sql
select 字段列表 from 表A...
union[all]
select 字段列表 from 表B...;
```

```sql
#查询薪资低于5k的员工和年龄大于50的员工全部查询出来
select * from emp where salary < 5000
union all#把all删掉的话，两个sql语句查到的重叠部分就没有了
select * from emp where age > 50
```

**对于联合查询的多张表的列数必须保持一致，字段类型也需要保持一致**

### 子查询

**sql语句中嵌套select语句，称为嵌套查询，又称为子查询**

```sql
select * from t1 where column1 = (select column1 from t2);
```

**子查询外部的语句可以是insert/update/delete/select中的任何一个**



#### 根据子查询结果

##### **标量子查询**

**(子查询结果为单个值)**

常用操作符**:= <> > >= < <=**

```sql
#查询销售部的所有员工信息
select * from emp where dept_id = (select id from dept where name = '销售部');
```

```sql
#查询在方东白入职之后的员工信息
select * from emp where entrydate > (select entrydate from emp where name = '方东白');
```

##### **列子查询**

**(子查询结果为一列)**

常用操作符：**in ,not in, any,some,all**

![截屏2024-04-16 21.25.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2021.25.12.png)

```sql
#查询销售部和市场部的所有员工信息
select * from emp where dept_id in (select id from dept where name = '销售部' or name = '市场部');
```

```sql
#查询比财务部所有人工资都高的员工信息
select * from emp where salary > all(select salary from emp where dept_id = (select id from dept where name = '财务部'));
```

```sql
#查询比研发部任意一人工资高的员工信息
select * from emp where salary > any(select salary from emp where dept_id = (select id from dept where name = '研发部'));
```

##### **行子查询**

**(子查询结果为一行)**

常用操作符: **= ,<>,in,not in**

```sql
#查询与张无忌的薪资及直系领导相同的员工信息
select * from emp where (salary,manegerid) = (select salary,managerid from emp where name = '张无忌');
```

##### **表子查询**

**(子查询结果为多行多列)**

常用**in**

```sql
#查询与路障课，宋远桥的职位和薪资相同的员工信息
select * from emp where (job,salary) in (select job,salary from emp where name = '路障课' or name = '宋远桥');
```

```sql
#查询入职日期是'2006-01-01'之后的员工信息及其部门信息
select e.*,d.* from (select * from emp where entrydate > '2006-01-01') e
left join dept d on e.dept_id = d.id;#左外连接
```

**根据子查询位置**分为

**where之后**

**from之后**

**select之后**

### 多表查询案例

![截屏2024-04-18 09.16.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2009.16.51.png)

### ![截屏2024-04-18 09.16.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2009.16.09.png)

```sql
#查询员工的姓名，年龄，职位，部门信息,(隐式内连接)
select e.name,e.age,e.job,d.name from emp e ,dept d where e.dept_id = d.id;

#查询年龄小于30岁的员工姓名，年龄，职位，部门信息,(显式内连接，on和where的使用,on后为两表的连接条件，where数据筛选)
select e.name,e.age,e.job,d.name from e inner join dept d on e.dept_id = d.id where e.age < 30;

#查询拥有员工的部门id，名称(去重)
select distinct d.id,d.name from emp e, dept d where e.dept_id = d.id;

#查询所有年纪大于40岁的员工，及其归属的部门名称，如果员工没有分配部门，也需要展示出来
select e.*,d.name from emp e left join dept d on e.dept_id = d.id where e.age > 40;

#查询所有员工的工资等级
select e.*,s.grade from emp e,salgrade s where e.salary >= s.losal and e.salary <= s.hisal;
select e.*,s.grade from emp e,salgrade s where e.salary between s.losal and s.hisal;

#查询研发部所有员工的信息及工资等级
select e.*,s.grade from (emp e,salgrade s ,dept d) where (e.salary between s.losal and s.hisal)
and (e.dept_id = d.id) and d.name = '研发部';

#查询研发部员工的平均工资
select avg(e.salary) from emp e,dept d where e.dept_id = d.id and d.name = '研发部';

#查询工资比灭绝搞的员工信息
select * from emp where salary > (select salary from emp where name = '灭绝');


#查询比平均工资薪资高的员工
select * from emp where salary > (select avg(e.salary) from emp);

#查询低于本部门平均工资的员工信息
select * from emp e2 where e2.salary < (select avg(e1.salary) from emp e1 where e1.dept_id = e2.dept_id);

#查询所有的部门信息，并统计部门的员工人数
select d.id,d.name,(select count(*) from emp e where e.dept_id = d.id) '人数' from dept d;

SELECT d.id, d.name, COUNT(e.id) AS '人数'  
FROM dept d  
LEFT JOIN emp e ON d.id = e.dept_id  
GROUP BY d.id, d.name;#group by

#查询所有员工的选课情况，展示出学生名称，学好和课程名称
select s.name,s.no,c.name from student s,student_cource sc,cource c where s.id = sc.studentid and sc.sourceid = c.id;
```

![截屏2024-04-18 09.50.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2009.50.29.png)

## 事物

### 事物简介

**事物是一组操作的集合，它是一个不可分隔的工作单位，事物会把所有的操作作为一个整体一起向系统提交或撤销操作请求，即这些操作同时成功或者同时失败**

mysql默认是自动开始事物的，一次操作后直接提交了

### 事物操作

#### 查看/设置事物提交方式

```sql
select @@autocommit; 
set @@autocommit = 0; # 改为手动  
#会话参数，只针对当前窗口有效
```

#### 提交事物

```sql
commit;
```

#### 回滚事物

```sql
rollback;
```

#### 开启事物

**若不把自动提交改为手动**

**可以手动临时开启一段事物**

```sql
start transaction;
begin;
```

### 事物四大特性

**ACID**

| 特性                 | 说明                                                         |
| -------------------- | ------------------------------------------------------------ |
| 原子性 - Atomicity   | 事物是不可分隔的最小操作单元，要么全部成功，要么全部失败     |
| 一致性 - Consistency | 事物完成的时候，必须使得所有的数据都保持一致状态             |
| 隔离性 - Isolation   | 数据库系统提供的隔离机制，保证事物在不受外部并发操作影响的独立环境下运行(多个事物操作同一个数据库，相互独立不影响) |
| 持久性 - Durability  | 事物一旦回滚或者提交，它对数据库的改变就是永久的             |

### 并发事物问题

**多个并发事物在执行过程中出现的问题**

|    问题    | 描述                                                         |
| :--------: | ------------------------------------------------------------ |
|    脏读    | 一个事物读到另外一个事物还没有提交的数据                     |
| 不可重复读 | 一个事物先后读取同一条记录，但两次读取的数据不同(事物b提交后，事物a的两次读取的值就会不一样) |
|    幻读    | 一个事物按照条件查询数据时，没有对应的数据行，但是在插入数据时，又发现这行数据已经存在，好像出现了幻影，因为前两个问题解决了，事物a不断查询，没有对应的数据行，但是一旦插入，显示数据重复 |

![截屏2024-04-16 22.13.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2022.13.50.png)

![截屏2024-04-16 22.15.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2022.15.16.png)



![截屏2024-04-16 22.16.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2022.16.53.png)

### 事物隔离级别

![截屏2024-04-16 22.18.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2022.18.19.png)

```sql
select @@transaction_isolation -- 查看事物隔离级别
set [session|global] transaction isolation level {read uncommitted | read committed | repeatable read | serializable} -- 设置事物隔离级别,一个时会话，一个是全部窗口
```

解决幻读的时候，事物a查询完比如id=4的数据，事物b插入id=4的数据会等待执行，直到事物a插入id=4的数据，并且提交，事物b那边结束运行，显示报错。

Serializable 是最高的事务隔离级别，在该级别下，事务串行化顺序执行，可以避免脏读、不可重复读与幻读。但是这种事务隔离级别效率低下，比较耗数据库性能，一般不使用，我的理解是事物串行化执行，一个事物开始执行操作了，就获得锁了，别人要等锁释放才可以执行操作，不然就是按下回车等待输出的过程。

**事物隔离级别越高，数据越安全，但是性能越差**



# 2.MySQL进阶

## 2.1存储引擎

### 2.1.1MySQL体系结构

![截屏2024-04-18 11.06.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2011.06.34.png)



从上到下依次为 **连接层，服务层，引擎层，存储层**



**index是在索引引擎层实现的，不同搜索引擎索引的实现是不一样的**

![截屏2024-04-18 11.10.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2011.10.20.png)

### 2.1.2存储引擎简介

**存储引擎就是存储数据，建立索引，跟新/查询数据等技术的实现方式。存储引擎是基于表的，而不是基于库的，所以存储引擎也可被称为表类型**(多张表可以选择不同的存储引擎)

![截屏2024-04-18 11.14.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2011.14.56.png)

**默认InnoDB**

```sql
#在创建表的时候，指定存储引擎
create table 表名(
  ...
)engine = INNODB [comment 注释];
```



```sql
#查看当前数据库支持的存储引擎
show engines;
```

![截屏2024-04-18 11.17.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2011.17.42.png)

### 2.1.3存储引擎特点

#### 2.1.3.1InnoDB

InnoDB是一种**兼容高可靠性和高性能**的通用存储引擎，在MySQL5.5之后，InnoDB是默认的MySQL存储引擎

- DML操作遵循ACID模型，支持**事物**

- **行级锁，**提高并发访问性能

- 支持**外键**FOREIGN KEY 约束，保证数据的完整性和正确性

- xxx.ibd,xxx为表名，innoDB引擎的每张表都会对应这样一个表空间文件，存储该表的表结构(frm,sdi)，数据和索引

  参数：innodb_file_pre_table(是否每一张表都有一个file，8版本后默认是打开的)

  

![截屏2024-04-18 11.31.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2011.31.39.png)

#### 2.1.3.2MyISAM

**MyISAM是mysql早期的默认存储引擎**

- 不支持事物，不支持外键
- 支持表锁，不支持行锁
- 访问速度快
- sdi，表结构信息
- myd，存放数据
- myi代表索引

![截屏2024-04-18 11.34.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2011.34.16.png)

#### 2.1.3.3Memory

**memory引擎的表数据存储在内存中，由于受到硬件问题，断电问题影响，只能将这些表作为临时表或者缓存使用**

- 内存存放，访问速度快
- 支持hash索引(默认)
- 文件：xxx.sdi 存储表结构信息

#### 2.1.3.4三者比较

**关键：事物，锁，外键**

![截屏2024-04-18 11.38.26](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2011.38.26.png)

### 2.1.4存储引擎选择

**选择存储引擎的时候，应该根据应用系统的特点选择合适的存储引擎，对于复杂的应用系统，还可以根据实际情况选择多种存储引擎进行组合**

![截屏2024-04-18 11.40.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2011.40.42.png)

## 2.2索引

### 2.2.1索引概述

索引(index) 是帮助MySQL高**效获取数据**的数据结构(**有序**)。在数据之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式**引用(指向)数据**，这样就可以在这些数据结构上实现高级查找算法，这种数据结构就是索引.

**无索引-->全表扫描，效率低**



**优点**

- 提高数据检索的效率，降低数据库的IO成本
- 通过索引列对数据进行排序，降低数据排序的成本，降低CPU的消耗

**缺点**

- 索引列也要占用空间
- 索引大大提高了查询的效率，同时也降低跟新表的速度，如对表进行INSERT，UPDATE，DELETE，效率低

### 2.2.2索引结构

**不同引擎结构不同**

![截屏2024-04-18 15.14.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2015.14.17.png)

![截屏2024-04-18 15.15.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2015.15.15.png)

<u>默认B+tree结构</u>

#### 2.2.2.1B-tree

**二叉树缺点**：顺序插入的时候，会形成一个链表，查询性能大大降低，大数据量情况下，层级较深，减速速度慢

**红黑树**：大数据情况下，层级较深，减速速度慢

**B-Tree：多路平衡查找树**

树的度---一个节点的子节点个数

一颗最大度数(max-degree) 为5(5阶) 的b-tree为例(每个节点最多存储4key，5个指针)

![截屏2024-04-18 15.22.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2015.22.08.png)

这个数据结构在插入数据的时候，先根据值插入到某一个数据单元中去，当加入后单元的元素个数达到了临界值（阶数）则**中间元素，向上分裂**

![截屏2024-04-18 15.29.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2015.29.19.png)



![截屏2024-04-18 15.29.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2015.29.34.png)



#### 2.2.2.2B+tree

**以一颗最大度数(max-degree)为4(4阶) 的b+tree为例：**

所有元素都会出现在叶子节点，并且形成一个单向链表

上面的数字充当索引使用

![截屏2024-04-18 15.32.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2015.32.17.png)

![截屏2024-04-18 15.35.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2015.35.16.png)

![截屏2024-04-18 15.35.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2015.35.25.png)

**MySQL索引数据结构对经典的B+Tree进行了优化，在原B+Tree的基础上，增加了一个指向相邻叶子节点的链表指针，形成了带有顺序指针的B+Tree，提高区间访问的性能**

![截屏2024-04-18 15.37.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2015.37.44.png)

##### 2.2.2.2.1优点

**相对于二叉树，层级更少，搜索效率更高**

**对于B-tree，无论是叶子节点还是非叶子节点，都会保存数据，这样导致一页中存储的键值对减少，指针跟着减少，要同样保存大量数据，只能增加树的高度，导致性能降低**，b+值都在叶子节点，**搜索效率稳定**，双向链表的设计使得**范围搜索和排序高效**

**相对于Hash索引，B+tree支持范围匹配及排序操作**

#### 2.2.2.3Hash

**哈希索引就是采用一定的hash算法，将键值换算成新的hash值，映射到对应的槽位上，然后存储在hash表中**

![截屏2024-04-18 15.42.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2015.42.01.png)

- **Hash索引只能用于对等比较(=,in) 不支持范围查询(between,>,<,...)**
- **无法利用索引完成排序操作**
- **查询效率高，通常只需要一次检索就可以了，效率通常高于B+tree索引**

**在MySQL中，支持hash索引的是Memory引擎，而InnoDB中具有自适应hash功能(在指定条件下自动构建hash)，hash索引是存储引擎根据B+tree索引在指定条件下自动构成的**

### 2.2.3索引分类

![截屏2024-04-18 15.52.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2015.52.43.png)



#### 2.2.3.1InnoDB分类

**在InnoDB索引引擎中，根据索引的存储形式，又可以分为以下两种**

![截屏2024-04-18 15.54.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2015.54.47.png)

**二级索引--辅助索引**

聚集索引选取规则：

**如果存在主键，主键索引就是聚集索引**

**如果不存在主键，将使用第一个唯一(UNIQUE)索引作为聚集索引**

**如果表没有主键，或没有合适的唯一索引，则InnoDB会自动生成一个rowid作为隐藏的聚集索引**

![截屏2024-04-18 16.01.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2016.01.00.png)

比如按照name进行查询的时候，先在name中找到对应的主键id，再在聚集索引中找到row行，这个过程称为**回表查询**

#### 2.2.3.2InnoDB主键树高度

**InnoDB主键索引的B+tree高度为多高**

假设：

**一页大小为16k，一行数据大小为1k，一页中可以存储16行这样的数据，InnoDB的指针占用6个字节的空间(固定)，主键若为bigiht，占用字节为8**

在高度为2的情况下，有以下等式：

第一页数据：

`n * 8 + (n + 1 * 6 = 16 * 1024) --> n = 1170`

那么一共可以有1171个指针指向叶子节点，每个叶子节点大概可以有16行数据，

那么可以存储的数据行数为 1171 * 16 = 18736

高度为3:

**1171 * 1171 * 16 = 21939856**

### 2.2.4索引语法

#### 2.2.4.1创建索引

`create [unique|fulltext] index index_name on table_name (index_col_name...);`

**不指定[]为常规索引**

**一个为单列索引，多个为联合索引**

#### 2.2.4.2查看索引

`show index from table_name;`

#### 2.2.4.3删除索引

`drop index index_name on table_name`

![截屏2024-04-18 16.28.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2016.28.45.png)

### 2.2.5SQL性能分析

#### 2.2.5.1查看执行频次

**MySQL客户端连接成功后，通过show [session|global] status 命令可以提供服务器状态信息。通过如下指令，可以查看到当前数据库的INSERT，UPDATE，DELETE，SELECT的访问频次**(全局和会话)

`show global status like 'COM___';`

![截屏2024-04-18 16.34.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2016.34.19.png)

#### 2.2.5.2慢查询日志

**慢查询日志记录了所有执行时间超过指定参数(long_query_time,单位:s,默认10秒)的所有sql语句的日志**

**MySQL的慢查询日志默认没有开启，需要在MySQL的配置文件(/etc/my.cnf)配置信息：**

```
#慢查询日志
slow_query_log=1
long_query_time=2
```

```
cd /var/lib/mysql
localhost-slow.log
```

![截屏2024-04-18 20.52.24](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2020.52.24.png)

![截屏2024-04-18 20.55.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2020.55.49.png)

#### 2.2.5.3show profiles

**profile 详情**

**show profiles 能够在做sql优化的时候帮我们了解时间都耗费到哪里去了，通过have_profiling参数，能够看到当前mysql是否支持profile操作**

![截屏2024-04-18 21.16.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2021.16.42.png)

默认profiling是关闭的，可以通过set语句在session/global级别开启profiling

```
set profiling = 1;
```

![截屏2024-04-18 21.18.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2021.18.53.png)

```sql
#查看每一条SQL的耗时基本情况
show profiles;

#查看指定query_id的SQL语句各个阶段的耗时情况
show profile for query query_id;

#查看指定query_id的SQL语句CPU的使用情况
show profile cpu for query query_id; ##query_id是show中看到的 
```

#### 2.2.5.4explain

```sql
#直接在select语句之前加上关键字explain/desc
EXPLAIN SELECT 字段列表 from 表名 WHERE 条件;
```

![截屏2024-04-18 21.32.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2021.32.34.png)

- **id** - select查询的序列号，表示查询中执行select字句或者是操作表的顺序(id相同，执行顺序从上到下；id不同，值越大，越先执行,多表查询的时候会有多个id)

- **select_type** - 表示select的类型，常见的取值有*<u>simple</u>*(简单表，即不使用表连接或子查询)，*<u>primary</u>*(主查询，即外层的查询)，<u>union</u>（*union*中的第二个或者后面的查询语句），*<u>subquery</u>*(select/where之后包含了子查询)等

- **type** - 表示连接类型，性能由好到差的连接类型为**NULL,system,const,eq_ref,ref,range,index,all**,     null基本达不到，到const就很好了，不查询任何表的时候，直接的select才可以达到null，业务系统的sql一般不会到null  

  主键和唯一索引回到const  

  非唯一索引会出现ref

  all-全表扫描

  index-用了索引，但还是会对索引进行扫描遍历

- **possible_key** - 显示可能应用在这张表上的索引，一个或者多个

- **key** - 实际使用的索引如果为null，则没有使用索引

- **key_len** - 表示索引中使用的字节数，该值为索引字段最大可能长度，并非实际使用长度，在不损失精确性的前提下，长度越短越好

- **rows** - MySQL认为必须要执行查询的行数，在innoDB引擎表中，是一个估计值，可能不总是准确的

- **filiterd** - 表示返回结果的行数占需读取行数的百分比，filiterd的值越大越好

- **extra** - 额外信息

**type的range**

![截屏2024-04-19 11.57.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2011.57.00.png)



### 2.2.6索引使用

#### 2.2.6.1最左前缀法则

如果索引了多列(**联合索引)**,要遵循最左前缀法则。最左前缀法则指的是**查询从索引的最左列开始**，并且不跳过索引中的列

如果跳过了某一列，索引将部分失效(**后面的字符索引失效**)

![截屏2024-04-19 11.09.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2011.09.08.png)

```sql
#假设上述的profession，age，status在同一个联合索引里面，那么查询这个字段的时候，要使用索引，会从左向右匹配，那么最左边的匹配一定为联合索引的最左的值，向右匹配的过程中直到不满足联合索引的顺序，结束索引查找，当一次索引都用不上，变成全表查询(all)

#如果用了一点索引，之后再做匹配，index  ,  用了那个索引条件就到头了 -- ref

#只要字段存在，顺序不重要，and会做重排序
```

![截屏2024-04-19 11.45.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2011.45.56.png)

#### 2.2.6.2索引失效情况

##### 2.2.6.2.1范围查询

**联合索引中，出现范围查询(>,<),范围查询右侧的列索引失效**

![截屏2024-04-19 11.18.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2011.18.45.png)

```
>= 会比 > 使用更多索引，性能更高
```

##### 2.2.6.2.2索引列运算

**在索引列上进行运算操作，索引将失效**

![截屏2024-04-19 11.42.24](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2011.42.24.png)

![截屏2024-04-19 11.52.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2011.52.29.png)

##### 2.2.6.2.3字符串不加引号

**字符串类型字段使用时，不加引号，索引将失效，会产生隐式类型转换**

![截屏2024-04-19 11.49.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2011.49.40.png)

![截屏2024-04-19 11.50.37](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2011.50.37.png)

##### 2.2.6.2.4模糊匹配

**如果仅仅是尾部模糊匹配，索引不会失效。如果是头部模糊匹配，索引失效**

![截屏2024-04-19 11.53.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2011.53.53.png)

![截屏2024-04-19 11.55.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2011.55.22.png)

##### 2.2.6.2.5or连接的条件

**用or分隔开的条件，如果or前的条件中的列有索引，而后面的列中没有索引，那么涉及的索引都不会被用到**

![截屏2024-04-19 12.01.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2012.01.19.png)

##### 2.2.6.2.6数据分布影响

**如果mysql评估使用索引比全表更慢，则不适用索引**

![截屏2024-04-19 12.05.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2012.05.54.png)

**当mysql认为大多数数据满足你的条件就不会走索引**

#### 2.2.6.3sql提示

**在sql语句中加入一些认为的提示来达到优化操作的目的**

比如说，一个字段既是联合索引的最左字段又是一个单独的单列索引，此时看使用哪一个索引

```sql
#use index --> 建议 有可能会不适用
explain select * from tb_user use index(idx_user_pro) where profession = 'aa';

#ignore index
explain select * from tb_user ignore index(idx_user_pro) where profession = 'aa';

#force index
explain select * from tb_user force index(idx_user_pro) where profession = 'aa';
```

#### 2.2.6.4覆盖索引

**尽量使用索引覆盖（查询使用了索引，并且需要返回的列，在该索引中已经全部能够找到），减少select***

| extra                    | description                                                  |
| ------------------------ | ------------------------------------------------------------ |
| using index condition    | 查找使用了索引，但是需要回表返回数据                         |
| using where; using index | 查找使用了索引，但是需要的数据都在索引列中能找到，所以不需要回表查询数据 |

![截屏2024-04-19 16.04.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2016.04.16.png)

#### 2.2.6.5前缀索引

**当字段类型为字符串（varchar text 等）有时候需要索引很长的字符串，这会让索引变得很大，查询时，浪费大量的磁盘IO，影响查询效率，此时可以只将字符串的一部分前缀，建立索引，这样可以大大节约索引空间，从而提高索引效率**

```sql
create index idx_xxx on table_name(column(n)); #提取列的前n个字符
```

**前缀长度**

**可以根据索引的选择性来决定，而选择性是指不重复的索引值(基数)和数据表的记录总数的比值，索引选择性越高则查询效率越高，**

**唯一索引的选择性为1，这是最好的索引选择性，性能也是最好的**

```sql
select count(distinct email) / count(*) from tb_user; #distinct对数据去重
select count(distinct substring(email,1,5)) / count(*) from tb_user;
```

![截屏2024-04-19 16.36.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2016.36.08.png)

到了索引可以判断完的地方还没结束，根据row要再检查前缀后是否符合条件(叶子是链表),如上述的17799

#### 2.2.6.6单列索引和联合索引

![截屏2024-04-19 16.40.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2016.40.19.png)

**phone和name都是一个单列索引，但是在查找的时候，只使用了phone的单列索引，name是通过回表查询得到的**

![截屏2024-04-19 16.42.32](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2016.42.32.png)

**在业务场景中，如果存在多个查询条件，考虑针对查询字段建立索引时，建议建立联合索引，而非单列索引**

<u>*联合索引的结构和使用情况*</u>

![截屏2024-04-19 16.44.52](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2016.44.52.png)



### 2.2.7索引设计原则

![截屏2024-04-19 16.48.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2016.48.55.png)

**注意第7条**

## 2.3SQL优化

### 2.3.1插入数据

#### 2.3.1.1批量插入

**500～1000条**

```
insert into table values (),(),();
```

#### 2.3.1.2手动事物提交

**默认自动事物提交，多次提交会有频繁的事物开启和关闭**

```sql
start transaction;
insert into table values (),(),();
insert into table values (),(),();
insert into table values (),(),();
commit;
```

#### 2.3.1.3主键顺序插入

![截屏2024-04-19 17.04.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2017.04.41.png)

**按照主键的顺序插入效率更高**

#### 2.3.1.4大批量插入数据

**如果一次性需要插入大批量数据，使用insert语句插入性能比较低，此时可以使用mysql数据库提供的load指令进行插入**

![截屏2024-04-19 17.10.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2017.10.49.png)

![截屏2024-04-19 17.11.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2017.11.39.png)



### 2.3.2主键优化

#### 2.3.2.1数据组织方式

在InnoDB存储引擎中，**表数据都是根据主键顺序组织存放的**，这种存储方式的表称为索引组织表(index organized table **IOT**)

#### 2.3.2.2页分裂

**页可以为空，也可以填充一般，也可以填满。**

**每个页包含了2-N行数据(如果一行数据过大，会行溢出)，根据主键排序**

![截屏2024-04-19 17.26.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2017.26.39.png)

![截屏2024-04-19 18.11.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.11.11.png)

#### 2.3.2.3页合并

**当删除一行记录时，实际上记录并没有被物理删除，只是被标记(flaged)为删除，并且它的空间变得允许被其他记录声明使用**

![截屏2024-04-19 18.14.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.14.27.png)

![截屏2024-04-19 18.15.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.15.04.png)

**MERGE_THRESHOLD:合并页的阈值，可以自己设置，在创建表或者创建索引时指定**

#### 2.3.2.4主键设计原则

**满足业务需求的情况下，尽量降低主键的长度**(聚集索引只有一个，但是二级索引会有很多个，主键过长，可能会导致二级索引的数据域占用空间过大)

**插入数据时，尽量选择顺序插入，选择使用auto_increment自增主键**(顺序插入效率高)

**尽量不要使用UUID做主键或者是其他自然主键，如身份证号**(无序乱序插入，可能出现页分裂现象,而且长度可能过长)

**业务操作的时候，避免对主键的修改**（会变跟索引结构）

### 2.3.3order by优化

- **Using filesort** : 通过表的索引或全表扫描，读取满足条件的数据行，然后在排序缓冲区sort buffer中完成排序操作，所有不是通过索引直接返回排序结果的排序都叫FileSort排序
- **Using index** ： 通过有序索引扫描直接返回有序数据，这种情况为using index，不需要额外排序，**操作效率高**

![截屏2024-04-19 18.26.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.26.51.png)

<u>*为age添加索引后*</u>

![截屏2024-04-19 18.27.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.27.43.png)

**违背最左前缀法则**

![截屏2024-04-19 18.30.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.30.09.png)

**与索引的排序方式不一致**

![截屏2024-04-19 18.31.21](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.31.21.png)

**为特殊的字段排序创造特有的索引规则，使得filesort不出现**

![截屏2024-04-19 18.33.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.33.56.png)

![截屏2024-04-19 18.34.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.34.29.png)

**以上优化的前提为----覆盖索引**

![截屏2024-04-19 18.38.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.38.20.png)

### 2.3.4group by优化

**分组字段没有创建索引之前**

![截屏2024-04-19 18.41.14](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.41.14.png)

**创建索引后**

![截屏2024-04-19 18.42.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.42.40.png)

**关于最左前缀法则**

![截屏2024-04-19 18.43.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.43.45.png)

**条件构造后满足最左前缀法则**

![截屏2024-04-19 18.44.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.44.55.png)

### 2.3.5limit优化

**覆盖索引 + 子查询**

```sql
#速度慢，数据冗余
select * from tb_sku limit 9000000,10; 

#in limit   mysql版本不支持limit和in/all/any/some子查询
select * from tb_sku where id in (select id from tb_sku order by id limit 9000000,10);

#res
select s.* from tb_sku s , (select id from tb_sku order by id limit 9000000,10) a where s.id = a.id;
```

### 2.3.6count优化

**MyISAM引擎把一个表的总行数存在了磁盘上，因此执行count(*)的时候会直接返回个数，效率高（前提没有where）**

**InnoDB引擎执行的时候，需要把数据一行一行地从引擎里面读出来，然后累计计数**

优化思路：**自己计数**

`count()是一个聚合函数，对于返回的结果集，一行行判断，如果count函数的参数不是null，累计值+1，否则不加，最后返回累计值`

*用法：*

```sql
count(*)；#求这个表的总记录数量，InnoDB不会把全部字段取出来，而是做了专门优化，不取值，服务层直接按行进行累加
count(主键)；#遍历整张表，取主键的id，返回给服务层，服务层拿到主键后，直接按行进行累加(主键不可能为null)
count(字段)；#不是null的值(InnoDB引擎遍历整张表把每一行的字段取出来，返回给服务层,根据有没有not null 约束来进行直接累计计数或先判断再累➕计数)
count(1)；#遍历整张表，但不取值，服务层对于返回的每一行，放一个数字1进去，直接按行进行累加
```

效率排行：

`count(字段) < count(主键id)  < count(1) 约等于 count(*),所以尽量使用count(*)`

### 2.3.7update优化

事物开启后，使用update跟新数据的时，一定要**进行索引字段当where条**件，不然就会触发**表锁**，别的事物无法操作

**并发性能降低**，使用索引字段当where条件只会触发行锁(InnoDB特点)

**InnoDB的行锁是针对索引加的锁，不是针对记录加的锁，并且该索引不能失效，否则会从行锁升级为表锁**

## 2.4视图/存储过程/触发器

### 2.4.1视图

#### 2.4.1.1简介

**视图(view) 是一种虚拟存在的表。视图中的数据并不在数据库中实际存在，行和列数据来自定义视图的查询中使用的表(基表)，并且是在使用视图时动态生成的**

视图只保存了查询的SQL逻辑，不保存查询结果。所以我们在创建视图的时候，主要的工作就落在创建这条SQL查询语句上

#### 2.4.1.2基本语法

**创建**

```sql
create [or replace] view 视图名称[(列表名称)] as select语句 [with[cascaded | local] check option]
create or replace view stu_v_1 as select id,name from student where id <= 1;
```

![截屏2024-04-19 20.24.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2020.24.29.png)

**查询**

```sql
#查看创建视图语句，默认参数也会显示
show create view 视图名称;

#查看视图数据
select * from 视图名称 条件;
```

**修改**

```sql
create [or replace] view 视图名称[(列名列表)] as select语句 [with[cascaded | local] check option]

alter view 视图名称[(列名列表)] as select语句 [with[cascaded | local] check option]
```

**删除**

```sql
drop view [if exists] 视图名称 [,视图名称] ...;
```

#### 2.4.1.3检查选项(cascaded)

如图，视图创建的时候限制的条件是 id <= 20

当想视图'插入'一条id为30的数据的时候，再去select*，视图返回的结果找不到这个记录,虽然基表已经添加了这个记录

![截屏2024-04-19 20.38.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2020.38.09.png)

```sql
create or replace view account_v as select id,name from account where id  <= 20
with cascaded check option ;#会检查数据插入
```

![截屏2024-04-19 20.42.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2020.42.57.png)



当使用**with check option**字句创建视图时，MySQL会通过视图检查正在更改的每个行，例如 插入，更新，删除，**以使其符合视图的定义**。MySQL允许基于另一个视图创建视图，他还会**检查依赖视图中的规则以保持一致性**。为了确定检查的范围，mysql提供了两个选项：

cascaded 和 local **默认值为cascaded**

cascaded -- v1是一个视图，存在约束条件，**v2是根据v1创建的视图**，当选择cascaded，v2在检查自己的条件的同时还会使得条件满足v1的条件

承上，若v3是根据v2创建的，但是没有加**with cascaded check option**，不会检查自己的条件，但是会满足v2的(包括v2自己关联v1)的条件(加**with cascaded check option**满足所有继承关系的条件，不管父级视图是否检查)

#### 2.4.1.4检查选项(local)

v2 继承自 v1，当v2家了local检查选项的时候，会检查自己的条件，**当v1也设置了检查条件才检查条件**，不然不检查

![截屏2024-04-20 16.04.07](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-20%2016.04.07.png)

#### 2.4.1.5跟新及作用

要使视图可跟新，视图中的行与基础表中的行必须存在一对一的关系，**如果视图包含以下任何一项，都不可跟新**

- 聚合函数或窗口函数(sum,min,max,count)
- distinct
- group by
- having
- union , union all



**视图的作用**

- **简单**- 视图不仅可以简化用户对数据的理解，也可以简化他们的操作，那些被经常使用的查询可以被定义为视图，从而使得用户不必为以后的操作每次指定全部的条件
- **安全** - 数据库可以授权，但不能授权到数据库特定的行和特定的列上，通过视图用户只能查询和修改他们所能见到的数据
- **数据独立** - 视图可以帮助用户屏蔽真实表结构变化带来的影响（比如数据库里的一个字段名变了，为了不影响视图使用，只需要replace视图，给字段起个别名，这样子相当于完全没变）

#### 2.4.1.6案例

```sql
#为了保证数据库表的安全性，开发人员在操作tb_user表时，只能看到的用户的基本字段，屏蔽手机号和邮箱两个字段
create or replace view tb_user_view as select id,name,profession,age,gender,status,create_time from tb_user;

#查询每个学生所选修的课程(三表联查)，这个功能在很多的业务中都有使用，为了简化操作，定义一个视图(注意避免重复字段)
create or replace view tb_stu_cource_view as select s.name stu_name,s.no stu_no,c.name cou_name from student s,student_cource sc ,cource c where s.id = sc.studentid and sc.courceid = c.id;
```

### 2.4.2存储过程

#### 2.4.2.1简介

**存储过程是事先经过编译并存储在数据库中的一段SQL语句的集合，调用存储过程可以简化应用开发人员的很多工作，减少数据在数据库和应用服务器之间的传输，对于提高数据处理的效率是有好处的**

**存储过程思想上很简单，就是数据库SQL语言层面的代码封装与重用**



**特点**

- 封装，复用
- 可以接受参数，也可以返回数据
- 减少网络交互，效率提升

#### **2.4.2.2基本语法**

```sql
#创建
create procedure 存储过程名称([参数列表])
begin
 		--sql语句
end;

#------------------------------------

#调用
call 名称([参数]);


#------------------------------------

#查看,查询指定数据库的存储过程及状态信息
select * from information_schema.ROUTINES where ROUTINE_SCHEMA = 'xxx';
#查询某个存储过程的定义
show create procedure 存储过程名称;

#------------------------------------

#删除
drop procedure [if exists] 存储过程名称;
```

```sql
create procedure p1()
begin
    select * from account;
end;

call p1();

select * from information_schema.ROUTINES  where ROUTINE_SCHEMA = 'db1';
```



当在命令行中使用创建语法会出问题，因为begin和end中间的语句结束存在';',会认为语句结束了，不符合创建语法

所以在命令行中，执行创建存储过程的sql时，**需要通过关键字delimiter指定sql语句的结束符**

![截屏2024-04-20 16.42.32](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-20%2016.42.32.png)

![截屏2024-04-20 16.44.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-20%2016.44.29.png)

#### 2.4.2.3变量

##### 2.4.2.3.1系统变量

**系统变量是MySQL提供，不是用户定义的，属于服务器层面，分为全局变量(global),会话变量(session)**

```sql
#查看系统变量
show [session | global] variables; --查看所有系统变量
show [session | global] variables like '....'; --通过like模糊查找
select @@[session | global] 系统变量名; --查看指定系统变量 #select @@global.autocommit;
```

```sql
#设置系统变量
set [global | session] 系统变量名 = 值;
set @@[session | global]系统变量名 = 值;
```

**全局系统变量当mysql服务器重启后，还是会变成初始的默认值，想要更改这个默认值，需要到mysql的配置文件修改**

##### 2.4.2.3.2用户定义变量

**用户自定义变量**是用户根据需要自己定义的变量，用户变量不用提前声明，在用的时候直接用<u>@变量名</u>使用就可以。其作用域为当前连接

```sql
#赋值
set @var_name = expr [, @var_name = expr]...;
set @var_name := expr [,@var_name := expr]...;

select @var_name := expr [, @var_name = expr]...;
select 字段名 into @var_name from 表名;
```

```sql
#使用
select @var_name;
```

```sql
set @my_name = 'account';
set @my_age := 10;#推荐，可以区别于 '==',因为sql的判断相等为=

set @my_name := 'account',@my_hobby = 'java';

select count(*) into @my_count from account;

select @my_age,@my_hobby;
```

**用户定义的变量无需对其进行声明或初始化，只不过获取到的值为null**

##### 2.4.2.3.3局部变量

**局部变量是根据需要定义在局部生效的变量，访问之前，需要declare声明。可用作存储过程内的局部变量和输入参数，局部变量的范围是在其内声明的begin ... end块**

```sql
#声明
declare 变量名 变量类型 [default ... ]; #数据类型就是数据库字段类型
```

```sql
#赋值
set 变量名 = 值
set 变量名 := 值
select 字段名 into 变量名 from 表名 ...;
```

```sql
create procedure p3()

begin
    declare acc_count int default 0;

    select count(*) into acc_count from account;
    select acc_count;
end;
```

#### 2.4.2.4If判断

```sql
if 条件1 then
	。。。
elseif 条件2 then
	。。。
else
  。。。
end if；
```

```java
create procedure p4()

begin
    declare score int default 58;
    declare result varchar(10);
    if score >= 85 then
        set result := '优秀';
    elseif score >= 60 then
        set result = '及格';
    else
        set result = '不及格';
    end if;
    select result;
end;

call p4();
```

#### 2.4.2.5参数

| 类型  | 含义                                         | 备注 |
| ----- | -------------------------------------------- | ---- |
| in    | 该类参数作为输入，也就是需要调用时传入值     | 默认 |
| out   | 该类参数作为输出，也就是该参数可以作为返回值 |      |
| inout | 既可以作为输入参数，也可以作为输出参数       |      |

```sql
create procedure 存储过程名称([in/out/inout 参数名 参数类型])
begin
  --sql语句
end;
```

```sql
create procedure p4()

begin
    declare score int default 58;
    declare result varchar(10);
    if score >= 85 then
        set result := '优秀';
    elseif score >= 60 then
        set result = '及格';
    else
        set result = '不及格';
    end if;
    select result;
end;

call p4();
```

```sql
create procedure p6(inout score double)
begin
    set score := score * 0.5;
end;

set @score = 178;
call p6(@score);
select @score;
```

#### 2.4.2.6case

```sql
CASE case_value
	when when_value1 then statement_list1
	[when when_value2 then statement_list2]...
	[else statement_list]
end CASE;
```

```sql
case
	when search_condition1 then statement_list1
	[when search_condition2 then statement_list2]...
	[else statement_list]
end case;
```

```sql
create procedure p7(in month int)
begin
    declare result varchar(10);
    case
        when month >= 1 and month <= 3 then
            set result := '第一季度';
        when month >4 and month <= 6 then
            set result := '第二季度';
        when month >=7 and month <= 9 then
            set result := '第三季度';
        when month >= 10 and month <= 12 then
            set result := '第四季度';
        else
            set result := '非法参数';
    end case ;

    select concat(month,'月，为',result);
end;

call p7(3);
```

#### 2.4.2.7循环结构

##### 2.4.2.7.1while循环

```sql
#先判定条件，如果条件为true，则执行逻辑，否则，不执行逻辑
while 条件 do
	sql逻辑...
end while;
```

```sql
create procedure p8(in n int)
begin

    declare res int default 0;
    while n > 0 do
        set res := n + res;
        set n := n -1;
    end while;
    select res;
end;

call p8(100);
```

##### 2.4.2.7.2repeat循环

```sql
#先执行一次逻辑，然后判定逻辑是否满足，如果满足，则退出。如果不满足，则继续下一次循环
repeat
	sql逻辑...
	until 条件
end repeat;
```

```sql
create procedure p9(in n int)
begin

    declare res int default 0;
    repeat
        set res := res + n;
        set n = n -1;
    until n <= 0
    end repeat;
    select res;
end;

call p9(100);
```

##### 2.4.2.7.3loop循环

LOOP实现简单的循环，如果不在SQL逻辑中增加退出循环的条件，可以用其来实现简单的死循环。LOOP可以配合一下两个语句使用:

- **LEAVE**  -  配合循环使用，退出循环
- **ITERATE**  -  必须用在循环中，作用是跳过当前循环剩下的语句，直接进入下一次循环

```sql
[begin_label:] LOOP
	sql逻辑...
end loop [end_label];
```

```sql
#label为循环标记
leave lable; -- 退出循环
iterate label; -- 直接进去下一次循环
```

```sql
create procedure p10(in n int)
begin
    declare res int default 0;
    sum:loop
        set res := res + n;
        set n = n -1;
        if n <= 0 then
            leave sum;
        end if;
    end loop sum;
    select res;
end;

call p10(100);
```

```sql
create procedure p11(in n int)
begin
    declare res int default 0;
    sum:loop
        if n % 2 = 1 then
            set n = n -1;
            iterate sum;
        end if;
        set res := res + n;
        set n = n -1;
        if n <= 0 then
            leave sum;
        end if;
    end loop sum;
    select res;
end;

call p11(100);
```

#### 2.4.2.8游标

游标是用来**存储查询结果集的数据类型**，在存储过程和函数中可以使用游标**对结果集进行循环的处理**。游标的使用包括游标的**声明，open，fetch，close**

```sql
#声明游标
declare 游标名称 cursor for 查询语句；

#打开游标
open 游标名称；

#获取游标记录
fetch 游标名称 into 变量[,变量];

#关闭游标
close 游标名称;
```

#### 2.4.2.9条件处理程序

**条件处理程序(Handler)可以用来定义在流程控制结构执行过程中遇到问题时相应的处理步骤**

```sql
declare handler_action handler for condition_value [,condition_value] ... statement;

handler_action
	continue:继续执行当前程序
	exit:终止执行当前程序
condition_value
	sqlstate sqlstate_value : 状态码,如02000
	sqlwarning : 所有以01开头的sqlstate代码的简写
	not found : 所有以02开头的sqlstate代码的简写
	sqlexception :  所有没有被sqlwarning 或not found 捕获的sqlstate代码的简写
```

```sql
#根据传入的参数uage，查询用户表tb_user中所有用户年龄小于uage的用户name，profession，
#并根据用户的姓名和专业插入到所创建的一张新表(id,name,profession)中
create procedure p12(in uage int)
begin
    declare uname varchar(100);
    declare upro varchar(100);#先声明普通变量，再声明游标
    declare u_cursor cursor for select name,profession from tb_user where age <= uage;
    #declare exit handler for not found close u_cursor;
    declare exit handler for sqlstate '02000' close u_cursor;#当满足sql状态码为02000时触发条件处理器，并关闭游标
    
    drop table if exists tb_user_pro;
    create table if not exists tb_user_pro(
        id int primary key auto_increment,
        name varchar(20),
        profession varchar(20)
    );
    open u_cursor;
    while true do
        fetch u_cursor into uname,upro;
        insert into tb_user_pro values (null,uname,upro);
    end while;#死循环无法结束，在游标内数据被遍历完之后会出现02000的sql错误状态码
    close u_cursor;
end;
```

### 2.4.3存储函数

**存储函数是有返回值的<u>存储类型</u>，存储函数的参数只能是in类型的**

```sql
create function 存储函数名称([参数列表])
returns type [characteristic ... ]
begin
	--sql语句
	return ...;
end;

characteristic说明:
1 - deterministic : 相同的输入参数总是产生相同的结果
2 - no sql :不包含sql语句
3 - reads sql data ： 包含读取数据的语句，但不包含写入数据的语句
```

```sql
-- 存储函数
create function fun1(n int)
returns int deterministic
begin
    declare total int default 0;
    while n > 0 do
        set total := total + n;
        set n = n -1;
    end while;

    return total;
end;

select fun1(100);
```

**必须有返回值，这玩意用的比较少**

### 2.4.4触发器

#### 2.4.4.1简介

**触发器是与表有关的数据库对象，指在insert/update/delete之前或之后，触发并执行触发器中定义的sql语句集合。触发器的这种特性可以协助应用在数据库端确保数据的完整性，日志记录，数据校验等操作。**

**使用别名OLD和NEW来引用触发器中发生变化的记录内容，这与其他的数据库是相似的。现在触发器还只支持行级触发，不支持语句级触发(比如一次update影响了5行，触发器触发5次，这是行级触发器，对于语句级触发器，一次update不管影响几行，之后触发一次触发器)**

| 触发器类型     | NEW和OLD                                               |
| -------------- | ------------------------------------------------------ |
| INSERT型触发器 | NEW表示将要或者已经新增的数据                          |
| UPDATE型触发器 | OLD表示修改之前的数据，NEW表示将要或者已经修改后的数据 |
| DELETE型触发器 | OLD表示将要或者已经删除的数据                          |

#### 2.4.4.2语法

```sql
#创建
create trigger trigger_name
before/after insert/update/delete
on table_name for each row -- 行级触发器
begin
	trigger_stmt;
end;

#查看
show triggers;

#删除
drop trigger [schema_name.]trigger_name; -- 如果没有指定schema_name，默认为当前数据库
```

#### 2.4.4.3insert案例

![截屏2024-04-20 20.12.38](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-20%2020.12.38.png)

```sql
create table user_logs(
    id int(11) not null auto_increment primary key ,
    operation varchar(20) not null comment '操作类型,insert/update/delete',
    operation_time datetime not null comment '操作时间',
    operation_id int(11) not null comment '操作ID',
    operation_params varchar(500) comment '操作参数'
)engine=innodb default  charset = utf8;
```



```sql
create trigger tb_user_insert_trigger
    after insert on tb_user for each row
begin
    insert into user_logs(id, operation, operation_time, operation_id, operation_params) VALUES
    (null,'insert',now(),new.id,concat('插入的数据内容',new.id,new.name));
end;
```

#### 2.4.4.4update案例

```sql
create trigger tb_user_update_trigger
    after update on tb_user for each row
begin
    insert into user_logs(id, operation, operation_time, operation_id, operation_params) VALUES
        (null,'update',now(),new.id,concat('跟新前数据内容',old.name,'跟新后的数据',new.name));
end;
```

#### 2.4.4.5delete案例

```sql
create trigger tb_user_delete_trigger
    after delete on tb_user for each row
begin
    insert into user_logs(id, operation, operation_time, operation_id, operation_params) VALUES
        (null,'delete',now(),old.id,concat('删除的数据为',old.name));
end;
```

## 2.5锁

### 2.5.1概述

**锁是计算机协调多个进程或线程并发访问某一资源的机制。在数据库中，除传统的计算机资源(CPU,RAM,I/O)的争用以外，数据也是一种供许多用户共享的资源，如何保证数据并发访问的一致性,有效性是所有数据库必须解决的一个问题，锁冲突也是影响数据库并发访问性能的一个重要因素。从这个角度来说，锁对数据库而言显得尤其重要，也更加复杂。**

### 2.5.2全局锁

**全局锁就是对整个数据库实例加锁，加锁后整个实例就处于只读状态，后续的DML的写语句，DDL语句，以及跟新操作的事物提交语句都将被阻塞。**

**其典型的使用场景是做全库的逻辑备份，对所有的表进行锁地，从而获取一致性视图，保证数据的完整性**

![截屏2024-04-20 20.49.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-20%2020.49.48.png)

![截屏2024-04-20 20.51.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-20%2020.51.03.png)

```sql
#加锁
flush tables with read lock;

#逻辑备份
mysqldump [-h -P]-u root -p 137321Aa account > D://account.sql #windows操作，不是mysql的操作

#解锁
unlock tables;
```



**问题**

- 如果在主库上备份，那么在备份期间都不能执行跟新，业务基本就得停摆
- 如果在从库备份，那么在备份期间从库不能执行主库同步过来的二进制日志(binlog)，会导致**主从延迟**



**在InnoDB引擎中，我们可以在备份时加上参数 --single-transaction参数来完成不加速的一致性数据备份**

```
mysqldump --single-transaction -u root -p 137321Aa account > D://account.sql #windows操作，不是mysql的操作
```

![截屏2024-04-20 21.00.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-20%2021.00.47.png)

### 2.5.3表级锁

**表级锁，每次操作锁住整张表，锁定粒度大，发生锁冲突的概率最高，并发度最低，应用在MyISAM，InnoDB，BDB等存储引擎中**

#### 2.5.3.1表锁

##### **2.5.3.1.1表共享读锁**

(read lock)

**加了表锁后，表还可以读，加表锁的会话窗口不能再执行加入操作，别的会话窗口进行写入操作会进入堵塞状态**

**知道加锁的那边释放锁后，加入操作才会继续执行**

##### 2.5.3.1.2**表独占写锁**

(write lock)

一个客户端对表加了写锁后，可以进行读写，但别的客户端就不能读写，会进入阻塞状态

```sql
#加锁
lock tables 表名 ... read / write

#释放锁
unlock tables /客户端断开连接
```

#### 2.5.3.2元数据锁

(meta data lock.MDL)

**MDL加锁过程是系统自动控制，无需显示使用，在访问一张表的时候会自动加上。MDL锁的主要作用是维护表元数据(表结构)的数据一致性在表上有活动事物的时候，不可以对元数据进行写入操作**

----避免DML和DDL冲突，保证读写的正确性

**当对一张表进行增删改查的时候，加MDL读锁(共享);当对表结构进行变更操作的时候，加MDL写锁(排他)**

![截屏2024-04-22 13.02.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2013.02.16.png)

![截屏2024-04-22 13.05.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2013.05.06.png)

```sql
#查看元数据锁
select object_type,object_schema,object_name,lock_type,lock_duration from performance_schema.metadata_locks;
```

![截屏2024-04-22 13.32.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2013.32.19.png)

#### 2.5.3.3意向锁

**为了避免DML在执行时，加的行锁与表锁的冲突，在InnoDB中加入了意向锁，使得表锁不用检查每行数据是否加锁，使用意向锁来减少表锁的检查**

事物A开启，修改一行数据，为那一行数据加入了行锁，并加入意向锁，表锁在添加的时候，检查是否有意向锁，以及要加的表锁是否和意向锁冲突(兼容直接加，不兼容就进入阻塞状态)



1 - 意向共享锁(IS) - 由语句select ... lock in share mode 添加

2 - 意向排他锁(IX) - 由insert, update,delete,select ... for update添加

- 意向共享锁(IS) 与表锁共享锁(read)兼容，与表锁排他锁(write)互斥
- 意向排他锁(IX) 与表锁共享锁(read)和排他锁都互斥。意向锁之间不会互斥

```sql
#查看意向锁及行锁的加锁情况
select object_schema,object_name,index_name,lock_type,lock_mode,lock_data from performance_schema.data_locks;
```

```sql
#加锁
select ....... lock in share mode;
```

### 2.5.4行级锁

**行级锁，每次操作锁住对应的行数据，锁定粒度最小，发生锁冲突的概率最低，并发度最高。应用在InnoDB存储引擎中**

InnoDB的数据是基于索引组织的，行锁是通过**对索引上的索引项加锁来实现的**，而不是对记录加的锁，对于行级锁，主要分为以下三类

#### 2.5.4.1行锁

(Record Lock) - 锁定单个记录的锁，防止其他事物对此进行update和delete,在RC，RR隔离级别下都支持

![截屏2024-04-22 13.44.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2013.44.25.png)

![截屏2024-04-22 13.45.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2013.45.35.png)

事物A获取共享锁后，事物B还可以获取共享锁，但不能获取排他锁

事物A获取排他后，事物B就不能再获取共享锁和排他锁了

![截屏2024-04-22 13.47.13](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2013.47.13.png)

![截屏2024-04-22 13.48.33](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2013.48.33.png)



#### 2.5.4.2间隙锁

(Gap Lock) - 锁定索引记录间隙(不含改记录)，确保索引记录间隙不变，防止其他事物在这个间隙进行insert，产生幻读。在RR隔离级别下都支持

![截屏2024-04-22 14.11.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2014.11.48.png)

![截屏2024-04-22 14.14.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2014.14.22.png)

#### 2.5.4.3临键锁

(Next-Key Lock) - **行锁和间隙锁的组合**，同时锁住数据，并锁住数据前面的gap，在RR级别下支持

![截屏2024-04-22 14.22.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2014.22.34.png)

## 2.6InnoDB引擎

### 2.6.1逻辑存储结构

![截屏2024-04-22 14.28.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2014.28.53.png)

- **表空间** - ibd文件，一个mysql实例可以对应多个表空间，用于存储记录，索引等数据
- **段** - 分为数据段(leaf node segment),索引段(non-leaf node segment),回滚段(Rollback segment),**InnoDB是索引组织表**，数据段就是B+树段叶子节点，索引段即为B+树段非叶子节点。段用来管理多个Extent
- **区** - 表空间的单元结构，**每个区的大小为1M**，默认情况下，**InnoDB存储引擎页大小为16K**，即一个区中**一共有64个连续的页**
- **页** - 是InnoDB存储引擎磁盘管理的最小单元，每个页的大小默认为16KB，**为了保证页的连续性**，InnoDB存储引擎每次从磁盘申请**4-5个区**
- **行** - InnoDB存储引擎数据是**按行进行存放的**

![截屏2024-04-22 14.36.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2014.36.44.png)

### 2.6.2架构

**左侧为内存结构图，右侧为磁盘结构**

![截屏2024-04-22 14.37.52](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2014.37.52.png)

#### 2.6.2.1内存结构

##### 2.6.2.1.1缓冲池

![截屏2024-04-22 14.41.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2014.41.08.png)

**增删改操作先把数据缓存到缓存区(如果缓存区没有数据的话)，避免每次进行磁盘IO，之后统一或者是在触发了某一机制后才进行统一的IO读写**

##### 2.6.2.1.2更改缓冲区

![截屏2024-04-22 14.45.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2014.45.00.png)

![截屏2024-04-22 14.45.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2014.45.57.png)

**非唯一的二级索引随机性很大，将很多次操作缓存在更改缓冲区，后面统一一次合并恢复到缓冲池，就可以统一进行IO**

##### 2.6.2.1.3自适应hash索引

(hash只适合等值匹配，不适合范围查询)

**自适应hash索引，用于优化对Buffer Pool数据的查询。InnoDB存储引擎会监控对表上各索引页的查询，如果观察到hash索引可以提升速度，则建立hash索引**

**无序人工干预，系统根据情况自动完成**

参数: adaptive_hash_index

![截屏2024-04-22 14.52.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2014.52.34.png)

##### 2.6.2.1.4日志缓冲区

![截屏2024-04-22 14.53.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2014.53.30.png)

![截屏2024-04-22 14.54.21](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2014.54.21.png)

| 时机 | 解读                                           |
| ---- | ---------------------------------------------- |
| 0    | 每秒将日志写入并刷新到磁盘一次                 |
| 1    | 日志在每次事物提交时写入并刷新到磁盘           |
| 2    | 日志在每次事物提交后写入，并每秒刷新到磁盘一次 |

#### 2.6.2.2磁盘结构

![截屏2024-04-22 14.58.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2014.58.03.png)

##### 2.6.2.2.1System Tablespace

系统表空间是更改缓冲区的存储区域。如果表是在系统表空间而不是每个表文件或通用空间中创建的，他也有可能包含表和索引数据

(mysql 5.x 版本中还包含InnoDB数据字典，undolog等)

**参数 - innodb_data_file_path**

![截屏2024-04-22 15.01.24](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2015.01.24.png)



##### 2.6.2.2.2FIle-Per-Table Tablespaces

**每个表的文件表空间包含单个InnoDB表的数据和索引，并存储在文件系统上的单个数据文件中**

**参数 - innodb_file_per_table** 默认开启

![截屏2024-04-22 15.04.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2015.04.11.png)

##### 2.6.2.2.3General Tablespaces

通用表空间，需要通过CREATE TABLESPACE 语法创建通用表空间，在创建表时，可以指定改表空间

```sql
create tablespace xxx add datafile 'file_name' engine = engine_name;
```

![截屏2024-04-22 15.08.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2015.08.06.png)

自己创建，自己指定

##### 2.6.2.2.4Undo Tablespaces

撤销表空间

MySQL实例在初始化时会自动创建两个默认的undo表空间(初始大小为16M)，用于存储undo log日志

##### 2.6.2.2.5Temporary Tablespaces

InnoDB使用会话临时表空间和全局临时表空间。存储用户创建的临时表等数据

##### 2.6.2.2.6Doublewrite Buffer Files

双写缓冲区

**InnoDB引擎将数据页从Buffer Pool刷新到磁盘前，先将数据页写入双写缓冲区文件，便于系统异常时恢复数据**

.dblwr

##### 2.6.2.2.7Redo Log

重做日志

![截屏2024-04-22 15.14.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2015.14.28.png)

以循环方式写入重做日志文件，涉及两个文件![截屏2024-04-22 15.15.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2015.15.50.png)

#### 2.6.2.3后台线程

**将InnoDB存储引擎缓冲池中的数据在合适的时机刷新到磁盘文件中去**

![截屏2024-04-22 15.20.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2015.20.18.png)

### 2.6.3事物原理

#### 2.6.3.1概述

![截屏2024-04-22 15.30.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2015.30.35.png)

![截屏2024-04-22 15.32.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2015.32.27.png)

![截屏2024-04-22 17.07.10](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2017.07.10.png)

#### 2.6.3.2redolog

![截屏2024-04-22 15.33.33](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2015.33.33.png)

![截屏2024-04-22 15.36.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2015.36.42.png)

执行增删改操作的时候，修改缓冲池中的数据(如果没有对应的数据的话，调用线程从磁盘中读取数据)，在redolog buffer中缓存变化的操作，**并跟新到磁盘中的 redo log file 重做日志文件**   如果事物提交发生错误，可以使用这个重做文件进行数据回滚（脏页刷新到磁盘发生错误的时候）

WAL(write-ahead-logging)先写日志

事物操作通常有很多条记录，这些记录随机操作数据页，涉及到大量的随机磁盘IO

如果使用log，log日志文件是追加的，此时它为顺序磁盘IO，性能高于随机磁盘IO

**事物成功提交后，这个日志也没啥用了，所以那个日志是可以循环使用的**

#### 2.6.3.3undolog

![截屏2024-04-22 15.45.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2015.45.30.png)

### 2.6.4MVCC

**Multi-Version Concurrency Control，多版本并发控制**

#### 2.6.4.1基本概念

![截屏2024-04-22 15.53.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2015.53.23.png)

右侧事物提交后，左侧事物的事物隔离级别的不可重复度被解决了，所以直接select是读不到跟新后的数据的

但是加了 lock in share mode后，读取最新数据

![截屏2024-04-22 15.53.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2015.53.54.png)

![截屏2024-04-22 16.34.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2016.34.11.png)

#### 2.6.4.2隐藏字段

![截屏2024-04-22 16.35.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2016.35.49.png)

#### 2.6.4.3undolog版本链

![截屏2024-04-22 16.40.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2016.40.45.png)

![截屏2024-04-22 16.44.59](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2016.44.59.png)

#### 2.6.4.4readview介绍

**ReadView(读视图) 是 快照读 SQL执行时MVCC提取数据的依据，记录并维护系统当前活跃的事物(未提交的)id**

| 字段           | 含义                                               |
| -------------- | -------------------------------------------------- |
| m_ids          | 当前活跃的事物ID集合                               |
| min_trx_id     | 最小活跃事物ID                                     |
| max_trx_id     | 预分配事物ID，当前最大事物ID+1(因为事物ID是自增的) |
| creator_trx_id | ReadView创建者的事物ID                             |

![截屏2024-04-22 16.51.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2016.51.45.png)

![截屏2024-04-22 16.53.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2016.53.43.png)

#### 2.6.4.5RC级别原理分析

![截屏2024-04-22 17.00.13](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2017.00.13.png)

#### 2.6.4.6RR级别原理分析

![截屏2024-04-22 17.03.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2017.03.09.png)

## 2.7MySQL管理

### 2.7.1系统数据库介绍

![截屏2024-04-22 17.14.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2017.14.40.png)

### 2.7.2常用工具

#### 2.7.2.1mysql

![截屏2024-04-22 17.15.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2017.15.23.png)

**#主要要指定数据库**

#### 2.7.2.2mysqladmin

![截屏2024-04-22 17.17.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2017.17.30.png)

![截屏2024-04-22 17.19.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2017.19.45.png)

#### 2.7.2.3mysqlbinlog

![截屏2024-04-22 17.21.26](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2017.21.26.png)

#### 2.7.2.4mysqlshow

![截屏2024-04-22 17.24.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2017.24.35.png)

**参数加到字段**

![截屏2024-04-22 17.26.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2017.26.53.png)

#### 2.7.2.5mysqldump

![截屏2024-04-22 17.28.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2017.28.30.png)

**备份数据库**

![截屏2024-04-22 18.12.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2018.12.55.png)

**输出参数的使用**

![截屏2024-04-22 18.15.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2018.15.05.png)

**-T可变参数的使用细节**

随便写一个目标地址，会报错，提示目标路径不安全，**要使用mysql认为安全(信任)的地址**

![截屏2024-04-22 18.17.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2018.17.40.png)

![截屏2024-04-22 18.19.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2018.19.02.png)

#### 2.7.2.6mysqlimport / source

![截屏2024-04-22 18.20.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2018.20.51.png)

**在mysql外**

![截屏2024-04-22 18.22.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2018.22.35.png)

**在mysql内执行**

![截屏2024-04-22 18.24.32](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2018.24.32.png)

# 3.MySQL运维

## 3.1MySQL日志

### 3.1.1错误日志

![截屏2024-04-22 20.20.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2020.20.35.png)

![截屏2024-04-22 20.22.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2020.22.08.png)

**实时查看文件尾部信息(做日志跟踪)**

![截屏2024-04-22 20.26.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2020.26.27.png)

### 3.1.2二进制日志

![截屏2024-04-22 20.28.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2020.28.01.png)

**日志文件后缀自增**

![截屏2024-04-22 20.32.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2020.32.50.png)

![截屏2024-04-22 20.33.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2020.33.44.png)



**在配置文件中修改日志格式**



![截屏2024-04-22 20.33.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2020.33.57.png)



**row 记录update操作的数据行，每一个update之前怎么样，之后怎么样**



**查看二进制日志文件**

![截屏2024-04-22 20.36.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2020.36.44.png)

**行日志要加v才能看见sql语句！**



**日志删除**

![截屏2024-04-22 20.40.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2020.40.53.png)

![截屏2024-04-22 20.42.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2020.42.25.png)

默认30天自动删除

![截屏2024-04-22 20.42.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2020.42.41.png)

### 3.1.3查询日志

![截屏2024-04-22 20.43.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2020.43.36.png)

![截屏2024-04-22 20.44.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2020.44.18.png)

### 3.1.4慢查询日志

![截屏2024-04-22 20.46.37](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2020.46.37.png)



![截屏2024-04-22 20.49.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2020.49.08.png)

## 3.2MySQL主从复制

### 3.2.1概述

![截屏2024-04-22 20.52.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2020.52.46.png)

**主库- Master**

**从库- Slave**

![截屏2024-04-22 20.53.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2020.53.31.png)

### 3.2.2原理

![截屏2024-04-22 20.55.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2020.55.36.png)

![截屏2024-04-22 20.55.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2020.55.55.png)



### 3.2.3搭建

![截屏2024-04-22 20.57.10](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2020.57.10.png)



#### 3.2.3.1主库配置

![截屏2024-04-22 21.02.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2021.02.30.png)

2 . 重启mysql服务器



![截屏2024-04-22 21.03.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2021.03.58.png)

![截屏2024-04-22 21.06.13](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2021.06.13.png)

#### 3.2.3.2从库配置

![截屏2024-04-22 21.07.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2021.07.49.png)

![截屏2024-04-22 21.09.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2021.09.00.png)



**从哪个配置文件的哪个位置开始读**

![截屏2024-04-22 21.09.24](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2021.09.24.png)

![截屏2024-04-22 21.10.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2021.10.18.png)

![截屏2024-04-22 21.11.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2021.11.55.png)

![截屏2024-04-22 21.12.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2021.12.23.png)

**关键参数- IO_Running 和 SQL_Running**

![截屏2024-04-22 21.13.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2021.13.11.png)

## 3.3MySQL分库分表

### 3.3.1简介

#### 3.3.1.1问题分析

![截屏2024-04-22 21.21.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2021.21.06.png)

![截屏2024-04-22 21.22.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2021.22.29.png)

**分库分表的中心思想都是将数据分散存储，使得单一数据库/表的数据量变小来缓解单一数据库的性能问题，从而达到提升数据库性能的目的**



#### 3.3.1.2**拆分策略**

![截屏2024-04-22 21.25.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2021.25.19.png)

#### 3.3.1.3垂直拆分

![截屏2024-04-22 21.28.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2021.28.29.png)

#### 3.3.1.4水平拆分

![截屏2024-04-22 21.30.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2021.30.04.png)

#### 3.3.1.5实现技术

![截屏2024-04-22 21.34.07](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2021.34.07.png)

### 3.3.2Mycat概述

安装jdk1.8并配置环境变量

![截屏2024-04-22 21.34.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2021.34.48.png)

mycat解压完后的文件目录

![截屏2024-04-23 11.44.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2011.44.23.png)

![截屏2024-04-23 11.45.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2011.45.17.png)

**我的mycat压根没有mysql的连接器jar包**

![截屏2024-04-23 11.49.33](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2011.49.33.png)

我把jar包放上去了，**不要忘记修改权限**



**概念介绍**

![截屏2024-04-23 12.04.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2012.04.51.png)

### 3.3.3Mycat入门

#### 3.3.3.1需求

![截屏2024-04-23 12.07.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2012.07.16.png)

#### 3.3.3.2环境准备

**装好mysql**

**关闭防火墙或者开放端口**

![截屏2024-04-23 12.08.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2012.08.23.png)

#### 3.3.3.3.分片配置(schema)

![截屏2024-04-23 12.11.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2012.11.16.png)

**配置文件在mycat的conf目录下**

![截屏2024-04-23 12.12.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2012.12.22.png)

修改数据节点主机名字，和关联数据库

![截屏2024-04-23 12.20.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2012.20.45.png)

配置数据节点的 dbDriver 为java

![截屏2024-04-23 12.18.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2012.18.47.png)

配置数据节点的数据库连接信息![截屏2024-04-23 12.18.37](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2012.18.37.png)

#### 3.3.3.4分片配置(server)

![截屏2024-04-23 12.21.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2012.21.44.png)

#### 3.3.3.5启动服务

![截屏2024-04-23 13.21.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2013.21.03.png)

#### 3.3.3.6连接mycat

![截屏2024-04-23 13.25.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2013.25.42.png)

#### 3.3.3.7分片规则rule

**在rule.xml中**

![截屏2024-04-23 13.30.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2013.30.22.png)

这是根据id判断该存在哪个表(会有一个txt文件记录规则)

algorithm也是一个引用，可以继续查看，java类

如果存入的索引过大了(每个节点数据库都装到指定大小了)，需要增加节点，以可以存入更多的数据

![截屏2024-04-23 13.47.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2013.47.20.png)





### 3.3.4Mycat配置

#### 3.3.4.1schema.xml

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2013.37.20.png" alt="截屏2024-04-23 13.37.20" style="zoom:50%;" />

**schema标签**

![截屏2024-04-23 13.37.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2013.37.50.png)

当checkSQLschema为true

**可以在不指定数据库的时候这么写，因为在执行的时候吧数据库名称去除，但是已经完成了数据库的定位**

![截屏2024-04-23 13.40.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2013.40.16.png)

![截屏2024-04-23 13.41.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2013.41.50.png)

**dataNode标签**

![截屏2024-04-23 13.44.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2013.44.17.png)

**dataHost标签**

![截屏2024-04-23 13.45.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2013.45.00.png)

#### 3.3.4.2rule.xml

![截屏2024-04-23 13.48.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2013.48.05.png)

```
column中为用来做判断的字段

algoritm为分页函数算法

函数中指定 提供分片处理的java类 和 所关联的属性配置
```

#### 3.3.4.3server.xml

![截屏2024-04-23 13.51.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2013.51.55.png)

**user标签**

![截屏2024-04-23 13.53.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2013.53.51.png)

### 3.3.5Mycat分片

#### 3.3.5.1垂直拆分

##### **3.3.5.1.1场景问题**

![截屏2024-04-23 14.02.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2014.02.49.png)

**配置服务器**

![截屏2024-04-23 14.04.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2014.04.12.png)

**配置配置文件**

![截屏2024-04-23 14.05.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2014.05.05.png)



![截屏2024-04-23 14.09.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2014.09.44.png)



配置完配置文件，那个shopping数据库中应该是没有内容的，是逻辑表结构，因为在配置文件中申明了，需要手动创建(节点数据库也会创建)

##### **3.3.5.1.2多表联查,全局表配置**

**在同一个节点下的数据表多表联查不会出问题，但是如果几张表在不同的节点数据库中，多表查询会出现问题**

sql底层的路由定位会出问题，不找到改找哪个节点

![截屏2024-04-23 14.21.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2014.21.17.png)

**配置**

修改dataNode和type

![截屏2024-04-23 14.22.07](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2014.22.07.png)

跟新全局表（对于mycat），关联节点中的都会变

#### 3.3.5.2水平拆分

![截屏2024-04-23 14.27.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2014.27.09.png)

**配置**

水平分要指定rule了

**dataNode配置中，name和database要换，但是datahost不用换**

![截屏2024-04-23 14.28.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2014.28.17.png)

![截屏2024-04-23 14.31.13](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2014.31.13.png)

![截屏2024-04-23 14.31.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2014.31.29.png)

**根据主键id求模**（因为我现在是3个节点，id % 3只会有三种情况，正好对应三个数据库）

#### 3.3.5.3范围分片

![截屏2024-04-23 14.41.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2014.41.49.png)



![截屏2024-04-23 14.42.13](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2014.42.13.png)

#### 3.3.5.4取模分片

![截屏2024-04-23 14.44.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2014.44.19.png)

![截屏2024-04-23 14.44.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2014.44.41.png)

#### 3.3.5.5一致性hash算法

![截屏2024-04-23 14.46.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2014.46.46.png)



![截屏2024-04-23 14.46.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2014.46.58.png)

**要配置count**

#### 3.3.5.6枚举分片

![截屏2024-04-23 14.51.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2014.51.08.png)

这里的规则是自定义的，不使用原来的名字

![截屏2024-04-23 14.51.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2014.51.27.png)



![截屏2024-04-23 14.53.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2014.53.54.png)

**替换column字段**

**并根据设计好的字段的枚举情况修改 外部txt文件**

配置默认节点。。。

#### 3.3.5.7应用指定算法

![截屏2024-04-23 15.00.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2015.00.31.png)

![截屏2024-04-23 15.00.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2015.00.50.png)

**这个分片规则默认没有，自己添加**

#### 3.3.5.8固定hash算法

**10位与1111111111运算得到的结果一定在0-1023之间**

![截屏2024-04-23 15.10.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2015.10.25.png)

**三个节点的范围可以均匀也可以不均匀**

![截屏2024-04-23 15.11.32](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2015.11.32.png)

**2 * 256 +  1* 512 = 1024(默认值)**

![截屏2024-04-23 15.13.38](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2015.13.38.png)

#### 3.3.5.9字符串hash解析

​	

![截屏2024-04-23 15.20.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2015.20.17.png)



![截屏2024-04-23 15.20.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2015.20.41.png)

![截屏2024-04-23 15.23.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2015.23.18.png)

**这个函数需要自己定义**

#### 3.3.5.10按天分片

![截屏2024-04-23 15.40.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2015.40.03.png)

![截屏2024-04-23 15.40.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2015.40.22.png)

![截屏2024-04-23 15.41.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2015.41.16.png)

#### 3.3.5.11按自然月分片

**按照月份来分片，每个自然月为一个分片**



**超过了指定月份，是进入循环，从头插入**

![截屏2024-04-23 15.46.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2015.46.09.png)

![截屏2024-04-23 15.46.37](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2015.46.37.png)

 如果数量不一致会报错

![截屏2024-04-23 15.51.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2015.51.12.png)

### 3.3.6Mycat管理及监控

#### 3.3.6.1mycat原理

![截屏2024-04-23 16.25.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2016.25.39.png)

#### 3.3.6.2管理工具

![截屏2024-04-23 16.28.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2016.28.29.png)



![截屏2024-04-23 16.29.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2016.29.17.png)

#### 3.3.6.3Mycat-eye

![截屏2024-04-23 16.34.26](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2016.34.26.png)



http://172.16.14.130:8082/mycat/

![截屏2024-04-23 17.56.52](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2017.56.52.png)

![截屏2024-04-23 17.58.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2017.58.36.png)

## 3.4MySQL读写分离

### 3.4.1介绍

![截屏2024-04-23 16.47.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2016.47.43.png)

#### 3.4.2一主一从

**基于二进制日志**

![截屏2024-04-22 20.55.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-22%2020.55.36.png)

#### 3.4.3一主一从读写分离

![截屏2024-04-23 16.57.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2016.57.05.png)

读写分离中可以不指定逻辑表



**balance - 负载均衡**

![截屏2024-04-23 16.59.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2016.59.01.png)

**对于一主一从 1， 3没区别**



**当主节点Master宕机后，业务瘫痪，不能操作数据，只能读取数据**

#### 3.4.4双主双从

![截屏2024-04-23 17.09.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2017.09.15.png)

![截屏2024-04-23 17.09.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2017.09.35.png)

**搭建环境**

![截屏2024-04-23 17.11.13](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2017.11.13.png)

![截屏2024-04-23 17.14.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2017.14.04.png)

![截屏2024-04-23 17.17.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2017.17.49.png)

![截屏2024-04-23 17.18.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2017.18.48.png)

![截屏2024-04-23 17.21.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2017.21.53.png)

#### 3.4.5双主双从读写分离

![截屏2024-04-23 17.26.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2017.26.00.png)

![截屏2024-04-23 17.28.21](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-23%2017.28.21.png)

**自动切换指的是，一个writehost没了后会不会自动换到别的writehost去**
