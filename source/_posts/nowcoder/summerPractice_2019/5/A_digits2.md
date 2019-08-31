---
title: 2019牛客暑期多校训练营（第五场）A
tags:
- 2019牛客暑期多校训练营（第五场）
- 思维题
---

[](https://ac.nowcoder.com/acm/contest/885/A)

# 题意

给出一个正整数n，最多为100. 

请找到满足以下条件的正整数：

1. 该数字的所有数字的总和可以由n分割。

2. 这个数字也可以由n分割。

3. 这个数字不超过$10^4$位数。

<!--more-->

如果不存在这样的整数，则输出“不可能”（不带引号）。
如果有多个这样的整数，请输出任何一个。

1. T组样例, $1 \le T \le 100$
2. $1 \le n \le 100$

```
样例:
3
1
9
12
输出:
1
666666
888
```

## 解释
888可分为12（888 = 12 * 74），8 + 8 + 8 = 24也可分为12（24 = 12 * 2）

# 解法

## 思路

直接输出n个n,
总和为n*n可以由n分割
该数字nnnnn...显然也可以由n分割
由于n最多为100, 所以位数最多为$10^4$

```cpp
#include <iostream>
using namespace std;

int solve(){
    int n;
    cin >> n;
    for (int i = 0; i < n; i++)
        cout << n;
    cout << endl;
    return 0;
}

int main() {
    int T;
    cin >> T;
    while(T--){
        solve();
    }
    return 0;
}
```