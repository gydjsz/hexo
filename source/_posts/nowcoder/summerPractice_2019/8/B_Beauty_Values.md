---
title: 2019牛客暑期多校训练营（第八场）B
tags: 
- 2019牛客暑期多校训练营（第八场）
- 思维题 
---

# 题意
求序列中所有连续区间的不同元素数量的和  

1. 输入n表示序列长度
2. n个数
3. $1 \le a_i \le n \le 10^5$

<!--more-->

```
样例:
4
1 2 1 3
输出:
18
```

## 解释
[1, 1], [2, 2], [3, 3], [4, 4]都是1
[1, 2], [1, 3], [2, 3], [3, 4], [1 , 2], [1, 3], [2, 3], [3, 4]都是2
[1, 4], [2, 4], [1, 4], [2, 4]都是3

1 * 4 + 2 * 4 + 3 * 2 = 18

# 解法

## 思路

枚举每个数字,然后计算其对于序列的贡献值,将他们加起来就是答案.因为重复的数没有贡献,所以如果将每个数第一次出现的贡献不为0,这个数对于后面所有的数都有贡献值,那么可以记录每个数字最后一次出现的位置index,如果在后面出现了重复的,那么后面的数对于index及前面的数都没有贡献

记pos为当前位置的索引,所以n - pos + 1为后面数的数目
使用数组a[]记录每个数最后一次出现的索引
(i - a[ m ]) * (n - post + 1)就是当前数能产生的贡献

1 2 3 4 5 6
如果当前数为3, 则1 2 3 和 3 4 5 6,
3 * 4 = 12, 包含3的共12种情况

1 2 1 3 4
加入中间的1
2 1和1 3 4,
(3 - 1) * (5 - 3 + 1) = 6

2 1
2 1 3
2 1 3 4
1
1 3
1 3 4

  可能觉得奇怪,如果后面还有很多1呢.直接加入即可,因为当前1才对后面1有贡献,
后面的1加不到前面的1上来,因为有i - a[m](m为当前数, a[m]为m最后出现的位置),这个直接就是
前面的1到当前的1, 包含当前1的中间一块;也有可能觉得后面的1和前面很多数不都可以组合吗,因为前面1算的就是和后面所有数组合的个数,所以对于[m ... m]这一块在前面已经算过了

核心就是
```
    res += (n - i + 1) * (i - a[m]);
    a[m] = i;
```

## 代码
```cpp
#include <cstring>
#include <iostream>
using namespace std;

typedef long long ll;
const int maxn = 1e5 + 10;

int main() {
    int n;
    cin >> n;
    int a[maxn];
    memset(a, 0, sizeof(a));
    ll res = 0;
    ll m;
    for(int i = 1; i <= n; i++){
        cin >> m;
        //计算加入当前数后产生的贡献
        res += (n - i + 1) * (i - a[m]);
        //更新当前数出现的位置
        a[m] = i;
    }
    cout << res << endl;
    return 0;
}
```
