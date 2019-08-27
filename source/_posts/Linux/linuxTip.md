---
title: Linux下的技巧操作
tags: linux 
---

# 一、好用的工具
1. autojump: 可以记录进入过的目录, 支持直接跳转到想到的目录
   j path: 进入path目录
   j: 进入权值最高的目录
   j -i[权重]: 增加权值
   j -d[权重]: 减少权值
   jo path: 打开目录
   j -s: 显示自动跳转数据库中的条目
<!--more-->

2. xsel: 建立终端和剪切板之间的通道
   cat txt | xsel -b -i: 将txt文件中的内容复制到xsel中
   xsel > txt: 将xsel中的内容复制到txt中
   xsel -o -i: 查看xsel中的内容


# 二、一些操作
1. python版本切换
sudo update-alternatives --config python

2. 使用清华源，提升pip下载速度
Linux下，修改 ~/.pip/pip.conf (没有就创建一个文件夹及文件。文件夹要加“.”，表示是隐藏文件夹)

内容如下：
```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
[install]
trusted-host=mirrors.aliyun.com
```

3. 查看服务状态
service --status-all  查看所有的服务状态
service xx status     查看xx的服务状态

4. 进程
ps aux 查看进程详细状况
a 显示终端上所有进程
u 显示进程的详细状态
x 显示没有控制终端的进程

top 动态显示进程并排序
ps -ef | grep 进程名
kill -9 进程号  (-9表示强行终止)

5. 远程访问
ssh -p port(默认22) userName@ip

免密登录:
ssh-keygen 生成秘钥
ssh-copy-id userName@ip 将公钥上传到服务器

配置别名:
在.ssh/目录下
touch config

加入,根据自己情况修改
Host server(别名)
	HostName ip
	User userName
	Port 22

ssh server即可登录

6. 互传文件
scp 源文件地址　目的地址

scp hello.cpp userName@ip:地址
scp userName@ip:文件地址　目的地址

配置别名之后
scp hello.cpp server:地址

7. ls
-a 所有子目录和文件
-l 列表显示文件详细信息
-h 人性化显示文件大小

8. cat和more
cat
-b 对非空行编号
-n 对所有行编号

more
空格 显示下一屏
enter 显示下一行
b 回滚一屏
f 前滚一屏
q 退出
/word 搜索

9. 重定向
```
>和>>
> 将结果重定向到文件中
>> 将结果追加到文件中
```

10. 权限管理
chown 修改拥有者
chgrp 修改组
chmod 修改权限

修改文件/目录拥有者
chown 用户名 文件名

递归修改文件/目录的组
chgrp -R 组名 文件名

递归修改文件权限
chmod -R 755 文件名

chmod修改权限
chmod +/-rwx 文件/目录

chmod修改权限的三个数字对应拥有者 组和其它用户的权限
r w x
4 2 1

11. 组管理
groupadd 组名    添加组
groupdel 组名    删除组
chgrp (-R) 组名 文件/目录名  (递归)修改所属组

12. 用户管理
useradd -m -g 组  新建用户名    添加新用户, -m自动建立家目录, -g指定用户所在组,否则新建一个同名组

passwd 用户名    设置用户名密码
userdel -r 用户名   删除用户,-r自动删除家目录

13. usermod
usermod -g 组 用户名   修改用户主组
usermod -G 组 用户名   修改用户附加组
usermod -s /bin/bash   修改用户登录shell

14. 时间和日期
date
cal -y 查看一年的日历

15. 磁盘信息
df -h  显示磁盘剩余空间
du -h 目录  显示目录下的文件大小

16. 查找文件
find 路径 -name "文件名"

17. 软链接
ln -s 被链接的源文件 链接文件  (无-s建立的是硬链接)

18. 打包和解包
打包
tar -cvf 打包文件.tar 被打包的文件

解包
tar -xvf 打包文件.tar

c 生成档案文件,创建打包文件
x 解开档案文件
v 列出归档解档的详细信息,显示进度
f 指定档案文件名称, f后面一定是.tar文件,所以放后面


1)gzip
压缩
tar -zcvf 打包文件.tar.gz 被打包的文件
解压
tar -zxvf 打包文件.tar.gz -C 目标路径

z 调用gzip,实现压缩和解压的功能
C 解压缩到指定目录

2)bzip2
压缩文件
tar -jcvf 打包文件.tar.bz2 被打包的文件
解压
tar -jxvf 打包文件.tar.bz2 -C 目标路径









