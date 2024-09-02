**快速构建，运行，管理应用的工具**

# 1.快速入门

## 1.1安装Docker

**官方安装文档**

https://docs.docker.com/engine/install/ubuntu/

**配置镜像加速器**

登录阿里云，控制台，找到容器镜像服务，找到镜像加速器

https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors

![截屏2024-05-22 20.40.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-22%2020.40.34.png)

## 1.2部署Mysql

![截屏2024-05-22 21.05.21](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-22%2021.05.21.png)

![截屏2024-05-22 21.07.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-22%2021.07.19.png)

当我们使用docker安装应用的时候，docker会自动搜索并下载应用**镜像**(image)

镜像不尽包含应用本身，**还包含应用运行所需要的环境，配置，系统函数库。** ---- **实现跨版本运行**

Docker会在运行镜像时创建一个隔离环境，称为**容器**(container) --- 与其他进程相呼隔离 不干扰 --- **可以在一个服务器上部署多个应用**

(如mysql，按照不同的端口再启动一下就是一个新的应用)

**容器模拟一个系统运行的环境**

![截屏2024-05-22 21.16.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-22%2021.16.20.png)



## 1.3命令解读

mysql隔离容器，对外无法访问，通过端口映射访问(前一个为宿主机端口，后一个为容器内端口)

环境变量的内容是这个镜像本身决定(需要什么)

![截屏2024-05-23 08.45.31](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2008.45.31.png)

![截屏2024-05-23 08.46.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2008.46.02.png)

# 2.Docker基础

## 2.1常用命令

**官方文档**

https://docs.docker.com/reference/

![截屏2024-05-23 08.50.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2008.50.54.png)

- 在DockerHub中搜索Nginx镜像，查看镜像名称
- 拉取Nginx镜像 - **docker pull nginx**

- 查看本地镜像列表 - **docker images**

![截屏2024-05-23 08.57.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2008.57.17.png)

- 保存镜像 - **docker save -o nginx.tar nginx:latest**

![截屏2024-05-23 08.59.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2008.59.06.png)

- 删除镜像 - **docker rmi nginx:latest**

![截屏2024-05-23 09.00.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2009.00.18.png)

- 运行本地镜像 - **docker load -i nginx (-q)**

**-q 不做输出**

![截屏2024-05-23 09.02.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2009.02.00.png)

- 创建并运行Nginx容器 - **docker run -d --name nginx -p 80:80 nginx**

- 查看容器 

**docker ps**

查看的是运行中的容器

![截屏2024-05-23 09.05.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2009.05.15.png)

**docker ps -a** 

代表查看所有的容器(不管是否运行中)

- 停止容器 - **docker stop nginx**

- 再次启动容器 - **docker start nginx**

- 查看指定容器日志 - **docker logs nginx**

**docker logs -f nginx** (follow，一直不断输出日志)

- 进入Nginx容器 - **docker exec -it nginx bash**   **exec**(执行) **-it(**面对隔离环境，添加一个可交互终端)   **bash**(使用命令行进行交互)
- 删除容器 - **docker rm mysql2** 

直接删需要先停止mysql2 容器

**docker rm -f mysql ** (强制删除)



```linux
vi ～/.bashrc //进入命别别名的文件
```

**linux中的命令别名**

![截屏2024-05-23 09.16.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2009.16.39.png)

```linux
source ~/.bashrc //使配置文件生效
```





## 2.2数据卷

### 2.2.1数据卷挂载

**虽然容器模拟一个系统运行的环境**

**但是只包含当前镜像所对应的应用运行必备的系统函数**

这导致进入容器后要想修改一个静态资源非常困难，如html,无法使用vi等命令

**通过数据卷来做映射，通过改变宿主机中的文件来自动改变容器中的文件**

![截屏2024-05-23 09.25.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2009.25.56.png)



**相关命令**

![截屏2024-05-23 09.26.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2009.26.57.png)

**已创建的容器不能再挂载**

![截屏2024-05-23 09.28.18](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2009.28.18.png)

![截屏2024-05-23 09.29.48](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2009.29.48.png)

![截屏2024-05-23 09.32.57](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2009.32.57.png)

### 2.2.2本地目录挂载

```
docker inspect nginx // 查看nginx的挂载情况
```

**不是自己创建的卷，容器运行时自动创建的卷，叫做匿名卷，名字自动生成(名字复杂，不方便迁移)**

mysql默认对数据存储的文件夹做了挂载，方便后续解耦

![截屏2024-05-23 09.46.00](/Users/sunnyday/Library/Application Support/typora-user-images/截屏2024-05-23 09.46.00.png)

**就算删除了mysql的某个容器，后面使用本地目录挂载，数据依然存在 - 实现数据持久化**

## 2.3自定义镜像

![截屏2024-05-23 09.54.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2009.54.08.png)

![截屏2024-05-23 10.04.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2010.04.39.png)

**镜像是一层层的，一个镜像有很多模块的压缩包组成**

**不同的镜像可以共享一些基础镜像**

**分层的结构，也方便镜像的部分管理和修改**

![截屏2024-05-23 10.04.03](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2010.04.03.png)

![截屏2024-05-23 10.05.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2010.05.15.png)

![截屏2024-05-23 10.09.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2010.09.25.png)

![截屏2024-05-23 10.15.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2010.15.12.png)



**先下载基础镜像**

**注意dockerfile中用的是相对路径，在buile的时候使用 . 为当前路径，所以要把dockerfile和jar包放在一起![截屏2024-05-23 10.20.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2010.20.39.png)**

## 2.4容器网络 

**当一个容器关闭，另外一个容器开启，可能之前那个容器的ip地址就会发生变化**

![截屏2024-05-23 10.24.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2010.24.17.png)



![截屏2024-05-23 10.29.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2010.29.04.png)

**启动时加入网桥，就不会加入默认的网桥**

![截屏2024-05-23 10.27.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2010.27.47.png)

# 3.项目部署

## 3.1部署前端

![截屏2024-05-23 10.36.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2010.36.30.png)

![截屏2024-05-23 10.42.59](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2010.42.59.png)







## 3.2部署Java

![截屏2024-05-23 10.34.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2010.34.12.png)

## 3.3DockerCompose

![截屏2024-05-23 10.47.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2010.47.09.png)

![截屏2024-05-23 10.49.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2010.49.09.png)

**依赖mysql，将来创建容器的时候会先创建mysql容器**

![截屏2024-05-23 10.50.36](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2010.50.36.png)

**还有自动创建网络**

![截屏2024-05-23 10.52.14](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-05-23%2010.52.14.png)