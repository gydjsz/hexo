---
title: 2019牛客暑期多校训练营（第一场）E
mathjax: true
tags: 
- 2019牛客暑期多校训练营（第一场）
- 动态规划
---

[ABBA](https://ac.nowcoder.com/acm/contest/881/E)

# 题意:
长度为2 * (n + m)的字符串,由字符'A'和'B'组成,求其所有组合情况中有n个"AB"和m个"BA"子序列的数量

1. 0 <= n, m <= $10^3$
   
2. 最多2019个测试用例，最多20个测试用例max{n，m} > 50
3. 所有可能的字符串数mod $10^9$ + 7

<!--more-->

```
样例:
1 2
1000 1000
0 0

输出:
13
436240410
1
```

## 解释:

1 2

1个'AB'和2个'BA'

以下是所有情况数:

```
ABABAB
ABABBA
ABBAAB
ABBABA
ABBBAA
BAABAB
BAABBA
BABAAB
BABABA
BABBAA
BBAAAB
BBAABA
BBABAA
```

由于是子序列, 而且只要含有就行
对于ABABAB, 可以第一个A和最后一个B,中间的变成BABA, 就有1个'AB', 2个'BA', 其它同理

共有13个,答案为13

---

0 0

0个'AB'和0个'BA', 就为空,这一种情况,
所以答案为1

# 解法

## 思路
设dp[i][j]: i个A和j个B
+ 当前位置为A: dp[i + 1][j]
+ 当前位置为B: dp[i][j + 1]

设之前的字符串中有i个A和j个B,i - j = k,这k个A只能与后面的B匹配,而只有n个AB匹配, 所以k < n,只要满足i - j < n,就算一种情况,同理j - i < m

所以可以得出
```cpp
if(i - j < n) dp[i + 1][j] += dp[i][j];
if(j - i < m) dp[i][j + 1] += dp[i][j];
```

## 代码:

```cpp
#include <cstring>
#include <iostream>
using namespace std;

int n, m;
const int mod = 1e9 + 7;
const int maxn = 2e3 + 10;
int dp[maxn][maxn];

int solve() {
    memset(dp, 0, sizeof(dp));
    //初始化,0个AB和0个BA情况为1
    dp[0][0] = 1;
    for (int i = 0; i <= n + m; i++) {
        for (int j = 0; j <= n + m; j++) {
            if (i - j < n)
            //新加入A后的情况数
                dp[i + 1][j] += dp[i][j];
            if (j - i < m)
                dp[i][j + 1] += dp[i][j];
            dp[i + 1][j] %= mod;
            dp[i][j + 1] %= mod;
        }
    }
    cout << dp[n + m][m + n] << endl;
    return 0;
}

int main() {
    while (cin >> n >> m) {
        solve();
    }
    return 0;
}
```



