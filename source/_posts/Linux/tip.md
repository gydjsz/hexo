---
title: 一些操作方法
---

1. python版本切换
sudo update-alternatives --config python

2. 使用清华源，提升pip下载速度
Linux下，修改 ~/.pip/pip.conf (没有就创建一个文件夹及文件。文件夹要加“.”，表示是隐藏文件夹)

内容如下：

[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
[install]
trusted-host=mirrors.aliyun.com
