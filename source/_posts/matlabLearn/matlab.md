---
title: matlab学习
tags: matlab
---

1. clc,clear;   清除命令

2. close all   清除所有窗口

3. warning off   关闭警告
<!--more-->

4. fprintf('result: %d', a); 输出a的值

* fprintf(fid, format, A)
* fid输出的位置(如果缺省，则输出在窗口)
* format(输出内容的类型)
* %e: 实数   %.3f: 浮点数(保留3位小数)
* A:输入内容的变量名

5. disp(a) 显示a的值

6. disp('hello');
   disp('numstr(12)')   %%numstr()将其他类型的变量转化成字符串形式
   disp(zeros(m, n))

7. <font size=3>**顺序结构**</font>

8. <font size=3>**判断结构**</font>
```matlab
if 条件
	执行内容
elseif 条件二
	执行内容
else
	执行内容
end
```

9. <font size=3>**循环结构**</font>
```matlab
for i = 1 : m
	循环内容
end

for i = 1 : 3 : 9
	fprintf('%d ', i);  输出1，4，7(步长为3)
end

while(条件)
	循环内容
end
```

10. a = mod(45678, 10)   获得45678的余数

11. n = fix(465 / 10)    n的值为46

12. [m, n] = size(a)   获取a的行、列数
	a(i, j)   第i行j列的值

13. <font size=3>**函数的自定义**</font>
function[输出] = fun(n)

