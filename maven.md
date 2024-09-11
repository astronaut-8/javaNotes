<u>*MAVEN*</u>

**2024.9.3重学maven**

# maven基础

## maven简介

![截屏2024-09-03 22.10.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-03%2022.10.28.png)

 ![截屏2024-09-03 22.13.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-03%2022.13.44.png)

![截屏2024-09-03 22.15.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-03%2022.15.04.png)

## 下载与安装

![截屏2024-09-03 22.17.52](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-03%2022.17.52.png)

![截屏2024-09-03 22.18.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-03%2022.18.23.png)

## maven基础概念

### 仓库

![截屏2024-09-03 22.21.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-03%2022.21.50.png)

![截屏2024-09-03 22.22.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-03%2022.22.04.png)

### 坐标

![截屏2024-09-03 22.26.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-03%2022.26.56.png)

### 配置仓库

关于maven的配置文件

- *maven/conf/setting.xml 为maven的全局配置文件*

- *在多用户环境中（如共享服务器），Maven 的配置和缓存会基于每个用户的个人文件夹来进行分离*

- *每个用户在自己的 用户名/.m2/setting.xml中可以设置自己个性化的配置 默认的每个用户的本地仓库也是在这里的*

找到maven文件夹修改配置文件

![截屏2024-09-03 22.29.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-03%2022.29.18.png)

![截屏2024-09-03 22.34.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-03%2022.34.53.png)

repositories可以配置多个仓库 按照从上到下的顺序查找依赖(只在本地仓库没有的情况下)

![截屏2024-09-03 22.36.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-03%2022.36.19.png)

镜像是对仓库的映射

![截屏2024-09-03 22.37.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-03%2022.37.02.png)

![截屏2024-09-03 22.44.36](/Users/sunnyday/Desktop/截屏2024-09-03 22.44.36.png)

**个人认为 实现公司私服 可以配置私服仓库 也可以使用mirror仓库映射实现**

![截屏2024-09-06 21.16.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-06%2021.16.56.png)

## maven项目

### maven工程目录结构

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-06%2021.39.04.png" alt="截屏2024-09-06 21.39.04" style="zoom:33%;" />

![截屏2024-09-06 21.21.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-06%2021.21.53.png)

### 手工构建maven项目命令

**pom文件所在的层级下执行命令**

![截屏2024-09-06 21.25.35](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-06%2021.25.35.png)

- **compile 编译命令会在pom层级下产生target目录用于存放编译后的文件**
- **test 完成后也会在target中产生一个文件夹，其中的文件记录的测试的过程结果的详细信息**

- **package 打包前会先编译和测试**
- **install 会根据groupId和artifactId在本地仓库中加入你的项目**

### 插件创建工程

![截屏2024-09-06 21.35.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-06%2021.35.49.png)

### IDEA生成

**web工程增加tomcat插件(可以在启动选项中配置)**

![截屏2024-09-06 21.50.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-06%2021.50.27.png)

## 依赖管理

### 依赖配置

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-06%2022.05.42.png" alt="截屏2024-09-06 22.05.42" style="zoom:33%;" />

### 依赖传递

![截屏2024-09-06 22.08.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-06%2022.08.25.png)

![截屏2024-09-06 22.10.13](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-06%2022.10.13.png)

### 可选依赖

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-06%2022.12.20.png" alt="截屏2024-09-06 22.12.20" style="zoom: 50%;" />

### 排除依赖

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-06%2022.13.48.png" alt="截屏2024-09-06 22.13.48" style="zoom: 50%;" />

### 依赖范围

![截屏2024-09-06 22.17.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-06%2022.17.03.png)

![截屏2024-09-06 22.20.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-06%2022.20.58.png)

## 生命周期与插件

### 构建生命周期

![截屏2024-09-06 22.23.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-06%2022.23.05.png)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-06%2022.23.51.png" alt="截屏2024-09-06 22.23.51" style="zoom:67%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-06%2022.24.08.png" alt="截屏2024-09-06 22.24.08" style="zoom: 67%;" />

![截屏2024-09-06 22.24.38](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-06%2022.24.38.png)

![截屏2024-09-06 22.24.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-06%2022.24.51.png)



#### 插件

![截屏2024-09-06 22.25.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-06%2022.25.56.png)

可以自己加入插件 **绑定生命周期** 指定参数 进行对应的操作

**目标（Goal）**：每个插件可以有多个目标，每个目标代表一个插件的具体任务。例如，`maven-compiler-plugin` 插件有一个 `compile` 目标用于编译代码，`maven-surefire-plugin` 插件有一个 `test` 目标用于运行测试。

![截屏2024-09-06 22.30.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-06%2022.30.47.png)

# maven高级

## 分模块开发与设计

![截屏2024-09-11 09.39.52](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2009.39.52.png)

**相互引用自己的资源**

要先install要导入的资源，不然不能在仓库中被找到

![截屏2024-09-11 09.47.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2009.47.01.png)

![截屏2024-09-11 09.50.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2009.50.41.png)

![截屏2024-09-11 09.58.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2009.58.16.png)

controller中使用多个配置文件

![截屏2024-09-11 10.01.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2010.01.56.png)

![截屏2024-09-11 10.03.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2010.03.25.png)

![截屏2024-09-11 10.03.58](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2010.03.58.png)

## 聚合

![截屏2024-09-11 10.09.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2010.09.11.png)





**一个聚合工程用来统一管理多个模块**

![截屏2024-09-11 10.13.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2010.13.39.png)

## 继承

![截屏2024-09-11 10.15.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2010.15.56.png)



在root pom文件中做统一的依赖管理 注意根标签发生变化

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2010.25.17.png" alt="截屏2024-09-11 10.25.17" style="zoom:67%;" />



**在子工程中定义继承的父工程**

**为和父工程保持一致 子工程省略 groupId 和 version**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2010.27.49.png" alt="截屏2024-09-11 10.27.49" style="zoom:50%;" />

**子工程中使用在父工程中定义的依赖时，不写version引用父工程**

![截屏2024-09-11 10.36.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2010.36.44.png)

![截屏2024-09-11 10.37.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2010.37.20.png)

![截屏2024-09-11 10.38.05](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2010.38.05.png)

## 属性

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2010.42.34.png" alt="截屏2024-09-11 10.42.34" style="zoom: 50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2010.43.15.png" alt="截屏2024-09-11 10.43.15" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2010.43.43.png" alt="截屏2024-09-11 10.43.43" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2010.44.11.png" alt="截屏2024-09-11 10.44.11" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2010.45.13.png" alt="截屏2024-09-11 10.45.13" style="zoom:50%;" />

## 版本管理

![截屏2024-09-11 10.46.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2010.46.41.png)

![截屏2024-09-11 10.49.23](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2010.49.23.png)



## 资源配置

项目程序中去引用pom root 中的属性配置

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2011.09.10.png" alt="截屏2024-09-11 11.09.10" style="zoom:50%;" />

**对于test中的配置文件 标签为testResources**

**${project.basedir}为所有的模块**

![截屏2024-09-11 11.11.37](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2011.11.37.png)

## 多环境开发配置

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2011.12.45.png" alt="截屏2024-09-11 11.12.45" style="zoom: 25%;" />



**在root pom文件中**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2019.07.54.png" alt="截屏2024-09-11 19.07.54" style="zoom:50%;" />

**在启动项中去增加指定环境参数**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2019.08.32.png" alt="截屏2024-09-11 19.08.32" style="zoom:33%;" />

## 跳过测试

**跳过测试的一些场景**

- 整体模块功能没开发
- 模块中某个功能未开发完毕
- 单个功能更新导致其他功能失效
- 快速打包



<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2019.20.24.png" alt="截屏2024-09-11 19.20.24" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2019.20.44.png" alt="截屏2024-09-11 19.20.44" style="zoom:50%;" />



<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2019.21.11.png" alt="截屏2024-09-11 19.21.11" style="zoom:50%;" />



## 私服

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2019.43.45.png" alt="截屏2024-09-11 19.43.45" style="zoom:50%;" />

### nexus服务器

https://help.sonatype.com/en/download.html

![截屏2024-09-11 19.48.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2019.48.06.png)



<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2019.58.42.png" alt="截屏2024-09-11 19.58.42" style="zoom:50%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2019.58.52.png" alt="截屏2024-09-11 19.58.52" style="zoom:50%;" />

**启动不了 不知道为什么🤷**

### 仓库分类和手动上传组件



<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2020.32.36.png" alt="截屏2024-09-11 20.32.36" style="zoom:33%;" />

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2020.33.06.png" alt="截屏2024-09-11 20.33.06" style="zoom:33%;" />

![截屏2024-09-11 20.35.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2020.35.36.png)

编辑成员

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2020.36.29.png" alt="截屏2024-09-11 20.36.29" style="zoom:50%;" />





### 本地仓库访问私服

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2020.41.37.png" alt="截屏2024-09-11 20.41.37" style="zoom:25%;" />

**在maven的配置文件中配置私服服务器信息**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2020.43.58.png" alt="截屏2024-09-11 20.43.58" style="zoom: 33%;" />

将mirror映射到自己的**群组**

![截屏2024-09-11 20.46.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2020.46.11.png)



### idea访问私服和组件上传

![截屏2024-09-11 20.49.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-09-11%2020.49.55.png)

**当发布一个资源 以release为例 访问url 在本地配置文件中寻找id对应server的信息去做登录**

