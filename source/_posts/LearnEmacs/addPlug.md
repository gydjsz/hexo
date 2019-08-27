---
title: 为emacs添加源
tags: emacs
---
参考链接:https://blog.csdn.net/sjhuangx/article/details/51252522
参考链接:https://blog.csdn.net/watanuki2006/article/details/52122427
<!--more-->
* 方法1 
emacs中输入M-x customize-variable RET package-archives，
进入之后可以看到当前的package源，
点击下面的INS按钮来插入新的package源，输入一个名字，一个url链接

name：melpa
url：https://melpa.org/packages

清华源:
"gnu" . "http://mirrors.tuna.tsinghua.edu.cn/elpa/gnu/"
"melpa" . "http://mirrors.tuna.tsinghua.edu.cn/elpa/melpa/"

输入list-pakcages, 进入插件管理

i - 选择要安装的包

d - 选择要删除的包

U - 升级已安装的包

x - 执行操作

d - 选择要删除的包

输入 M-x package-refresh-contents，刷新package信息

