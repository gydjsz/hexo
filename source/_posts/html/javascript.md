---
title: JavaScript
tags:
- JavaScript
---

# JavaScript常用系统函数

<!--more-->

|函数名|说明|
|:-:|:-:|
|eval("1 + 1")|返回字符串表达式中的值|
|parseInt(string, radix)|返回不同进制的数(默认十进制), 值介于2~36之间,如果在此范围外, 则返回NaN|
|parseFloat("1.1")|返回实数, 将一个字符串转换成对应的小数|
|escape("?a=1")|返回对一个字符串进行编码后的结果字符串|
|encodeURI("?a=12 ")|返回一个对URI字符串编码后的结果|
|decodeURI()|将一个已编码的URI字符串解码成最原始的字符串返回|
|unescape()|将一个用escape方法编码的结果字符串解码成原始字符串并返回|
|isNaN()|检测是否为非数值型, 是, 返回true|
|Math.abs()|绝对值|
|Math.ceil()|返回大于等于x的最小整数|
|Math.floor()|返回小于等于x的最大整数|
|Math.max()/Math.min()|返回两个数的最大/最小值|
|Math.pow(n, m)|$n^m$|
|Math.random()|返回[0, 1)的数|
|Math.round()|返回四舍五入后的值|
|<对象>.toString()|把对象转换为字符串, 如果在括号内指定一个2~36的整数, 则所有数值转换成特定的进制数
