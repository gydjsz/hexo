---
title: python
tags: python
---

**大小写转换**
```python
name = "hello, world"
name = name.title()   #将单词首字母大写
print(name)           #输出Hello, World
print(name.upper())   #输出HELLO, WORLd   (将字符串中的所有字母转化为大写)
print(name.lower())   #输出hello, world   (将字符串中的所有字母转化为小写)
```
<!--more-->

**注释**
```python
单行: #

多行: ''' 注释''' 或者  """ 注释 """
```

**删除末尾的空格**
```python
favorite_language = 'python '
favorite_language.rstrip()   #删除末尾的空格
```

**将非字符串转化为字符串**
```python
str(n)  #将非字符串转化为字符串
```

**列表**
```python
list = []   #创建一个空列表
list.append('abc')    #在列表末尾添加元素
list.insert(0, 'abc')    #在列表的0位置处添加一个元素
del list[1]    #删除列表中1位置的元素
x = list.pop()    #弹出栈顶元素
list.remove('abc')   #移除第一个出现的元素abc
list.sort()    #按字母顺序从小到大
list.sort(reverse=True)   #按与字母顺序相反的顺序排列
sorted(list)     #字母顺序显示列表，不影响列表内部排序
sorted(list, reverse=True) #逆字母顺序显示列表，不影响列表内部排序
list.reverse()     #反转列表中的元素
len(list)    #获取列表长度
```

**print输出的格式**
```python
输入不换行:
print(2233, end = '')
```

**产生数字列表**
```python
生成数字列表:
numbers = list(range(5))   #range产生[0, 5)的整数
```

** #表示乘方运算

**最小值、最大值及总和**
```python
numbers = list(range(10))

#数字列表中的最小值,最大值以及总和
min(numbers)    
max(numbers)
sum(numbers)
```

**列表解析**
```python
range(start, stop, step)    #从start开始，以stop - 1结束步长为step的整数
list = [value for value in range(1, 21, 2)]   #1~20以内的奇数.列表解析: 表达式(value) + 循环(for...))
```

**切片**
```python
players = ['charles', 'martina', 'michael', 'florence', 'eli']
print(players[0 : 3])    #输出列表中的0~2号元素
print(players[:4])       #省略第一个索引，默认为从头开始
print(players[2:])       #省略第二个索引，默认为从索引2开始到末尾
print(players[-3:])      #从倒数第三个索引开始到末尾所有元素

#遍历切片
for player in players[:3]:
	print(player)

#复制列表
other_players = players[:]

#如果是下面这样, 并不能复制列表
other_players = players     #将other_players关联到players，other_players内容改变对players有效
```

**元组**
```python
dimensions = (200, 50)    #使用括号来标识, 元组内的值不能更改,但是可以重新定义
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

for key in favorite_language.keys():    #遍历键
	print(key)
for name in favorite_language.values():  #遍历值
	print(name)
for k, v in favorite_language.items():
	print(k + " " v)
```

**嵌套**
```python
#字典列表
alien_0 = {'color': 'green', 'points': 5}
alien_1 = {'color': 'yellow', 'points': 10}
alien_2 = {'color': 'red', 'points': 15}

aliens = [alien_0, alien_1, alien_2]

for alien in aliens:
	print(alien)

#字典中储存列表
pizza = {
	'crust': 'thick',
	'toppings': ['mushrooms', 'extra cheese'],
}
```






