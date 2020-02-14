---
title: nginx学习
tags:
- nginx
---

# 介绍

nginx是一个高性能的HTTP和反向代理服务器

<!--more-->

## 安装

1. 安装依赖

```bash
yum -y install make zlib zlib-devel gcc-c++ libtool  openssl openssl-devel

# 下载pcre.tar.gz
tar -zxvf pcre.tar.gz
cd pcre
./configure
make & make install

# 下载nginx.tar.gz
tar -zxvf nginx.tar.gz
cd nginx
./configure
make & make install
```

## 常用命令

启动
```bash
/usr/sbin/nginx
```

关闭
```bash
nginx -s stop
```

重新载入配置文件
```bash
nginx -s reload
```

# nginx配置

配置文件位置: /etc/nginx

## 全局块

设置一些影响nginx服务器整体运行的配置指令, 主要包括配置运行nginx服务器的用户(组)、允许生成的worker process数, 进程PID存放路径、日志存放路径和类型以及配置文件的引入等

这个是nginx服务器并发处理任务的关键配置, worker_process值越大, 可以支持的并发处理量也越多, 一般为CPU数\*核数
```nginx
worker_processes 1;
```

## events块

events块涉及的指令主要影响nginx服务器与用户的网络连接, 常用的设置包括是否开启对多worker process下的网络连接进行序列化, 是否允许同时接收多个网络连接, 选取哪种事件驱动模型来处理连接请求, 每个work process可以同时支持的最大连接数等

worker process支持的最大连接数为1024
```nginx
worker_connections 1024;
```

## http全局块

http全局块配置的指令包括文件引入、MIME-TYPE定义、日志自定义、连接超时时间、单链接请求数上限等

代理使用
```nginx
server {
    listen 8080;
    server_name 192.169.9.211;

    location / {
        proxy_pass https://www.baidu.com;
    }
}

server {
    listen 8080;
    server_name 192.169.9.211;

    location / {
        root /home/resource; 
    }
}
```

# Rewrite语法

```nginx
if 空格(条件){
    重写模式
}
```

+ set: 设置变量
+ return: 返回状态码
+ break: 跳出rewrite
+ rewrite: 重写

条件:

1. =, 判断相等, 用于字符串比较
2. ~, 用正则匹配(区分大小写, ~*不分大小写)
3. -f, -d, -e来判断是否为文件, 为目录, 是否存在

例子:

```nginx
location / {
    if ($remote_addr = 192.168.9.1) {
        return 403;
    }

    if ($request_method = POST) {
        return 405;
    }
    
    # UA中包含msie, 则请求到 ie.html(不break会循环重定向)
    if ($http_user_agent ~ MSIE) {
        rewrite ^.*$ ie.html break;
    } 

    if ($http_user_agent ~ MSIE) {
        SET $flag = 1
    }

    if ($request_method = GET) {
        SET $flag = 2
    }

    if ($flag 2) {
        return 200;
    }
}
```

# 反向代理

## 实例一

在浏览器中输入47.93.238.102:8880, 跳转到linux系统tomcat主页面中, tomcat页面链接为47.93.238.102:8888

1. 启动tomcat
```bash
docker run --name tomcat-test -p 8888:8080 -d tomcat
```

2. 启动nginx
```bash
docker run --name nginx-test -p 8880:80 -d nginx
```

浏览器中输入
47.93.238.102:8880
和
47.93.238.102:8888

页面分别为:

![](nginx1.png)

![](nginx2.png)

3. 修改default.conf文件内容并覆盖docker容器中的文件

default.conf
```nginx
location / {
   root   /usr/share/nginx/html;
   index  index.html index.htm;
   # 添加这一行 
   proxy_pass http://47.93.238.102:8888;
}
```

```bash
docker cp default.conf nginx-test:/etc/nginx/conf.d/default.conf
```

4. 进入nginx容器并刷新配置

```bash
docker exec -it nginx-test bash
nginx -s reload
```

5. 查看结果

浏览器中输入:
47.93.238.102:8880

结果:
![](nginx3.png)

## 实例二

使用nginx反向代理, 根据访问的路径跳转到不同端口的服务中

1. 运行两个tomcat服务器

由于docker内部容器是独立的, 容器对内端口可以同为8080(耗时1h改端口才想起有这个特点)

第一个tomcat服务器使用8888端口, 

```bash
docker run --name tomcat-01 -p 8888:8080 --rm -d tomcat
```

第二个tomcat服务器使用8889端口

```bash
docker run --name tomcat-02 -p 8889:8080 --rm -d tomcat
```

添加页面
```bash
docker exec -it tomcat-02 bash
cp -r webapps.dist/* webapps 
cd webapps/ROOT
echo "hello, world" > index.jsp
```

此时一个tomact为主页面, 一个为hello, world

![](nginx2.png)

![](nginx4.png)

2. 在tomcat中添加两个页面文件

tomcat-01

webapps文件夹下新建一个edu文件夹, 并写入内容
```bash
mkdir edu
echo "tomcat 8888" > index.html
```

tomcat-02

webapps文件夹下新建一个vol文件夹, 并写入内容
```bash
mkdir vol 
echo "tomcat 8889" > index.html
```

3. 启动nginx并修改配置文件

此处映射两个端口, 并挂载文件夹, 方便修改
```bash
docker run --name nginx-test -p 8880:80 -p 8890:8000 -d --rm -v /home/dal/document/nginx/:/etc/nginx/conf.d/ nginx
```

proxy.conf

```nginx
server {
    listen       8000;
    server_name  localhost;

        location ~ /edu/ {
            proxy_pass http://47.93.238.102:8888;
        }

        location ~ /vol/ {
            proxy_pass http://47.93.238.102:8889;
        }
}
```

让配置生效
```bash
docker exec -it nginx-test bash -c "nginx -s reload"
```

4. 查看效果

可以看到访问/edu/和/vol/路径, 分别为tomcat-01和tomcat-02的内容

![](nginx5.png)

![](nginx6.png)

## 负载均衡

在浏览器中输入地址http://47.93.238.102:8890/edu/index.html, 实现负载均衡

1. tomcat中准备文件

tomcat-01和tomcat-02的webapps文件夹下新建文件夹edu, 并写入index.html文件

内容分别为
tomcat-01: tomcat 8888

tomcat-02: tomcat 8889

2. 配置nginx

```bash
docker run --name nginx-test -p 8880:80 -p 8890:8000 -d --rm -v /home/dal/document/nginx/:/etc/nginx/conf.d/ nginx
```

在/home/dal/document/nginx/下新建一个balance.conf文件

balance.conf
```nginx
upstream myserver {
    server 47.93.238.102:8888;
    server 47.93.238.102:8889;
}

server {
    listen       8000;
    server_name  localhost;

    location / {
        proxy_pass http://myserver;
    }
}
```

刷新配置
```bash
docker exec -it nginx-test bash -c "nginx -s reload"
```

3. 查看结果

![](nginx7.png)

![](nginx8.png)

可以发现刷新访问链接, 内容在两个tomcat之间进行切换, 即实现了负载均衡

## 动静分离

将动态请求和静态请求分离, 可以理解成使用nginx处理静态页面, tomcat处理动态页面

方式一(推荐):

纯粹将静态文件独立为单独的域名, 放在独立的服务器上

方式二:

动态和静态文件混合在一起发布, 通过nginx分开


## 实例

1. 新建资源文件

进入nginx容器
```bash
docker exec -it nginx-test bash
```

在/usr目录下新建data文件夹

并在其中放入image和www两个文件夹, 分别保存图片和网页

资源可以简单从外部复制
```bash
docker cp image nginx-test:/usr/data
docker cp www nginx-test:/usr/data
```

2. 修改配置文件

default.conf

```nginx
server {
    listen       80;
    server_name  localhost;

    location /www/ {
        root /usr/data/;
    }

    location /image/ {
        root /usr/data/;
        autoindex on;
    }
```

+ root 后面跟资源目录
+ autoindex on: 表示将文件夹中的内容列出来

3. 结果

访问http://47.93.238.102:8880/image/链接可以看到资源的列表被显示了出来

![](nginx8.png)

访问http://47.93.238.102:8880/www/index.html链接, 可以看到html页面

![](nginx10.png)

# 负载均衡服务的分配策略

1. 轮询(默认)

每个请求按时间顺序逐一分配到不同的后端服务器

2. weight

权值, 权重默认为1, 权重越高被分配的客户端越多

```nginx
upstream myserver {
    server 47.93.238.102:8888 weight=5;
    server 47.93.238.102:8889 weight=10;
}
```

3. ip_hash

每个请求按访问ip的hash结果分配, 这样每个访客固定访问一个后端服务器, 可以解决session问题

```nginx
upstream myserver {
    ip_hash
    server 47.93.238.102:8888;
    server 47.93.238.102:8889;
}
```

4. fair(第三方)

按后端服务器的响应时间来分配请求, 响应时间短的优先分配

# gzip压缩提升网站速度

gzip配置参数

+ gzip on | off: 是否开启gzip
+ gzip_buffers 32 4K | 16 8 K: 压缩在内容中缓冲几块, 每块多大
+ gzip_comp_level [1-9](推荐6): 压缩级别, 压的越小, 越浪费CPU资源
+ gzip_disable: 正则匹配URI, 什么样的URI不进行gzip
+ gzip_min_length 200: 开始压缩的最小长度
+ gzip_http_version 1.0 | 1.1: 开始压缩的http协议版本(可以不设置)
+ gzip_proxied: 设置请求者代理服务器, 该如何缓存内容
+ gzip_types text/css, application/xml: 对哪些类型的文件进行压缩, 如txt、xml、html、css
+ gzip_vary on | off: 是否传输gzip压缩标志 

```nginx
server{

    ...

    gzip on;
    gzip_buffers: 32 4K;
    gzip_comp_level 6;
    gzip_min_length 4000;
    gzip_types text/css text/xml applicatoin/javascript;
}
```

# expires缓存提升网站负载

在location或if段中写:

```nginx
expires 30s;
expires 30m;
expires 2h;
expires 30d;
```

```nginx
location ~* \.(jpg|jpeg|gif|png){
    root html;
    expires 1d;
}
```

# nginx中的正则表达式

```nginx
location [ = | ~ | ~* | ^~ ] uri{

}
```

1. =: 用于不含正则表达式的uri前, 要求请求字符串与uri严格匹配, 如果匹配成功, 就停止继续向下搜索并立即处理该请求
2. ~: 用于表示uri包含正则表达式, 并且区分大小写
3. ~\*: 用于表示uri包含正则表达式, 并且不区分大小写
4. ^~: 用于不含正则表达式的uri前, 要求nginx服务器找到标识uri和请求字符串匹配度最高的location后, 立即使用此location处理请求, 而不再使用location中的正则uri和请求字符串做匹配

注意: 如果uri包含正则表达式, 则必须要有~或~\*标识

