---
title: STL模板库学习笔记
photo: https://miao.su/images/2019/02/05/095b2b39b.png
---
[![](https://miao.su/images/2019/02/05/095b2b39b.png)](https://hexo.io/themes/)
<!--more-->

<font size = 4 color = "#00F5FF">
[<span id = "m">目录(点击章节可跳转)：</span>](#m)
1. [vector容器](#1)
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
3. [deque容器](#3)
- 3.1 [构造](#3.1)
- 3.2 [赋值操作](#3.2)
- 3.3 [大小操作](#3.3)
- 3.4 [双端插入与删除](#3.4)
- 3.5 [数据存取](#3.5)
- 3.6 [插入与删除操作](#3.6)
4. [stack容器](#4)
- 4.1 [构造](#4.1)
- 4.2 [赋值操作](#4.2)
- 4.3 [数据存取](#4.3)
- 4.4 [大小操作](#4.4)
5. [queue容器](#5)
- 5.1 [构造](#5.1)
- 5.2 [存取、插入和删除](#5.2)
- 5.3 [赋值](#5.3)
- 5.4 [大小操作](#5.4)
6. [list容器](#6)
- 6.1 [构造](#6.1)
- 6.2 [插入与删除](#6.2)
- 6.3 [大小操作](#6.3)
- 6.4 [赋值操作](#6.4)
- 6.5 [存取](#6.5)
- 6.6 [反转排序](#6.6)
7. [set/multiset容器](#7)
- 7.1 [构造](#7.1)
- 7.2 [赋值操作](#7.2)
- 7.3 [大小操作](#7.3)
- 7.4 [插入与删除](#7.4)
- 7.5 [查找操作](#7.5)
- 7.6 [修改排序规则](#7.6)
8. [map/multimap容器](#8)
- 8.1 [构造](#8.1)
- 8.2 [赋值](#8.2)
- 8.3 [大小操作](#8.3)
- 8.4 [插入和删除](#8.4)
- 8.5 [查找](#8.5)

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
v.begin()   //指向容器中第一个元素位置
v.end()     //指向容器中最后一个元素的下一个位置
v.rbegin()  //指向容器中最后一个位置
v.rend()    //指向容器中第一个元素的前一个位置
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

//逆序遍历
for(vector<int>::reverse it = v.rbegin(); it != v.rend(); ++it)
	cout << *it << endl;

//vector容器的迭代器是随机访问迭代器
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

# [<span id = "3">三、deque容器</span>](#m)
<font size = 3>
deque是双向开口，可以在头尾两端分别做元素的插入和删除操作
[<span id = "3.1">1.构造</span>](#m)
```cpp
deque<T> deqT   //默认构造
deque<T> deqT(begin, end)   //将[begin, end)区间中的数据拷贝给deqT
deque<T> deqT(n, elem)    //将n个elem元素拷贝给deqT
deque<T> deqT(deq)      //拷贝构造
```

[<span id = "3.2">2.赋值操作</span>](#m)
```cpp
assign(begin, end)   //将[begin,end)区间的元素拷贝过来
assign(n, elem)      //将n个elem拷贝过来
=   //重载运算符=直接赋值
swap(deq)    //与deq的元素交换
```

[<span id = "3.3">3.大小操作</span>](#m)
```cpp
size()   //返回容器中元素的大小
empty()  //判断容器是否为空
resize(num)   //重新指定容器的长度为num，若容器变长，则以默认值填充新位置。如果容器变短，则末尾超出容器长度的元素被删除
resize(num, elem)   //重新指定容器的长度为num，若容器变长，则以elem填充新位置。如果容器变短，则末尾超出容器长度的元素被删除
```

[<span id = "3.4">4.双端插入与删除</span>](#m)
```cpp
push_back(elem)   //在容器尾部插入一个数据
push_front(elem)   //在容器头部插入一个数据 
pop_back()     //删除容器最后一个数据
pop_front()   //删除容器第一个数据
```

[<span id = "3.5">5.数据存取</span>](#m)
```cpp
at(index) 和 []   //at()方法和[]访问,索引index
front()   //返回第一个数据
back()   //返回最后一个数据
```

[<span id = "3.6">6.插入和删除操作</span>](#m)
```cpp
insert(pos, elem)   //在pos位置插入一个elem元素,返回新数据位置
insert(pos, n, elem)   //在pos位置插入一个elem元素，无返回值
insert(pos, begin, end)  //在pos位置插入[begin, end)区间的数据，无返回值

clear()   //移除容器所有数据
erase(begin, end)   //删除[begin, end)区间的数据，返回下一个数据的位置
erase(pos)    //删除pos位置的数据,返回下一个数据的位置
```

[<span id = "3.7">7.一个例子</span>](#m)
```cpp
//将5名学生的分数排序，去掉最低和最高分，计算平均分
#include <iostream>
#include <deque>
#include <algorithm>
using namespace std;

int main(){
	deque<int> deq;   //定义
	deq.push_back(80);   //尾插入值
	deq.push_back(72);
	deq.push_back(90);
	deq.push_back(85);
	deq.push_back(74);
	sort(deq.begin(), deq.end());  //由小到大排序
	deq.pop_front();   //去掉头元素
	deq.pop_back();    //去掉尾元素
	double sum = 0;
	for(deque<int>::iterator it = deq.begin(); it != deq.end(); ++it)   //利用迭代器遍历求和
		sum += *it;
	cout << sum / deq.size() << endl;   //取平均
	return 0;
}

/*
如果是由大到小排序
   sort(deq.begin(), deq.end(), cmp);

   bool cmp(int a, int b){
	return a > b;
   }
*/
```
</font>

# [<span id = "4">四、stack容器</span>](#m)
<font size = 3>
stack所有元素进出都必须符合"先进后出"的条件，只有stack顶端的元素才有机会被外界取用.stack不提供遍历功能，也不提供迭代器
[<span id "4.1">1.构造</span>](#m)
```cpp
stack<T> stkT    //默认构造
stack<T> stkT(stk)  //拷贝构造 
```

[<span id = "4.1">2.赋值操作</span>](#m)
```cpp
stkT = stk //重载操作符=
```

[<span id = "4.2">3.数据存取</span>](#m)
```cpp
push(elem)  //向栈顶添加一个元素
pop()       //向栈顶移除一个元素
top()       //返回栈顶元素
```

[<span id = "4.3">4.大小操作</span>](#m)
```cpp
empty()   //判断堆栈是否为空
size()   //返回堆栈大小
```
</font>

# [<span id = "5">五、queue容器</span>](#m)
<font size = 3>
一种先进先出的数据结构,允许从一端新增元素，从另一端移除元素,不提供遍历，也没有迭代器
[<span id = "5.1">1.构造</span>](#m)
```cpp
queue<T> queT   //默认构造
queue<T> queT(que)   //拷贝构造
```

[<span id = "5.2">2.存取、插入和删除</span>](#m)
```cpp
push(elem)   //往队尾插入一个元素
pop()       //从队头移除第一个元素
back()      //返回最后一个元素
front()     //返回第一个元素
```

[<span id = "5.3">3.赋值</span>](#m)
```cpp
queT = que   //重载运算符=将一个queue容器拷贝给另一个
```

[<span id = "5.4">4.大小操作</span>](#m)
```cpp
empty()   //判断队列是否为空
size()    //返回队列的大小
```
</font>

# [<span id = "6">六、list容器</span>](#m)
<font size = 3>
list容器是一个循环双向链表，对于任何位置的元素插入和删除是常数时间
[<span id = "6.1">1.构造</span>](#m)
```cpp
list<T> lstT               //默认构造
list<T> lstT(begin, end)   //将[begin, end)区间中的元素拷贝给lstT
list<T> lstT(n, elem)      //将n个elem拷贝给lstT
list<T> lstT(lst)          //拷贝构造
```

[<span id = "6.2">2.插入和删除操作</span>](#m)
```cpp
push_back()   //在容器尾部加入一个元素
pop_back()    //删除容器最后一个元素
push_front(elem)   //在容器头部加入一个元素
pop_front()      //删除容器第一个元素
begin()     //第一个元素的迭代器
end()      //最后一个元素的下一个迭代器
rbegin()   //最后一个元素的迭代器   reverse_iterator迭代器
rend()     //第一个元素的前一个迭代器 reverse_iterator迭代器
insert(pos, elem)   //在pos位置插入elem元素,返回新数据的位置
insert(pos, n, elem)   //在pos位置插入n个elem元素
insert(pos, begin, end)   //在pos位置插入[begin, end)区间的数据
clear()   //移除容器内所有元素
erase(begin, end)   //删除[begin, end)区间的数据，返回下一个数据的位置
erase(pos)    //删除pos位置的数据,返回下一个数据的位置
remove(elem)   //删除容器中所有与elem值匹配的数据
```

[<span id = "6.3">3.大小操作</span>](#m)
```cpp
size()  //返回容器中元素的个数
empty()  //判断容器是否为空
resize(num)  //重新指定容器长度为num,若容器变长，则以默认值填充新位置，若容器变短，则末尾超出容器长度的元素被删除
resize(num, elem)  //重新指定容器长度为num,若容器变长，则以elem填充新位置，若容器变短，则末尾超出容器长度的元素被删除
```

[<span id = "6.4">4.赋值操作</span>](#m)
```cpp
assign(begin, end)  //将[begin, end)区间的数据拷贝给容器
assign(n, elem)     //将n个elem拷贝给容器
=     //重载运算符
swap(lst)   //和lst的元素交换
```

[<span id = "6.5">5.存取</span>](#m)
```cpp
front()   //返回第一个元素
back()   //返回最后一个元素
```

[<span id = "6.6">6.反转排序</span>](#m)
```cpp
reverse()   //反转链表，将数据反转
sort()      //list排序, 由小到大,由于list不是随机访问的迭代器，内部提供了相应算法的接口
/*
   //由大到小排序
   lst.sort(cmp);

   bool cmp(int a, int b){
	return a > b;
   }
*/
```
</font>

# [<span id = "7">七、set/multiset容器](#m)
<font size = 3>
所有元素会根据元素的键值自动被排序，set的元素既是键值又是实值(key即value)，set不允许两个元素有相同的键值.不能通过迭代器改变set元素的值，因为set元素值就是键值，关系到set元素的排序规则,如果任意改变，会破坏set组织。换句话说，set的iterator是一种const_iterator
multiset特性和用法与set完全相同，唯一的差别在于它允许键重复

[<span id = "7.1">1.构造</span>](#m)
```cpp
set<T> st   //默认构造
multiset<T> mst   
set<T> stT(st) //拷贝构造 
```

[<span id = "7.2">2.赋值操作</span>](#m)
```cpp
=         //重载运算符
swap(st)  //交换两个容器
```

[<span id = "7.3">3.大小操作</span>](#m)
```cpp
size()  //返回容器中元素的数目
empty()  //判断容器是否为空
```

[<span id = "7.4">4.插入和删除操作</span>](#m)
```cpp
insert(elem)   //在容器中插入elem元素
clear()      //清除所有元素
erase(pos)     //删除pos迭代器所指元素，返回下一个元素的迭代器
erase(begin, end)   //删除[begin, end)的所有元素，返回下一个元素的迭代器
erase(elem)   //删除容器中值为elem的元素
```

[<span id = "7.5">5.查找操作</span>](#m)
```cpp
find(key)   //查找key是否存在，若存在返回该键元素的迭代器，不存在，返回set.end()
count(key)   //查找键key的个数
lower_bound(keyElem)  //返回第一个key >= keyElem的迭代器
upper_bound(keyElem)  //返回第一个key > keyElem的迭代器
equal_range(keyElem) 
/*equal_range是C++ STL中的一种二分查找的算法，试图在已排序的[first,last)中寻找value，
它返回一对迭代器i和j，其中i是在不破坏次序的前提下，value可插入的第一个位置（亦即lower_bound），
j则是在不破坏次序的前提下，value可插入的最后一个位置（亦即upper_bound），
因此，[i,j)内的每个元素都等同于value，而且[i,j)是[first,last)之中符合此一性质的最大子区间
*/

pair<set<int>::iterator, set<int>::iterator> it = st.equal_range(elem);
//it.first为lower_bound()
//it.second为upper_bound()

//对组
pair是将2个数据组合成一组数据，当需要这样的需求时就可以使用pair，
如stl中的map就是将key和value放在一起来保存。
另一个应用是，当一个函数需要返回2个数据的时候，可以选择pair。
pair的实现是一个结构体，主要的两个成员变量是first second
因为是使用struct不是class，所以可以直接使用pair的成员变量。

pair<T1, T2> p   //创建一个空对象
pair<T1, T2> p(v1, v2)  //创建一个pair对象，两个元素分别为v1和v2
pair<T1, T2> p = make_pair(v1, v2)  //调用方法创建

pair.first   //第一个元素
pair.second  //第二个元素
```

[<span id = "7.6">6.修改set排序规则</span>](#m)
```cpp
#include <iostream>
#include <set>
using namespace std;

//利用仿函数指定set容器的排序规则
//使一个类的使用看上去像一个函数。其实现就是类中实现一个operator()，这个类就有了类似函数的行为，就是一个仿函数类了。
class Mycompare{
	public:
		bool operator()(int a, int b){   //重载()
			return a > b;
		}
};

int main(){
	set<int, Mycompare> st;    //由于set自动从小到大排序,这里需要一个排序函数，但是set<>中必须是一种类型，不能是函数，所以就用仿函数来使得一个类拥有函数的行为
	st.insert(1);
	st.insert(2);
	st.insert(3);
	st.insert(4);

	for(set<int, Mycompare>::iterator it = st.begin(); it != st.end(); ++it)
		cout << *it << " ";    //输出4 3 2 1
	return 0;
}
```
</font>

# [<span id = "8">八、map/multimap</span>](#m)
<font size = 3>
特性：所有元素会根据元素的键值自动排序，map的所有元素是pair，同时拥有实值和键值，pair第一元素为键值，第二为实值，map不允许两个元素有相同的键值(multimap键值可重复)
[<span id = "8.1">1.构造</span>](#m)
```cpp
map<T1, T2> mapT   //默认构造
map<T1, T2> mapT(m)  //拷贝构造
```

[<span id = "8.2">2.赋值</span>](#m)
```cpp
=   //重载操作符=
swap(mp)   //交换两个集合容器
```

[<span id = "8.3">3.大小操作</span>](#m)
```cpp
size()   //返回容器中元素的个数
empty()  //判断容器是否为空
```

[<span id = "8.4">4.插入和删除</span>](#m)
```cpp
insert()   //往容器插入元素，返回pair<iterator, bool>
//第一种:通过pair的方式插入
insert(pair<int, string>(1, "折纸"))
//第二种:通过pair的方式插入
insert(makr_pair(2, "二亚"))
//第三种:通过value_type的方式插入
insert(map<int, string>::value_type(3, "狂三"))
//第四种:通过数组的方式插入
mapStu[7] = "七罪"
mapStu[8] = "八舞"

first 和 second   //键值和实值

clear()   //删除所有元素
erase(pos)   //删除pos迭代器所指的元素，返回下一个元素的迭代器
erase(begin, end)   //删除[begin, end)的所有元素，返回下一个元素的迭代器
erase(keyElem)     //删除容器中key为keyElem的对组
```

[<span id = "8.5">5.查找</span>](#m)
```cpp
find(key)   //查找key是否存在，若存在，返回元素的迭代器，否则返回map.end()
count(keyElem)   //返回容器中key为keyElem的对组个数，对map来说要么是0,要么是1，对于multimap来世，值可能大于1
lower_bound(keyElem)  //返回第一个key >= keyElem元素的迭代器
upper_bound(keyElem)  //返回第一个key > keyElem元素的迭代器
equal_bound(keyElem)   //返回容器中key与keyElem相等的上下限的两个迭代器
```
</font>
