---
title: java基本数据类型
tags: java
---

1. char类型转化为小写字母
大写为toUpperCase();
```java
char c = input.nextLine().toLowerCase().toCharArray()[0];
```
******
<!--more-->

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
```java
double b = 3.150;
NumberFormat nf = NumberFormat.getInstance();
nf.format(b);  //3.15
```
5. 将Double转化为Integer类型
Double d = 3.7;
```java
int n = d.inValue();  //double转化为int类型
Integer it = Integer.valueOf(n);   //int类型转化为double类型
```

6. double类型保留小数
```java
DecimalFormat df = new DecimalFormat("0.00");
double b = 3.141;
df.format(b);
```



