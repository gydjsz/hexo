---
title: 第七届蓝桥杯真题解题报告
---

<span id = "0">目录：</span>
1. [煤球数目](#1)
2. [生日蜡烛](#2)
3. [凑算式](#3)
4. [快速排序](#4)
5. [抽签](#5)
6. [方格填数](#6)
7. [剪邮票](#7)
8. [四平方和](#8)
9. [交换瓶子](#9)
10. [最大比例](#10)

<!--more-->

[<span id = "1">1. 煤球数目</span>](#0)

有一堆煤球，堆成三角棱锥形。具体：
第一层放1个，
第二层3个（排列成三角形），
第三层6个（排列成三角形），
第四层10个（排列成三角形），
....
如果一共有100层，共有多少个煤球？

请填表示煤球总数目的数字。
注意：你提交的应该是一个整数，不要填写任何多余的内容或说明性文字。

**解：
思路：
3 - 1 = 2
6 - 3 = 3
10 - 6 = 4
通过观察可以写出该数列的递推公式: an - an-1 = n, 对an累加求和即可**

```cpp
#include <iostream>
using namespace std;

int main(){
	int a = 0, s = 0;
	for(int i = 1; i <= 100; i++){   //递推计算数列
		a += i;   //求an
		s += a;   //累加ai
	}
	cout << s << endl;
	return 0;
}
```

**答案：171700**

---

[<span id = "2">2. 生日蜡烛</span>](#0)

某君从某年开始每年都举办一次生日party，并且每次都要吹熄与年龄相同根数的蜡烛。
现在算起来，他一共吹熄了236根蜡烛。
请问，他从多少岁开始过生日party的？
请填写他开始过生日party的年龄数。
注意：你提交的应该是一个整数，不要填写任何多余的内容或说明性文字。

**解：
思路：因为蜡烛数目和年龄数对应，而年龄是一个等差数列，其前n项和为Sn = n * (n + 1) / 2**
**设他从n年开始举办生日，现在已经m年了，则Sm - Sn + an= 236, (这里+an，是因为会减去开始的那一年的蜡烛数，比如从1开始举办生日，举办了3年，就是Sm = 1+2+3 = 6, Sn = 1, Sm - Sn = 5, (实际应该是6才对).
m * (m + 1) / 2 - n * (n + 1) / 2 + n = 236<br />
=> (m + n)(m - n + 1) = 472<br/>
472因式分解：2, 2, 2, 59<br />
得出m - n + 1只能是2 * 2 * 2 = 8, (m + n)只能是59<br />
m - n + 1 = 8<br />
m + n = 59  联立求得方程组的解为m = 33, n = 26**

程序解决：
```cpp
#include <iostream>
#include <cstring>
using namespace std;

int main(){
	int sum;
	for(int i = 1; i <= 150; i++){
		sum = 0;
		for(int j = i; j <= 150; j++){
			sum += j;
			if(sum == 236){
				cout << i << endl;
			}
		}
	}
	return 0;
}
```

**答案：26**

---


[<span id = "3">3. 凑算式</span>](#0)

```
     B      DEF
A + --- + ------- = 10
     C      GHI
```
     
这个算式中A ~ I代表1 ~ 9的数字，不同的字母代表不同的数字。

比如：
6+8/3+952/714 就是一种解法，
5+3/1+972/486 是另一种解法。

这个算式一共有多少种解法？

注意：你提交应该是个整数，不要填写任何多余的内容或说明性文字。

**解：
思路：生成1-9的全排列，然后暴力尝试即可，这里可以将除法转化为乘法，避免小数的情况，转化后为
A * C * GHI + B * GHI + C * DEF = 10 * C * GHI**

```cpp
#include <iostream>
#include <algorithm>
using namespace std;

int main(){
	int a[10];
	for(int i = 1; i <= 9; i++){
		a[i] = i;
	}
	int sum = 0;
	do{
		int A = a[1], B = a[2], C = a[3], D = a[4], E = a[5], F = a[6], G = a[7], H = a[8], I = a[9];
		int GHI = G * 100 + H * 10 + I;
		int DEF = D * 100 + E * 10 + F;
		if(A * C * GHI + B * GHI + C * DEF == 10 * C * GHI){
			sum++;
		}
	}while(next_permutation(a + 1, a + 10));   //全排列函数从生成数组a中[1, 10)的排列
	cout << sum << endl;
	return 0;
}
```

**答案：29**

---

[<span id = "4">4. 快速排序</span>](#0)

排序在各种场合经常被用到。
快速排序是十分常用的高效率的算法。
其思想是：先选一个“标尺”，
用它把整个队列过一遍筛子，
以保证：其左边的元素都不大于它，其右边的元素都不小于它。
这样，排序问题就被分割为两个子区间。
再分别对子区间排序就可以了。
下面的代码是一种实现，请分析并填写划线部分缺少的代码。

```cpp
#include <stdio.h>
void swap(int a[], int i, int j) {
	int t = a[i];
	a[i] = a[j];
	a[j] = t;
}

int partition(int a[], int p, int r) {
    int i = p;
    int j = r + 1;
    int x = a[p];
    while(1){
        while(i<r && a[++i]<x);
        while(a[--j]>x);
        if(i>=j) break;
        swap(a,i,j);
    }
	______________________;
    return j;
}

void quicksort(int a[], int p, int r) {
    if(p<r){
        int q = partition(a,p,r);
        quicksort(a,p,q-1);
        quicksort(a,q+1,r);
    }
}

int main() {
	int i;
	int a[] = {5,13,6,24,2,8,19,27,6,12,1,17};
	int N = 12;
	quicksort(a, 0, N-1);
	for(i=0; i<N; i++) printf("%d ", a[i]);
	printf("\n");
	return 0;
}
```

注意：只填写缺少的内容，不要书写任何题面已有代码或说明性文字。

**解:
思路：首先了解程序中的快速排序，采用分治的思想，将数据以第一个数为基准分成两部分,例子:**
```
1. 初始：6 1 2 7 9 3 4 10 8     key = 6
6 1 2 7 9 3 4 10 8
i                  j

2. i从左边开始向右移动，遇到第一个比key大的数，停止(while(i < r && a[++i] < x))，j从最右边开始向左移动,遇到第一个比key小的数，停止(while(a[--j] > x))
6 1 2 7 9 3 4 10 8
      i     j

3. 交换i和j的位置,即代码中的swap()
6 1 2 4 9 3 7 10 8
      i     j

4. 重复2操作
6 1 2 4 9 3 7 10 8
        i j

5. 重复4操作
6 1 2 4 3 9 7 10 8
        i j

6. 重复2操作
6 1 2 4 3 9 7 10 8
        j i
```

**7. i > j, 跳出循环, 此时由于j在i的左边，所以a[j] < a[i] 并且a[j] < a[0], 否则i不会到j的右边,
swap(a, p, j)操作，将6和3交换位置, 就完成了一次快速排序, 这里的p是传入的数据的最左边位置，也就是样例数据的a[0]**

**答案：swap(a, p, j)**

---

[<span id = "5">5. 抽签</span>](#0)

X星球要派出一个5人组成的观察团前往W星。
其中：
A国最多可以派出4人。
B国最多可以派出2人。
C国最多可以派出2人。
....
那么最终派往W星的观察团会有多少种国别的不同组合呢？
下面的程序解决了这个问题。
数组a[] 中既是每个国家可以派出的最多的名额。
程序执行结果为：
DEFFF
CEFFF
CDFFF
CDEFF
CCFFF
CCEFF
CCDFF
CCDEF
BEFFF
BDFFF
BDEFF
BCFFF
BCEFF
BCDFF
BCDEF
....
(以下省略，总共101行)

```cpp
#include <stdio.h>
#define N 6
#define M 5
#define BUF 1024

void f(int a[], int k, int m, char b[]) {
	int i,j;
	if(k==N){ 
		b[M] = 0;
		if(m==0) printf("%s\n",b);
		return;
	}

	for(i=0; i<=a[k]; i++){
		for(j=0; j<i; j++) b[M-m+j] = k+'A';
		______________________;  //填空位置
	}
}

int main() {	
	int  a[N] = {4,2,2,1,1,3};
	char b[BUF];
	f(a,0,M,b);
	return 0;
}
```
仔细阅读代码，填写划线部分缺少的内容。
注意：不要填写任何已有内容或说明性文字。

**解：
思路：a数组表示各国能派出的人数，b数组表示派出的组合，在f()函数中，第一个循环是遍历各国，第二个循环是各国派出的人数，这里明显是一个递归方程，k应该不断增加1，才能遍历完每一个国家，由于b中存储各国的人员，所以循环j次，就代表派出了j个人，m - j就表示还需要派出多少人，观察if内部程序，当k == N时表示对国家选择完，然后再看各国派出的人数是否达到了人，，当m == 0时，表示一种情况生成完毕，输出b数组**

**答案：f(a, k + 1, m - j, b)**

---


[<span id = "6">6. 方格填数</span>](#0)

如下的10个格子
```
   +--+--+--+
   |  |  |  |
+--+--+--+--+
|  |  |  |  |
+--+--+--+--+
|  |  |  |
+--+--+--+
```

填入0~9的数字。要求：连续的两个数字不能相邻。
（左右、上下、对角都算相邻）
一共有多少种可能的填数方案？
请填写表示方案数目的整数。
注意：你提交的应该是一个整数，不要填写任何多余的内容或说明性文字。

**解：
思路：这题直接暴力求解，全排列所有情况，然后一个个试就完事，判断每一个格子与周围格子的绝对值之差是否为1，如果为1就返回false, 否则返回true，每次以当前格子为基础，向四周比较，比较完就把当前格子划掉就不会漏掉。**


```
   +--+--+--+                 +--+--+--+
   |a |b |c |                 |  |b |c |  
+--+--+--+--+  比较完a  => +--+--+--+--+,  
|d |e |f |g |              |d |e |f |g |
+--+--+--+--+              +--+--+--+--+
|h |i |j |                 |h |i |j |
+--+--+--+                 +--+--+--+   
```

```cpp
#include <iostream>
#include <cmath>
#include <algorithm>
using namespace std;

bool can(int a, int b){
	return abs(a - b) == 1 ? false : true;   //判断格子是否相邻
}

int main(){
	int array[10];
	for(int i = 0; i <= 9; i++)   //初始化数组array
		array[i] = i;
	int sum = 0;

	do{
		int a = array[0], b = array[1], c = array[2], d = array[3], e = array[4], f = array[5], g = array[6], h = array[7], i = array[8], j = array[9];
	
		//一个个比较
		if(can(a, b) && can(a, d) && can(a, e) && can(a, f) && can(b, e) && can(b, f) && can(b, c) && can(b, g) && can(c, f) && can(c, g) && can(d, h) && can(d, e) && can(d, i) && can(e, i) && can(e, h) && can(e, f) && can(e, j) && can(f, i) && can(f, j) && can(f, g) && can(g, j) && can(h, i) && can(i, j))  
			sum++;
	}while(next_permutation(array, array + 10));   //生成全排列
	cout << sum << endl;
	return 0;
}
```

**答案：1508**

---

[<span id = "7">7. 剪邮票</span>](#0)

如,![][picture1] 有12张连在一起的12生肖的邮票。
现在你要从中剪下5张来，要求必须是连着的。
（仅仅连接一个角不算相连）
比如，![][picture2]，![][picture3]中，粉红色所示部分就是合格的剪取。
请你计算，一共有多少种不同的剪取方法。
请填写表示方案数目的整数。
注意：你提交的应该是一个整数，不要填写任何多余的内容或说明性文字。

**解：
思路：将邮票的标号改为1, 2, 3, 4, 6, 7, 8, 9, 11, 12, 13, 14, 这样上下相差5，左右相差1，下左边界和上右边界的数就相差2，便于判断，暴力循环求所有情况生成的数组c，dfs搜索c判断邮票是否连接，标号+{-1, 1, -5, 5}表示上下左右移动，已知c数组的第一个数如果该数经过移动操作能和c中第二个数相等，那么表示两个邮票连接**

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;

int visit[5];
int a[] = {1, 2, 3, 4, 6, 7, 8, 9, 11, 12, 13, 14}; 
int b[] = {-1, 1, -5, 5};
int c[5];

void dfs(int n){
    for(int i = 0; i < 4; i++) {
        int t = c[n] + b[i];
        if(t < 1 || t > 14 || t == 5 || t == 10) continue;
        for(int j = 0; j < 5; j++)
            if(!visit[j] && c[j] == t) {
                visit[j] = 1;
                dfs(j);
            }
    }
}

int main(){
	int sum = 0;
	for(int i = 1; i <= 14; i++)
		for(int j = i + 1; j <= 14; j++)
			for(int k = j + 1; k <= 14; k++)
				for(int p = k + 1; p <= 14; p++)
					for(int q = p + 1; q <= 14; q++) {
					c[0] = a[i];
					c[1] = a[j];
					c[2] = a[k];
					c[3] = a[p];
					c[4] = a[q];
					memset(visit, 0, sizeof(visit));
					visit[0] = 1;
					dfs(0);
					int flag = 0;
					for(int i = 0; i < 5; i++){
						if(visit[i] == 1){
							flag++;
						}
					}
					if(flag == 5)
						sum++;
				}
	cout << sum << endl;
	return 0;
}
```
**答案：116**

---

[<span id = "8">8. 四平方和</span>](#0)

四平方和定理，又称为拉格朗日定理：
每个正整数都可以表示为至多4个正整数的平方和。
如果把0包括进去，就正好可以表示为4个数的平方和。
比如：
5 = 0^2 + 0^2 + 1^2 + 2^2
7 = 1^2 + 1^2 + 1^2 + 2^2
（^符号表示乘方的意思）
对于一个给定的正整数，可能存在多种平方和的表示法。

要求你对4个数排序：
0 <= a <= b <= c <= d
并对所有的可能表示法按 a,b,c,d 为联合主键升序排列，最后输出第一个表示法

程序输入为一个正整数N (N<5000000)

要求输出4个非负整数，按从小到大排序，中间用空格分开
例如，输入：
5
则程序应该输出：
0 0 1 2

再例如，输入：
12
则程序应该输出：
0 2 2 2

再例如，输入：
773535
则程序应该输出：
1 1 267 838

资源约定：
峰值内存消耗 < 256M
CPU消耗  < 3000ms

**解：
思路：暴力枚举，附加一些条件即可。每一层循环的终点就是剩余的值，比如第一层循环i, 终点为sqrt(n)，需要遍历0 ~ sqrt(n), 第二层循环ｊ，终点为sqrt(n - i * i), 因为已经有一个i值，不需要从0 ~ sqrt(n)，第三层循环就为0 ~ sqrt(n - i * i - j * j)，第四层就不需要循环了，因为，虽然有四个未知数,，但只要知道其中三个，另一个就可以直接求出来，只需要判断第四个数的开方是否为整数就行了**

```cpp
#include <iostream>
#include <cmath>
using namespace std;

int main(){
	int n;
	cin >> n;
	for(int i = 0; i <= sqrt(n); i++){
		for(int j = 0; j <= sqrt(n - i * i); j++)
			for(int k = 0; k <= sqrt(n - i * i - j * j); k++){
				double s = 1.0 * sqrt(n - i * i - j * j - k * k);
				if(s == (int)s){
					cout << i << " " << j << " " << k << " " << s << endl;
					return 0;
				}
			}
	}
	return 0;
}
```
---

[<span id = "9">9. 交换瓶子</span>](#0)

有N个瓶子，编号 1 ~ N，放在架子上。
比如有5个瓶子：
2 1 3 5 4
要求每次拿起2个瓶子，交换它们的位置。
经过若干次后，使得瓶子的序号为：
1 2 3 4 5
对于这么简单的情况，显然，至少需要交换2次就可以复位。
如果瓶子更多呢？你可以通过编程来解决。

输入格式为两行：
第一行: 一个正整数N（N<10000）, 表示瓶子的数目
第二行：N个正整数，用空格分开，表示瓶子目前的排列情况。
输出数据为一行一个正整数，表示至少交换多少次，才能完成排序。

例如，输入：
5
3 1 2 5 4
程序应该输出：
3

再例如，输入：
5
5 4 3 2 1
程序应该输出：
2

资源约定：
峰值内存消耗 < 256M
CPU消耗  < 1000ms

**解：
思路：求最少交换次数，那么每次交换的结果必须是最终的位置，那么只需要通过选择排序，每次都从后面挑选出最小的数换到前面它所在的位置就行了.**

```cpp
#include <iostream>
using namespace std;

int main(){
	int n;
	cin >> n;
	int a[n];
	for(int i = 0; i < n; i++)
		cin >> a[i];
	int min, flag = 0;
	int sum = 0;
	for(int i = 0; i < n; i++){
		min = a[i];
		flag = i;
		for(int j = i; j < n; j++)
			if(min > a[j]){
				min = a[j];
				flag = j;
			}
		if(flag != i){
			sum++;
			a[flag] = a[i];
			a[i] = min;
		}
	}
	cout << sum << endl;
	return 0;
}
```

[<span id = "10">10. 最大比例</span>](#0)

X星球的某个大奖赛设了M级奖励。每个级别的奖金是一个正整数。
并且，相邻的两个级别间的比例是个固定值。
也就是说：所有级别的奖金数构成了一个等比数列。比如：
16,24,36,54
其等比值为：3/2
现在，我们随机调查了一些获奖者的奖金数。
请你据此推算可能的最大的等比值。

输入格式：
第一行为数字 N (0< N <100)，表示接下的一行包含N个正整数
第二行N个正整数Xi(Xi<1 000 000 000 000)，用空格分开。每个整数表示调查到的某人的奖金数额

要求输出：
一个形如A/B的分数，要求A、B互质。表示可能的最大比例系数
测试数据保证了输入格式正确，并且最大比例是存在的。

例如，输入：
3
1250 200 32

程序应该输出： 
25/4

再例如，输入：
4
3125 32 32 200
程序应该输出：
5/2

再例如，输入：
3
549755813888 524288 2
程序应该输出：
4/1

资源约定：
峰值内存消耗 < 256M
CPU消耗  < 3000ms

```cpp
#include <iostream>
#include <cmath>
#include <algorithm>
using namespace std;

typedef long long ll;

struct Node{
	ll p;
	ll q;
};

ll gcd(ll a, ll b){
	return b == 0 ? a : gcd(b, a % b);
}

int main(){
	ll n;
	cin >> n;
	ll a[n];
	Node b[n];
	for(ll i = 0; i < n; i++)
		cin >> a[i];
	sort(a, a + n);  //对a数组排序
	ll size = 0;
	for(ll i = 1; i < n; i++){  //剔除重复的元素
		if(a[size] != a[i]){
			a[++size] = a[i];
		}
	}
	if(a[0] == a[size])    //如果最终只有一个元素,那么直接输出
		cout << 1 << "/" << 1 << endl;
	else{
		for(ll i = 1; i <= size; i++){   //求出比值的分子分母
			ll g = gcd(a[i], a[i - 1]);
			b[i - 1].p = a[i] / g;
			b[i - 1].q = a[i - 1] / g;
		}
		ll pq1, pq2;
		ll A, B;
		double t = 999999999, t1;
		for(ll i = 0; i < size; i++)   //求出最小的比例系数
			for(ll j = i; j < size; j++){
				if(b[i].p * b[j].q > b[i].q * b[j].p){
					pq1 = b[i].p * b[j].q;
					pq2 = b[i].q * b[j].p;
					t1 = pq1 / pq2; 
				}
				else if(b[i].p * b[j].q < b[i].q * b[j].p){
					pq1 = b[i].q * b[j].p;
					pq2 = b[i].p * b[j].q;
					t1 = pq1 / pq2;
				} 
				else{
					pq1 = b[i].p;
					pq2 = b[i].q;
					t1 = pq1 / pq2;
				}
				if(t1 < t){
					t = t1;
					A = pq1;
					B = pq2;
				}
			}
		ll g = gcd(A, B);   //对系数进行化简
		cout << A / g << "/" << B / g << endl;
	}
	return 0;
}
```


[picture1]:data:image/png;base64,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

[picture2]:data:image/png;base64,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

[picture3]:data:image/png;base64,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


