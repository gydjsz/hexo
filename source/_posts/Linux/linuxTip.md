---
title: Linux下的技巧操作
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
