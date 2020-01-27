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

# 常用函数

## substring

提取字符串中介于两个指定下标之间的字符

```javascript
var msg = 'hello';
msg.substring(start, end)
```

+ start参数为必须项, 指定提取的第一个字符所在的位置
+ end参数可选, 指定最后一个字符的下一个位置, 如果省略则返回的子串会一直到字符串结尾

## splice

在数组中添加/删除项目, 然后返回被删除的项目

```javascript
array.splice(index, howmany, item1, ..., itemX)
```

+ index: 必须, 规定位置, 使用负数表示从结尾处开始规定位置
+ howmany: 必须, 表示要删除的数量, 为0, 则不会删除
+ item1,...,itemX: 可选, 向数组中添加新的项目

## filter

创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素

```javascript
array.filter(function(currentValue, index, arr), thisValue)
```

+ currentValue: 必须项, 表示当前元素
+ index: 可选, 表示当前元素的索引值
+ arr: 可选, 表示当前元素所属的数组对象
+ thisValue: 可选, 对象作为该执行回调时使用, 传递给函数, 用作 "this" 的值。
如果省略了thisValue, "this" 的值为 "undefined"

```js
var arr = list.filter(x => x === false)
```

## padStart和padEnd

+ padStart: 在头部补全字符串
+ padEnd: 在尾部补全字符串

字符串不足两位, 在字符前面补0
```javascript
str.padStart(2, '0')
```

## findIndex方法

传入一个方法, 返回满足该方法的第一个索引

```js
var arr = [1, 2, 3, 4, 5]
const i = arr.findIndex(x => x === 5) // i = 4
```

# Timing事件

1. 在等待指定的毫秒数后执行函数
```javascript
setTimeout(function, milliseconds)
```

2. 等同于setTimeout(), 但持续重复执行该函数
```javascript
setInterval(function, milliseconds)
```

3. clearTimeout() 方法停止执行 setTimeout() 中规定的函数。

```javascript
clearTimeout(timeoutVariable)
```

4. clearInterval() 方法停止 setInterval() 方法中指定的函数的执行。

```javascript
clearInterval(timerVariable);
```
