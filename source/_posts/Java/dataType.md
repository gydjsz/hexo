---
title: java基本数据类型
---

1. char类型转化为小写字母
大写为toUpperCase();
```java
char c = input.nextLine().toLowerCase().toCharArray()[0];
```
******

2. float类型常量后面必须要有后缀f或F
double类型后面有后缀d或D,但是可以省略
```java
float x = 3.14f;
```
******

3. double类型可以进行模运算
```java
double x = 3.0 / 2;   //x的值为1.0
```
******

4. 数字格式化
double b = 3.150;
NumberFormat nf = NumberFormat.getInstance();
nf.format(b);  //3.15


