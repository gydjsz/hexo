---
title: python
tags: python
---

# 大小写转换
```python
name = "hello, world"
name = name.title()   # 将单词首字母大写
print(name)           # 输出Hello, World
print(name.upper())   # 输出HELLO, WORLd   (将字符串中的所有字母转化为大写)
print(name.lower())   # 输出hello, world   (将字符串中的所有字母转化为小写)
```
<!--more-->

# 注释
```python
单行: # 

多行: ''' 注释''' 或者  """ 注释 """
```

# 删除末尾的空格
```python
favorite_language = 'python '
favorite_language.rstrip()   # 删除末尾的空格
```

# 将非字符串转化为字符串
```python
str(n)  # 将非字符串转化为字符串
```

# 列表
```python
list = []   # 创建一个空列表
list.append('abc')    # 在列表末尾添加元素
list.insert(0, 'abc')    # 在列表的0位置处添加一个元素
del list[1]    # 删除列表中1位置的元素
x = list.pop()    # 弹出栈顶元素
x = list.pop(1)  # 弹出索引为1的元素
list.remove('abc')   # 移除第一个出现的元素abc
list.sort()    # 按字母顺序从小到大
list.sort(reverse=True)   # 按与字母顺序相反的顺序排列
sorted(list)     # 字母顺序显示列表，不影响列表内部排序
sorted(list, reverse=True) # 逆字母顺序显示列表，不影响列表内部排序
list.reverse()     # 反转列表中的元素
len(list)    # 获取列表长度
list.count("2233")  # 统计数据出现的次数
tmp = [1, 2, 3]
list.extend(tmp)  # 追加列表
list.index("1")   # 获得列表的索引
```

# print输出的格式
```python
输入不换行:
print(2233, end = '')
```

# 产生数字列表
```python
生成数字列表:
numbers = list(range(5))   # range产生[0, 5)的整数
```

** # 表示乘方运算

**最小值、最大值及总和**
```python
numbers = list(range(10))

# 数字列表中的最小值,最大值以及总和
min(numbers)    
max(numbers)
sum(numbers)
```

# 列表解析
```python
range(start, stop, step)    # 从start开始，以stop - 1结束步长为step的整数
list = [value for value in range(1, 21, 2)]   # 1~20以内的奇数.列表解析: 表达式(value) + 循环(for...))
```

# 切片
```python
players = ['charles', 'martina', 'michael', 'florence', 'eli']
print(players[0 : 3])    # 输出列表中的0~2号元素
print(players[:4])       # 省略第一个索引，默认为从头开始
print(players[2:])       # 省略第二个索引，默认为从索引2开始到末尾
print(players[-3:])      # 从倒数第三个索引开始到末尾所有元素

# 遍历切片
for player in players[:3]:
	print(player)

# 复制列表
other_players = players[:]

# 如果是下面这样, 并不能复制列表
other_players = players     # 将other_players关联到players，other_players内容改变对players有效
```

# 元组
```python
dimensions = (200, 50)    # 使用括号来标识, 元组内的值不能更改,但是可以重新定义
name_list = [1, 2, 3]
name_tuple = (1, 2, 3)

# 列表和元组的相互转化
list(name_tuple)
tuple(name_list)
dimensions = (400, 100)
for dimension in dimensions:
	print(dimension)
```

**判断特定值是否在列表中**
```python
numbers = [1, 2, 3, 4]
print(3 in numbers)         //True
print(56 not in numbers)   //True
```

**遍历键值对**
```python
tom = {'name': 'tom', 'age': 34, 'heigh': 150, 'weight': 40}
for k, v in tom.items():
	print(k + ":", end = ' ')
	print(v)
```

**遍历字典**
```python
favorite_language = {
	'jen': 'python',
	'sarah': 'c',
	'edward': 'ruby',
	'phil': 'python',
}

for key in favorite_language.keys():    # 遍历键
	print(key)
for name in favorite_language.values():  # 遍历值
	print(name)
for k, v in favorite_language.items():
	print(k + " " v)

for key in favorite_language:
	print("%s %s" % (key, favorite_language[key]))

# 字典中有键则更改值,否则增加键值对
favorite_language["jen"] = "java"

# 合并字典
name_tuple = {
	"name": "李四",
	"age": 12
}
tmp = {
"name" :"张三"
}

# 有键则改值,否则增加键值对
name_tuple.update(tmp)

# 清空字典
name_tuple.clear()
```

# 嵌套
```python
# 字典列表
alien_0 = {'color': 'green', 'points': 5}
alien_1 = {'color': 'yellow', 'points': 10}
alien_2 = {'color': 'red', 'points': 15}

aliens = [alien_0, alien_1, alien_2]

for alien in aliens:
	print(alien)

# 字典中储存列表
pizza = {
	'crust': 'thick',
	'toppings': ['mushrooms', 'extra cheese'],
}
```

# 字符串操作

```python
string = "hello, world"
print(len(string))
print(string.count("l"))
print(string.index("l"))

# 判断空白字符
s = " "
print(s.isspace())

s = "3\u00b2"
print(s.isnumeric())
print(s)

s = "hello, world"
print(s.startswith("hell"))
print(s.endswith("hell"))

# 检测ｓｔｒ是否包含在ｓ的start,end范围中,是返回索引,否则返回-1
# index判断不存在会报错
print(s.find("l", 0, len(s)))

# 替换字符串,返回替换之后的字符串,不会修改原字符串的内容
s = "123"
sr = "2233"
print(s.replace(s, sr))
print(s)

poem = [
    "123",
    "1234",
    "12345",
    "123456",
    "1234567"
]

print("-" * 10)

# center 居中,填充长度,填充字符
# ljust 左对齐
# rjust 右对齐
for p in poem:
    print("|%s|" % p.ljust(10, "*"))


# lstrip() 截掉左边开始的空白字符
# rstrip() 截掉右边开始的空白字符
# strip() 截掉左右两边开始的空白字符

s = " aa bb cc "
print(s.lstrip())
print(s.rstrip())
print(s.strip())

# string.split(str="", num) 以str分割字符串string, 分割num+1个子字符串,返回列表
print(s.split())

# string.join(seq) 以string为分隔符,将seq中的所有元素合并为一个新的字符串
s = ["1", "2", "3"]
print(" ".join(s))
```

**绘制图形**

导入模块import turtle

+ 画笔运动函数

| 函数 | 别名(缩写) | 功能 |
| :----: | :----: | :----: |
|forward(d) | fd(d)	| 向前移动距离d代表距离|
|backward(d) | bk(d)或back(d) | 向后移动距离d代表距离|
|right(degree) | rt(degree)	| 向右转动多少度|
|left(degree) | It(degree) | 向左转动多少度|
|goto(x,y) || 将画笔移动到坐标为（x，y）的位置|
|stamp() || 绘制当前图形|
|speed(speed) || 画笔绘制的速度范围[0，10]整数|
|setheading(degree) | seth(degree) | 海龟朝向，degree代表角度|
|circle(radius,extent) || 绘制一个圆形，其中，radius为半径，extent为度数，例如，若extent为180，则画一个半圆；如果画一个圆形，可不必写第2个参数|

+ 画笔控制函数

| 函数 | 别名(缩写) | 功能|
| :----: | :----: | :----: |
|pendown() | down()或pd() | 画笔落下，移动时绘制图形|
|penup() | up()或pu() | 画笔拾起，移动时不绘制图形|
|reset() || 恢复所有设置|
|pensize(width) | width() | 画笔的宽度|
|pencolor(colorstring) || 画笔的颜色|
|fillcolor(colorstring)	|| 绘制图形的填充颜色|

+ 填充图像
turtle.begin_fill() 准备开始填充图形
绘制图像
turtle.end_fill()  填充完成

# 文件操作

open()函数将打开文件的对象保存在file_object中
关键字with的功能是不再需要访问文件后自动将文件关闭
read()将字符串保存在contents中
```python
with open("test.txt") as file_object:
	contents = file_object.read()
	print(contents)
```

逐行读取数据
```python
with open("test.txt") as file_object:
	for line in file_object:
		print(line)
```

将文件内容的每一行保存在一个列表中
```python
with open('test.txt') as file_object:
    lines = file_object.readlines()
```

将信息写入文件中
```python
with open('test2.txt','w') as info:
    info.write('Hello world!')
```

将信息追加到文件末尾
```python
with open('test2.txt','a') as info:
    info.write('Hello my brothers!\n')
```

# 异常处理

try中出现错误，执行except中的代码
```python
try:
    print(2/0)
except:
    print("We can't divide by zero!")
```

通过raise显式抛出自己的包含特定信息的异常, 执行了raise语句，raise之后的语句将不能执行
```python
def read_C():
    try:
        C = float(sys.argv[1])
		except ValueError:
			raise ValueError('Degrees must be number, not "%s"' % sys.argv[1])
    if C < -273.15:
        raise ValueError('C=%g is a non-physical value!' % C)
    return C
```

try中没有异常，执行else中的代码
```python
a = int(input())
b = int(input())
try:
    answer = a/b
except:
    print("We can't divide by zero!")
else:
    print(answer)
```

# 保留小数

```python
n = 3.14159
print("%.3f" % n)

'''
输出:
3.142
'''
```

```python
print(round(80.23456, 3))
print(round(100.000056, 3))

'''
输出:
80.235
100.0
'''
```

# lambda函数

```python
# 求x + y
f = lambda x, y: x + y
print(f(1, 2))
```

```python
list1 = [1, 7, 2, 3]
def f(x):
	return abs(x)

# 利用sorted函数对列表中的元素根据绝对值大小升序排序
list2 = sorted(list1, key = f)
print(list2)
```

```python
list1 = [1, 7, 2, 3]
list2 = sorted(list1, key = lambda x: abs(x))
print(list2)
```

# map()和reduce()

## map()函数

map()函数会根据传入的函数对指定的序列做映射。
map()函数接收两个参数，一个是function函数，另一个参数是一个或多个序列。
map()函数会将传入的函数依次作用到传入序列的每个元素，并把结果作为新的序列返回。

map()函数的定义为：

map(function, sequence[, sequence, ...]) -> list

```python
# 对传入的序列都执行乘2操作
r = map(lambda x: x * 2, [1, 2, 3, 4])
print(list(r))

'''
输出:
[2, 4, 6, 8]
'''
```

```python
# 对传入的序列依次执行求和操作
r = map(lambda x, y: x + y, [1, 2, 3], [4, 5, 6])
print(list(r))
```

## reduce()函数

reduce()函数把传入的函数作用在一个序列[x1, x2, x3, ...]上，这个函数必须要接收两个参数，reduce()函数把第一次计算的结果继续和序列中的下一个元素做累积计算

reduce()函数的定义为:
reduce(function, sequence[, initial]) -> value

function参数是有两个参数的函数，reduce()函数依次序列中取元素，和上一次调用function函数的结果做参数再次调用function函数.

```python
from functools import reduce
r = reduce(lambda x, y: x + y, [1, 2, 3, 4, 5],6)
print(r)

# 相当于((((((1+6)+2)+3)+4)+5))
```

# matplotlib绘图

import matplotlib.pyplot as plt

## 折线图


```python
import matplotlib.pyplot as plt

x = [1, 2, 3, 4]
y = [4, 5, 7, 8]
plt.plot(x, y)
plt.show()
```


![png](output_1_0.png)


---
#### 设置曲线的样式


```python
plt.plot(x, y, marker="*", linewidth=3, linestyle="--", color="orange")
plt.show()
```


![png](output_3_0.png)


|:-:|:-:|
|marker|数据点样式
|linewidth|线宽
|linestyle|线型样式
|color|颜色


```python
plt.plot(x, y, "bo-") ## 绘制蓝色圆点实线
plt.show()
```


![png](output_6_0.png)


---
#### 在一张图中绘制多条折线


```python
x1 = [3, 5, 7, 10]
y1 = [4, 2, 3, 1]
plt.plot(x, y, 'g--', x1, y1, 'bo-') ## (x,y)为绿色虚线，(x1, y1)为蓝色圆点直线
plt.show()
```


![png](output_8_0.png)


---
#### 绘制曲线：y = $x ^ 3$ + $x ^ 2$ + 1


```python
x = range(0, 31) ## 生成0-30的列表
def f(x):
    return x ** 3 + x ** 2 + 1
y = [f(w) for w in x] ## 获得x对应的y并存储为列表
plt.plot(x, y)
plt.show()
```


![png](output_10_0.png)


---
#### 数组计算

|:-:|:-:|
|array|接受一个序列并创建一个数组
|arange|类似于range
|linspace|返回在指定区间均匀间隔的数组，常用于创建等差数列
|zeros|根据指定形状创建一个全 0 数组
|zeros_like()|根据传入数组的形状创建一个全 0 数组
|ones()|根据指定形状创建一个全 1 数组
|ones_like()|根据传入数组的形状创建一个全 1 数组


```python
import numpy as np

x1 = np.array([1, 2, 3, 4]) ## 将序列转化为数组  
x2 = np.arange(1, 4)  ## 1~4的生成一个数组
x3 = np.arange(1, 10, 2) ## 产生一个间隔为2的数组
x4 = np.linspace(1, 9, 5) ## 产生1~10间隔相等的5个数的数组，也就是[1, 3, 5, 7, 9]
x5 = np.zeros((3, 3)) ## 创建一个3 * 3的全零数组
x6 = np.zeros_like(x5) ## 根据x5的形状,创建一个3 * 3的全零数组
x7 = np.ones((2, 3)) ## 创建一个2 * 3的全一数组
x8 = np.ones_like(x7) ## 根据x7的形状创建一个2 * 3的全一数组
print("array:\n{}".format(x1))
print("arange:\n{}".format(x2))
print("arange:\n{}".format(x3))
print("linspace:\n{}".format(x4))
print("zeros:\n{}".format(x5))
print("zeros_like:\n{}".format(x6))
print("ones:\ng{}".format(x7))
print("ones_like:\n{}".format(x8))
```

    array:
    [1 2 3 4]
    arange:
    [1 2 3]
    arange:
    [1 3 5 7 9]
    linspace:
    [1. 3. 5. 7. 9.]
    zeros:
    [[0. 0. 0.]
     [0. 0. 0.]
     [0. 0. 0.]]
    zeros_like:
    [[0. 0. 0.]
     [0. 0. 0.]
     [0. 0. 0.]]
    ones:
    g[[1. 1. 1.]
     [1. 1. 1.]]
    ones_like:
    [[1. 1. 1.]
     [1. 1. 1.]]
    

**向量化：将需要循环才能操作数组的 Python 函数转化为直接操作整个数组的函数。向量化能够使得程序更短、可读性更好，且程序的运行速度更快。**

#### 绘制$y = t^2*e^{-t^2}$


```python
import matplotlib.pyplot as plt
import numpy as np

def f(x):
    return x ** 2 * np.e ** (-1 * x ** 2)

x = np.linspace(0, 3, 50) ## 生成50个数，区间为[0, 3]
y = f(x)

plt.plot(x, y)
plt.show()
```


![png](output_16_0.png)

