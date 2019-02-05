---
title: STL模板库学习笔记
---
![](https://miao.su/images/2019/02/05/095b2b39b.png)
<!--more-->
<font size = 4 color = "#00F5FF">
[<span id = "m">目录(点击章节可跳转)：</span>](#m)
1. [vector](#1)
- 1.1 [构造](#1.1)
- 1.2 [常用赋值操作](#1.2)
- 1.3 [大小操作](#1.3)
- 1.4 [存取](#1.4)
- 1.5 [插入与删除](#1.5)
- 1.6 [一些技巧](#1.6)
2. [string容器](#2)
- 2.1 [构造](#2.1)
- 2.2 [基本操作](#2.2)
- 2.3 [存取操作](#2.3)
- 2.4 [拼接操作](#2.4)
- 2.5 [查找与替换](#2.5)
- 2.6 [字典序比较](#2.6)
- 2.7 [子串](#2.7)
- 2.8 [插入与删除](#2.8)
- 2.9 [string和char\*](#2.9)
- 2.10 [一些实用方法](#2.10)
</font>

# <span id="1">[**一、vector**](#m)</span>
<font size = 3>
迭代器可以动态分配内存,当内存不足时，会自动分配一个更大的空间，并把值拷贝进来，原有空间释放
<span id = "1.1">[1.构造](#m)</span>
```cpp
//头文件#include <vector>
vector<T> v;    //定义
vector<T> v(v.begin(), v.end())  //拷贝[v.begin(),v.end())的元素
vector<T> v(n, elem);    //将n个elem
vector<T> v(vec)     //拷贝构造
```

[<span id = "1.2">2.常用赋值操作</span>](#m)
```cpp
v.assign(begin, end);   //将[begin, end)拷贝到v中
v.assign(n, elem);     //将n个elem拷贝给v
v = vec;   //直接赋值
v.swap(vec);   //v与vec元素互换

//使用swap()方法收缩内存
vector<int> v;
for(int i = 0; i < 10000; i++)
	v.push_back(i);
v.size();   //大小为10000
v.capacity();  //大小大于10000

v.resize(3);  //重新指定容器长度为3,其容量大于10000
//vector<int> (v)为匿名构造一个vector容器，将v中的值赋给匿名容器，容器size为3,容量为3
vector<int> (v).swap(v);  //此时匿名容器和v交换，v的size为3,容量也为3,匿名容器空间被系统自动回收
```

[<span id = "1.3">3.大小操作</span>](#m)
```cpp
size()   //返回容器中元素的个数
empty()  //判断容器是否为空
resize(num)  //重新指定容器的长度为num， 若容器变长，则以默认值填充新位置，如果容器变短，则末尾超出容器长度的元素被删除
resize(num, elem)  //重新指定容器的长度为num， 若容器变长，则以elem填充新位置，如果容器变短，则末尾超出容器长度的元素被删除
capacity()   //容器的容量
reserve(len)   //容器预留len个元素长度，预留位置不可初始化，元素不可访问

如果事先可以预测所需空间大小
v.reserve(10000)  //预开辟空间10000, 可以节约多次扩充空间的时间

```

[<span id = "1.4">4.存取</span>](#m)
```cpp
v.at(index) 和 v[index]  //返回index所指数据
front()   //返回容器中第一个数据的元素
back()   //返回容器中最后一个数据的元素
```

[<span id = "1.5">5.插入与删除</span>](#m)
```cpp
insert(pos, count, elem)   //迭代器指向的pos(pos是迭代器)位置插入count个elem元素
push_back(elem)    //尾部插入一个元素elem
pop_back()   //删除最后一个元素
erase(start, end)     //删除迭代器从start到end之间的元素,start和end是迭代器
clear()   //删除容器中所有元素
```

[<span id = "1.6">6.一些技巧</span>](#m)
```cpp
//向容器中添加数据
v.push_back(1);  //将1存入容器v中
v.push_back(2);  //将2存入容器v中
v.push_back(3);  //将3存入容器v中

//通过迭代器遍历容器
//v.begin() 起始迭代器，指向第一个元素的地址，v.end() 结束迭代器，指向最后一个元素的下一个地址
for(vector<int>::iterator it = v.begin(); it != v.end(); ++it){   	
	cout << *it << endl;   //依次输出容器中的数据
}

//通过for_each()
//头文件#include <algorithm>
for_each(v.begin(), v.end(), output);    

void output(int n){
	cout << n << endl;
}

//容器嵌套
vector<vector<int>> vv;

vector<int> v1;
vector<int> v2;
vector<int> v3;

v1.push_back(1);
v2.push_back(2);
v3.push_back(3);

vv.push_back(v1);
vv.push_back(v2);
vv.push_back(v3);

for(vector<vector<int>>::iterator iter = vv.begin(); iter != vv.end(); ++iter)
	for(vector<int>::iterator it = (*iter).begin(); it != (*iter).end(); ++it)
		cout << *it << endl;
```
</font>

# [<span id = "2">**二、string容器**</span>](#m)
<font size = 3>
[<span id = "2.1">1. 构造</span>](#m)
```cpp
string str;   //创建一个空的字符串
string s(str);  //使用一个string对象初始化另一个string对像,即拷贝
string s1(3, 'h');   //使用n个字符初始化
```

[<span id = "2.2">2.string基本赋值操作</span>](#m)
```cpp
string s = "hello";
string s1 = s;  //字符串赋值 
s1.assign(s);   //将字符s赋给s1
s1.assign("hello", n);  //将前n个字符赋给当前字符串(string& assign(const char* s, int n)), 这里貌似使用常量字符串或者字符数组是这样的，如果是变量字符串，那么是从n位置开始之后的字符串,如果n是3, 那么s1 = "lo";
s1.assign(n, c);  //用n个字符c赋给s1
s1.assign(s, start, n);  //将s从start开始n个字符赋值给s1
```
 
[<span id = "2.3">3.string存取字符操作</span>](#m)
```cpp
string s = "hello";
s[0];   //通过[]获取
s[1];
s[2];

s.at(0);  //通过at方法获取
s.at(1);
s.at(2);
 
/*
1.[]和at的区别：[]访问越界直接挂掉，at访问越界会抛出out_of_range异常
2.[]效率高，at更安全
*/
```

[<span id = "2.4">4.string拼接操作</span>](#m)
```cpp
string s1 = "hello", s2 = "world";

s1 += s2;   //使用+=操作符，将s2连接到s1尾

s1.append(s2);   //append()方法, 将s2连接到s1尾
s1.append("hello", n);   //将字符串的前n个字符连接到s1尾,适用于字符串常量和字符数组

s1.append(s2, start, n);  //将字符串s2从start开始的n个字符连接到s1尾
s1.append(n, c);   //在s1尾部添加n个字符c
```

[<span id = "2.5">5.string查找与替换</span>](#m)
```cpp
//查找不到返回-1
str.find(s, pos);  //查找第一次出现s的位置，从pos开始查找
str.find(s, pos, n); //从pos开始查找s的前n个字符第一次出现的位置

//rfind(),r即right,从右往左查找
//这里应该是每次将该字符作为首字符依次往后找，比如str = "abcabc", n = str.find("abc", 3),n = 3;
str.rfind(s, pos);     //从pos开始从右向左查找s第一次出现的位置
str.rfind(s, pos, n);  //从pos开始从右向左查找s的前n个字符第一次出现的位置

str.replace(pos, n, s);   //替换从pos开始的n个字符为字符串s
/*
str = "hello";
str.replace(0, 2, "abcdef");   //str = "abcdefllo"
*/
```

[<span id = "2.6">6.string比较操作</span>](#m)
```cpp
str1.compare(str2);  //字典序比较，>返回1，<返回-1, ==返回0
//也可以直接使用>,<,==比较
```

[<span id = "2.7">7.string子串</span>](#m)
```cpp
str.substr(pos, n);  //返回由pos开始的n个字符组成的字串
```

[<span id = "2.8">8.插入与删除</span>](#m)
```cpp
str.insert(pos, s);   //在pos位置插入字符串s
str.insert(pos, n, c);  //在指定位置插入n个字符c
str.erase(pos, n);   //删除从pos开始的n个字符
```

[<span id = "2.9">9.string和char\*</span>](#m)
```cpp
//string转char*
string s = "hello";
const char* cs = s.c_str();   //添加const,或者强制转换(char*)s.c_str(),因为其返回值是const char*
//c++中存在const char*到string的隐式类型转换,反之不行

//char*转string
char* cs = "hello";
string s(cs);  //直接赋值就ok
```

[<span id = "2.10">10.一些实用方法</span>](#m)
```cpp
c = toupper(c);   //将小写字符c转化为大写
c = tolower(c);   //将大写转化为小写
```
</font>
