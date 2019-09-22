---
title: markdown语法学习
tags: markdown
---
[markdown语法]https://www.jianshu.com/p/b03a8d7b1719
[markdown语法]https://www.appinn.com/markdown/#header
[先来一头马克飞象(在线使用markdown)]https://maxiang.io/
<!--more-->

目录：
1. [插入跳转链接](#1)
2. [缩进](#2)
3. [插入图片](#3)
4. [添加页内跳转](#4)
5. [文章摘要显示](#5)
6. [表格](#6)

# <span id = "1"> <font size = 3>1.插入跳转链接</font></span>
<a href="https://blog.fbzl.org/" target="_blank">https://blog.fbzl.org/"</a>
```
<a href="https://blog.fbzl.org/" target="_blank">https://blog.fbzl.org/"</a>
```
******

# <span id = "2"><font size = 3>2.缩进</font></span>
在每一行开头的时候，先输入下面的代码，然后紧跟着输入文本即可。注意有分号
```
半角空格: &ensp;或 &#8194;
全角空格: &emsp;或 &#8195;
不换行空格: &nbsp;或 &#160;
```
******

# <span id = "3"><font size=3>3.插入图片</font></span>

首先插入本地图片的地址不是绝对路径,比如说我图片的绝对路径为~/Picture/picture1.jpg
我写博客的位置是~/Documents/blog,那么添加的图片路径是相对路径,即从我当前路径开始算起,
路径为../Picture/picture1.jpg;

了解linux的应该知道, ".."表示上一层目录, 可是我使用这种方法,会出现网页中显示不了图片的情况,甚至在一篇文章里面可以显示,但是放在另一篇文章里面就显示不了的情况,所以现在给出其它的方法

一. <font size=3>**使用base64编码**</font>
图片和base64编码转换网址: http://www.vgot.net/test/image2base64.php?

因为编码很长,所以可以放在文章的最后面,以下的picture是编码id,可以自己设定
```
文章

插入的图片位置:
![图片说明][picture]

文章

文章末尾

代码区域填写:
[picture]:data:image/png;base64,编码

```

二. <font size=3>**使用百度云添加图片外链**</font>
1. 首先上传图片到百度云盘中,存储容量有2T,所以不用担心图片太多,最好是单独创建一个文件夹,将图片保存在里面

2. 鼠标右键点击分享,默认加密选项,有效期永久,然后点击确定

3. 将链接复制到浏览器中打开,可以看到自己的图片,然后右键点击图片然后复制图片链接,这个就是你的图片外链了

4. 在博客中添加图片外链:
```
![图片说明](链接)
```


二. <font size=3>**使用github添加图片外链**</font>
1. 先将图片上传到github中
2. 在github中打开图片，复制图片链接

# <span id="4"><font size=3>4.添加页内跳转</font></span>
```
[跳转到标题](#1)
<span id="1">标题</span>
```

# <span id="5"><font size=3>5.文章摘要显示</font></span>
```
摘要文字部分
<!--more-->
```

# <span id="6"><font size=3>6.插入表格</font></span>

```cpp
| 左对齐 | 右对齐 | 居中对齐 |
| :-----| ----: | :----: |
| 单元格 | 单元格 | 单元格 |
| 单元格 | 单元格 | 单元格 |
```
