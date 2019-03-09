---
title: Linux下超级好用的工具
---

1. autojump: 可以记录进入过的目录, 支持直接跳转到想到的目录
   j path: 进入path目录
   j: 进入权值最高的目录
   j -i[权重]: 增加权值
   j -d[权重]: 减少权值
   jo path: 打开目录
   j -s: 显示自动跳转数据库中的条目

2. xsel: 建立终端和剪切板之间的通道
   cat txt | xsel -b -i: 将txt文件中的内容复制到xsel中
   xsel > txt: 将xsel中的内容复制到txt中
   xsel -o -i: 查看xsel中的内容

