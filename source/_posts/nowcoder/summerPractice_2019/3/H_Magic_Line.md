---
title: 2019牛客暑期多校训练营（第三场）H
mathjax: true
tags:
- 2019牛客暑期多校训练营（第三场）
---

# 题意
平面上有n个点,找出不同于n个点的任意两点画一条线,这条线将这些点分成点数相同的两部分,两点不能重合。

1. 输入T组样例, $1 \le T \le 1000$
2. $2 \le n \le 1000$
3. $|x, y| \le 1000$
4. 找出的两点坐标的绝对值$\le 10^9$

<!--more-->

```
样例:
1
4
0 1
-1 0
1 0
0 -1
输出:
-1 999000000 1 -999000001
```

## 解释

输入1,表示1组样例
下一行4,表示有4个点
输入4行, 每行一个点(x, y)
输出两个点的坐标(-1, 999000000) (1, -999000001)

# 解法

## 思路

1. 先将点x, y从小到大排序 
2. 找到中间的点,然后画一条线,将该线稍微倾斜然后平移一点即可
   
由于题中给的点范围为1000, 而找的坐标为$10^9$, 所以只需要将(x1 - 1, y1 + INF), (x2 + 1, y2 - INF)

## 代码

```cpp
#include <algorithm>
#include <iostream>
using namespace std;

const int INF = 8e8;

bool cmp(pair<int, int> a, pair<int, int> b) {
    if (a.first == b.first)
        return a.second < b.second;
    return a.first < b.first;
}

int solve() {
    int n;
    cin >> n;
    pair<int, int> a[n];
    for (int i = 0; i < n; i++)
        cin >> a[i].first >> a[i].second;
    sort(a, a + n, cmp);
    cout << a[n / 2 - 1].first - 1 << " " << a[n / 2 - 1].second + INF << " "
         << a[n / 2].first + 1 << " " << a[n / 2].second - INF << endl;
    return 0;
}

int main() {
    int T;
    cin >> T;
    while (T--) {
        solve();
    }
    return 0;
}
```
