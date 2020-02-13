---
title: Docker使用
tags:
- Docker
---

# 概念

+ Docker主机: 安装了Docker程序的机器
+ Docker客户端: 连接Docker主机进行操作
+ Docker仓库: 用来保存各种打包好的软件镜像
+ Docker镜像: 软件打包好的镜像, 放在Docker仓库中
+ Docker容器: 镜像启动后的实例成为一个容器; 容器是独立运行的一个或一组应用

<!--more-->

# 安装docker

```bash
# 安装必要的工具
sudo yum install -y yum-utils device-mapper-persistent-data lvm2

# 修改源
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# 更新yum缓存
sudo yum makecache fast

# 安装docker-ce
sudo yum install -y docker-ce
```

卸载
```bash
sudo yum remove docker-ce

# 默认将镜像安装在此处
sudo rm -rf /var/lib/docker
```

## Docker加速器

在/etc/docker/下新建daemon.json

写入一下内容:

镜像根据自己阿里云镜像加速器的填写:
https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors

```json
{
    "registry-mirrors":["https://registry.docker-cn.com"]
}
```

重启Docker
```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

该命令可以查看镜像配置:
```bash
docker info
```

# Docker的使用

使用Docker的步骤:
1. 安装Docker
2. 去Docker仓库找到这个软件对应的镜像
3. 使用Docker运行这个镜像, 这个镜像就会生成一个Docker容器
4. 对容器的启动停止就是对软件的启动停止

+ 启动docker

```bash
systemctl start docker
```

```bash
# 开机启动docker 
systemctl enable docker

# 关闭开机启动docker 
systemctl disable docker
```

+ 停止docker

```bash
systemctl stop docker
```

# 常用操作

1. 检索

docker search 关键字

```bash
docker search mysql
```

2. 拉取

```bash
docker pull 镜像名:tag
```

:tag 是可选的, 表示标签, 多为软件的版本, 默认是latest

3. 列表

查看所有本地的镜像

```bash
docker images
```

4. 删除

删除指定的本地镜像

```bash
docker rmi image-id
```

eg:
```bash
docker rmi aeea3708743f
```

# 容器操作

1. 运行

```bash
docker run --name container-name -d image-name
```

+ --name: 自定义容器名
+ -d: 后台运行
+ image-name: 指定镜像模板
+ --rm: 关闭容器后就删除该容器

eg:
```bash
docker run --name myTomcat -d tomcat:latest
```

1. 列表

查看运行中的容器

```bash
docker ps (-a)
```

+ 加上-a可以查看所有容器

3. 停止

停止当前运行的容器

```bash
docker stop container-name/container-id
```

eg:
```bash
docker stop 8918978dc7f5
或
docker stop myTomcat
```

4. 启动

启动容器

```bash
docker start container-name/container-id
```

5. 删除容器

```bash
docker rm container-id
```

6. 端口映射

-p: 主机端口映射到容器内部的端口

```bash
docker run -d -p 6379:6380 --name myredis docker.io/redis
```

7. 容器日志

```bash
docker logs container-name/container-id
```

+ -f: 查看实时日志
+ -t: 查看日志产生的日期
+ --tail=10: 查看最后的10条日志
+ --since: 输出指定日期之后的日志

--since "2020-02-01"(该日期之后的日志)
--since 30m(30分钟后的日志)

```bash
docker logs -f -t --since="2020-02-01" --tail=10 882e80252d57
```

1. 宿主机和容器之间交换文件

宿主机中的文件复制到容器
```bash
docker cp /home/dal/document/1.html tomcat-test:/usr/local/tomcat/webapps
```

容器中的文件复制到宿主机中
```bash
docker cp tomcat-test:/usr/local/tomcat/webapps/1.html /home/dal/
```

# Docker进入容器

```bash
docker exec -it 容器ID/name bash
```

# 安装mysql

下载mysql
```bash
docker pull mysql
```

运行, 指定映射端口和mysql密码
```bash
docker run -p 8806:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql
```

其他几个高级操作

将主机的/conf/mysql文件夹挂载到docker容器的/etc/mysql/conf.d文件夹, 修改mysql的配置文件只需要将配置文件放在/conf/mysql中
```bash
docker run -p 8806:3306 -v /conf/mysql:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=123456 -d mysql
```

# nginx

1. 安装
```bash
docker pull nginx
```

2. 运行

```bash
docker run -d --name nginx-test -p 80:80 nginx 
```

# 一些问题

1. tomcat首页404

进入tomcat目录
```bash
docker exec -it 运行的tomcat容器ID bash
```

将webapps.dist内容放到webapps中即可

```bash
mv webapps.dist/* webapps
```

2. 虚悬镜像

由于版本迭代导致旧版本的标签为<none>, 此时可以删除该镜像

命令:
```bash
docker image prune
```

3. 镜像无法删除

```bash
rm -rf /var/lib/docker
```

# Docker数据卷

+ 数据卷可以在容器之间共享和重用, 容器间传递数据将变得高效方便
+ 对数据卷内数据的修改会立马生效, 无论是容器内操作还是本地操作
+ 对数据卷的更新不会影响镜像, 解耦了应用和数据
+ 卷会一直存在, 直到没有容器使用, 可以安全地卸载它

## 创建方法

1. 创建一个数据卷

```bash
docker volume create vol-1
```

2. 查看数据卷

```bash
docker volumn ls
```

3. 将数据文件放在主机的某个文件夹下

```bash
cp 1.html /home/dal/document
```

4. 根据镜像启动容器, 并挂载数据卷

```bash
docker run --rm -d --name tomcat-test -p 8888:8080 -v /home/dal/document/:/usr/local/tomcat/webapps/document/ tomcat
```

将 /home/dal/document 挂载到 tomcat容器的 /usr/local/tomcat/webapps/document 目录下
-v /home/dal/document/:/usr/local/tomcat/webapps/document/

# Dockerfile定制镜像

1. FROM 

指定镜像, 镜像不存在时会自动从Docker拉取

```bash
FROM <镜像>
```

2. LABEL

制作人信息

```bash
LABEL "zhangsan <123@qq.com>"
```

3. RUN

执行命令行命令

```bash
RUN <命令行命令>
```

4. COPY

复制文件

```bash
COPY 源路径1 源路径2
```

eg:
```bash
COPY /home/dal/document/1.jpg /usr/local/tomcat/webapps/ROOT/
```

5. CMD

运行程序

+ CMD: 在docker run时运行
+ RUN: 在docker build时运行

```bash
CMD 命令
```

6. ENV

设置环境变量

```bash
ENV <key> <value>
```

```bash
ENV JAVA_HOME /usr/local/jdk
```

7. VOLUME

定义匿名数据卷

```bash
VOLUME <路径>
```

8. EXPOSE

声明端口

```bash
EXPOSE <端口1>
```

9. WORKDIR

指定工作目录

```bash
WORKDIR <工作目录>
```

10. ENTRYPOINT

执行指令
```bash
ENTRYPOINT ["<executeable>","<param1>","<param2>",...]
```


11. 构建镜像

```bash
docker build -t tomcat-test .
```

+ -t: 指定要创建的目标镜像名
+ .: Dockerfile文件所在的目录, 可以指定Dockerfile的绝对路径

## 案例一

创建一个tomcat镜像, 向其中的index.html文件写入"hello, world"

1. 新建一个文件夹

```bash
mkdir docker
```

2. 新建Dockfile文件

```bash
FROM tomcat

WORKDIR /usr/local/tomcat
RUN cp -r webapps.dist/* webapps
RUN echo "hello, world" > webapps/ROOT/index.html
```

切换目录
```bash
WORKDIR /usr/local/tomcat
```

将webapps.dist中的内容复制到webapps中
```bash
RUN cp -r webapps.dist/* webapps
```

将"hello, world"覆盖index.html文件内容
```bash
RUN echo "hello, world" > webapps/ROOT/index.html
```

3. 在当前文件夹下构建镜像

```bash
docker build -t tomcat-test .
```

4. 查看镜像

```bash
docker images
```

![](docker1.png)

5. 运行镜像

```bash
docker run -d --name tomcat-01 --rm -p 8888:8080 tomcat-test
```

![](docker2.png)

## 案例二

利用Dockerfile将一个war包部署到Docker

1. 新建文件夹, 并将test.zip复制到该目录下

```bash
mkdir docker
cp test.zip /home/dal/document/docker/
```

2. 创建镜像文件Dockfile
 
```bash
FROM tomcat

WORKDIR /usr/local/tomcat/webapps/ROOT
COPY test.zip .
RUN unzip test.zip
RUN rm test.zip
```

将/usr/local/tomcat/webapps/ROOT作为工作目录, 然后使用COPY命令复制test.zip到工作目录下, 之后解压文件, 最后将该压缩包删除

3. 构建镜像

```bash
docker build -t tomcat-test .
```

4. 镜像版本迭代会产生<none>, 将其删除

```bash
docker images

docker image prune
```

5. 运行镜像

```bash
docker run --name tomcat-01 -d -p 8888:8080 --rm tomcat-test
```

# idea集成Docker

1. Docker开启远程访问

docker默认不支持远程访问

修改/lib/systemd/system/docker.service的ExecStart
```bash
ExecStart=/usr/bin/dockerd -H tcp://0.0.0:2375 -H unix:///var/run/docker.sock
```

```bash
# 重新加载配置文件
systemctl daemon-reload
# 重启服务
systemctl restart docker.service
# 查看端口是否开启
netstat -nlpt
# curl查看是否生效
curl http://127.0.0.1:2375/info
```

2. idea连接docker

在setting中找到Docker, 然后修改Engine API URL为:
tcp://192.168.1.1:2375

下方出现Connection successful则表示成功

Registry的Address设置为阿里云加速器地址

3. 导入依赖, 编写配置文件

pom.xml
```xml
<properties>
    <java.version>1.8</java.version>
    <docker.image.prefix>gydjsz</docker.image.prefix>
</properties>
```

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
        <plugin>
            <groupId>com.spotify</groupId>
            <artifactId>docker-maven-plugin</artifactId>
            <version>1.2.2</version>
            <configuration>
                <!-- 镜像名称(该命令的springboot) docker build -t springboot . -->
                <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
                <!-- 指定标签 -->
                <imageTags>
                    <imageTag>latest</imageTag>
                </imageTags>
                <!-- 基础镜像 -->
                <baseImage>java</baseImage>
                <!-- 提供作者本人的信息 -->
                <maintainer>gydjsz 123@qq.com</maintainer>
                <!-- 切换到ROOT目录 -->
                <workdir>/ROOT</workdir>
                <cmd>["java", "-version"]</cmd>
                <entryPoint>["java", "-jar", "${project.build.finalName}.jar"]</entryPoint>

                <!-- 指定Dockerfile路径 -->
                    <dockerDirectory>${project.basedir}/src/main/docker</dockerDirectory>-->
                <!-- 指定远程docker地址 -->
                <dockerHost>http://47.93.238.102:2375</dockerHost>
                <!-- 复制jar包到docker容器的指定目录配置 -->
                <resources>
                    <resource>
                        <targetPath>/ROOT</targetPath>
                        <!-- 用于指定需要复制的目录, 该处表示target目录-->
                        <directory>${project.build.directory}</directory>
                        <!-- 用于指定要复制的文件-->
                        <include>${project.build.finalName}.jar</include>
                    </resource>
                </resources>
            </configuration>
        </plugin>
    </plugins>
</build>
```

上述配置使用docker-maven插件自动生成Dockerfile, 
内容:
```bash
FROM java
MAINTAINER gydjsz 123@qq.com
WORKDIR /ROOT
ADD /ROOT/springboot-01-0.0.1-SNAPSHOT.jar /ROOT/
ENTRYPOINT ["java", "-jar", "springboot-01-0.0.1-SNAPSHOT.jar"]
CMD ["java", "-version"]
```

4. 编写java代码

HelloController.java
```java
@RestController
public class HelloController {

    @RequestMapping("/hello")
    public String hello(){
        return "hello";
    }
}

```

5. 执行命令打包部署

```bash
mvn clean package docker:build
```

![](docker3.png)

6. 运行docker插件

![](docker4.png)

根据镜像创建容器

![](docker5.png)

配置命令
![](docker6.png)

最后生成一个容器

![](docker7.png)

7. 查看结果

![](docker8.png)

## idea集成Docker扩展

打包时自动创建镜像

在docker-maven插件的configuration标签后添加如下配置
```xml
<executions>
    <execution>
        <id>build-image</id>
        <phase>package</phase>
        <goals>
            <goal>build</goal>
        </goals>
    </execution>
</executions>
```

点击maven的package按钮就可以执行打包并发布镜像

由于新镜像的发布, 旧镜像变为虚悬镜像, 可以直接删除
