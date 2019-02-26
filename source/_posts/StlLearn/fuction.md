---
title: 二、STL算法篇
---

1. next_permutation(): 按照字典顺序产生区间内元素下一个较大的排列组合
   prev_permutation(): 按照字典顺序产生区间内元素下一个较小的排列组合
```cpp
#include <algorithm>
int perm[] = {3, 1, 2};   //初始化该数组

int permutation(int n){
	do{
		for(int i = 0; i < 3; i++)    //输出一个排列
			cout << perm[i] << " ";
		cout << endl;
	}while(next_permutation(perm, perm + 3));   //按照字典序生成排列，若生成完毕，返回false
	/*输出 3 1 2
	       3 2 1 */
	return 0;
}
```
2. nth_element(a + l, a + k, a + r): 使得ａ这个数组中区间(l, r)内的第ｋ大元素处在第ｋ个位置上, 默认排在k前面的元素都不比它大，排在它后面的元素都不比它小,而左右两边是无序的
```cpp
#include <algorithm>

int kth(){
	int a[] = {0, 6, 3, 7, 5, 1};
	nth_element(a + 1, a + 2, a + 6);
	for(int i = 1; i < 6; i++)
		cout << a[i] << " ";    //第2大是３，输出1 3 7 5 6
	return 0;
}
```

3. to_string(): 将数字转化为字符串
```cpp
string s = to_string(123);   // s = "123"
```

使用stringstream字符串流来实现转换
```cpp
#include <sstream>   //头文件sstream

//int 转 string
int n = 123;
stringstream ss;
ss << n;
string s;
ss >> s;
cout << s << endl;   //s = "123"

ss.clear();   //同一个stream多次转换应该调用clear()方法

//string 转 int
s = "2233";
ss << s;
ss >> n;
cout << n << endl;   //n = 2233
```


