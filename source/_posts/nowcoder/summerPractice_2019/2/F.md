---
title: 2019牛客暑期多校训练营（第二场）F
tags:
- 2019牛客暑期多校训练营（第二场）
- 深度搜索
---

# 题意

给定2n人，您需要将他们分配给红队或白队，这样每个队伍都由N人组成，
总竞争价值最大化。总竞争价值是不同团队中每对人的竞争价值的总和。

等效方程是$\sum_{i=1}^{2n}\sum_{j=i+1}^{2n}$(如果第i个人与第j个人不在同一团队中$v_{ij}$ = 0)

输出表示最大可能的竞争价值的整数

<!--more-->

1. $1 \le n \le 14$
2. $0 \le v_{ij} \le 10^9$
3. $v_{ij} = v_{ji}$

```cpp
样例：
2
0 1 2 3
1 0 2 3
2 2 0 2
3 3 2 0
输出：
10
```

## 解释

将1、2放一队， 3、4放一队竞争价值最大
1-3: 2
1-4: 3
2-3: 2
2-4: 3

竞争价值为10

# 解法

## 思路

由于n最大为14，2n就是28，将28人分成两个队伍，就是$C_{28}^{14}$大约4e7, 
可以使用dfs搜索所有的组合情况，搜索完成之后需要求出两个队伍之间的竞争价值,
总时间复杂度为$O(C_{28}^{14}n^2)$, 会超时。
因为dfs每次搜索时会将一个人加入队伍中，所以只需要用一个数组将该人与其他人的竞争价值总和存储下来，
每次只需要减去和自己同队伍的竞争价值即可，由于竞争价值是双向的，
所以需要减去竞争价值的2倍，例如队伍中有1，加入2，
那么新加入的竞争价值中包含有开始加入的1-2，和后来加入的2-1两个重复的竞争价值，由于同队，所以都变为0

## 代码

```cpp
#include <iostream>
#include <vector>
using namespace std;

typedef long long ll;
const int maxn = 30;
int n;
ll a[maxn][maxn];  //竞争价值
ll value[maxn];    //每个人对其它所有人的竞争价值的和
ll res;      //结果
vector<int> v;  //加入队伍的情况

int dfs(ll index, ll ans) {
	//如果队伍中已经加入了n个人，则判断最大值
	if (v.size() == n) {
		res = max(ans, res);
		return 0;
	}
	//如果当前队伍人数加上剩余的人数都小于n，则返回
	if (v.size() + 2 * n - index < n) {
		return 0;
	}
	//当前人对其它所有人的竞争价值总和
	ll now = value[index];
	//加入队伍后需要减去其它人对当前人的竞争价值和当前人对其它人的竞争价值
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		now -= 2 * a[index][*it];
	}
	//将当前人加入队伍
	v.push_back(index);
	//继续往后dfs，同时下标+1，竞争价值也加进来
	dfs(index + 1, ans + now);
	v.pop_back();
	dfs(index + 1, ans);
	return 0;
}

int main() {
	cin >> n;
	for (int i = 1; i <= 2 * n; i++) {
		for (int j = 1; j <= 2 * n; j++) {
			cin >> a[i][j];
			//计算i和其它人竞争价值的和,此时是把i和其它人都不在一个队伍看待
			value[i] += a[i][j];
		}
	}
	dfs(1, 0);
	cout << res << endl;
	return 0;
}
```
