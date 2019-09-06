---
title: 2019牛客暑期多校训练营（第二场）H
tags:
- 2019牛客暑期多校训练营（第二场）
- 单调栈
---

# 题意

给定一个n * m的二进制矩阵,输出全部为1的第二大矩阵

1. 两个矩阵有一块不同,则视为不同的矩阵
2. $1 \le n, m \le 1000$
3. $n * m \ge 2$
4. $C_{ij} \in \{0, 1\}$ 

<!--more-->

```
样例:
1 3
101
输出:
1
```

第一个1和第三个1最大,
所以第二大为1

# 解法

## 思路

<img src="monotoneStack.png" width="30%" height="30%"/>

使用单调栈求图中矩形的面积,如图

2 3 3 2
求出每个矩形位置的最大可扩展左右边界
对于第一个2和最后一个2来说,它们的左右边界最大都是
[1, 4],所以面积为2 \* 4 = 8
对于中间的3来说,它们的左右边界最大都是[2, 3]
所以面积为3 \* 2 = 6

通过单调栈可以求出每个位置的最大可扩展边界
然后用左右边界的间隔乘以高度就是面积,不断更新最大面积就是结果

1. 将数组中的每一列从上往下加和,形成的每一行可以看做矩形的高度
2. 对于每一行来说,可以使用单调栈,求出每个数的左右边界

用一个栈s来存储每个数的下标(这里方便说明,所以栈顶元素指的是a[s.top()], 下标是s内的数据)
对于左边界:
- 从左向右遍历
- 当栈为空时,下标为最左边,即1
- 当栈不为空时,如果栈顶元素大于等于当前数,则不断弹出栈顶元素,最后得到的就是小于当前数的数,此时将栈顶元素的下标+1入栈.
因为只要栈中的元素比当前数大,那么当前数就可以向左扩展,例如3 2 1, 对于右边的1来说,它的左边界是3,因为3和2都比1大,可以形成1 1 1的矩形
这里+1, 是因为栈顶的数小于当前数,所以将栈顶下标+1入栈

```cpp
 while (!s.empty() && height[i][s.top()] >= height[i][j])
    s.pop();
L[j] = s.empty() ? 1 : s.top() + 1;
s.push(j);
```

对于右边界:
- 从右向左遍历
- 当栈为空时,下标为最右边,即m
- 当栈不为空时,如果栈顶元素大于等于当前数,则不断弹出栈顶元素,最后得到的就是小于当前数的数,此时将栈顶元素的下标-1入栈
因为只要栈中的元素比当前数大,那么当前数就可以向右扩展,例如1 2 3, 对于2来说,它的右边界是3,所以可以弹出3,2扩展到3的位置,可以形成2 2的矩形
这里-1,是因为从右向左遍历,栈顶的元素小于当前数,所以是栈顶下标-1

```cpp
while (!t.empty() && height[i][t.top()] >= height[i][j])
    t.pop();
R[j] = t.empty() ? m : t.top() - 1;
t.push(j);
```

对于每一行来说都可以求得当前行的每一列的左右边界,所以可以在求出左右边界之后,就计算面积

```cpp
area = (R[j] - L[j] + 1) * height[i][j]
```

使用res1和res2分别记录第一大和第二大面积

由于题目要求第二大边界,所以
1. 判断area是不是比res1大,是则更新res2为res1,res1为area,这里忘记将res2更新为res1,结果卡了很久,否则和第二大比较
2. 如果比res2小,肯定不更新,如果比res2大,这里可能和res1的边界是一样的,也就是同一个矩形,例如2 2 2,三个2的左右边界都是[1, 3]
+ 因为area和第一个是相等的,而边界也是一样的,所以不能将res2更新
+ 所以可以用rL和rR分别记录该左右边界,在判断area小于res1且大于res2时,可以判断是否是同一个边界
+ 如果area是相同的,边界情况也是相同的,那么可能出现的行不同,例如:
```
1 1 1
0 0 0
1 1 1
```
对于这种情况,第一行和第三行的边界情况相同,但是出现的行不同,算作不同的矩形
+ 可以用一个index记录最大值所在的行,所以在$res1 \le area \le res2$时,判断左右边界和行数不同时相等,则该area可以作为第二大面积
```cpp
int rL = 0, rR = 0, index = 0;
int area = (R[j] - L[j] + 1) * height[i][j];
if(area > res1){
    res2 = res1;
    res1 = area;
    index = i;
    rL = L[j];
    rR = R[j];
}
else if(area > res2 && (L[j] != rL || R[j] != rR || index != i)){
    res2 = area;
}
```
1. 由于每个位置都是求最大可能达到的矩形,而前面是将重复的矩形舍去,而第二大矩形可能是第一大矩形的子矩形
例如: 
1 1 1
每个位置的边界都为[1, 3],最大矩形面积为3,次大的矩形应该是最大矩形的子矩形,即1 1, 面积为2
由于子矩形可能是宽度减少或者高度降低
所以可以将第二大矩形的面积和宽度减小1和高度减小1的矩形面积进行比较

```cpp
int areaHeight = (R[j] - L[j] + 1) * (height[i][j] - 1);
int areaWight = (R[j] - L[j]) * height[i][j];
res2 = max(res2, max(areaHeight,areaWight));
```
4. 如果area不是最大,但比res2大,那么不论该位置的边界是否相同,该面积都是第二大,所以不需要判断第二大面积的边界是否相同

## 代码
```cpp
#include <iostream>
#include <stack>
using namespace std;

const int maxn = 1e3 + 10;
//输入矩形的大小
int n, m;
//记录矩形的高度
int height[maxn][maxn];
//记录每一行每个数的左右边界
int L[maxn], R[maxn];
//记录第一大和第二大
int res1, res2;

int solve() {
    for (int i = 1; i <= n; i++) {
        stack<int> s, t;
        //计算当前行的左边界
        for (int j = 1; j <= m; j++) {
            while (!s.empty() && height[i][s.top()] >= height[i][j])
                s.pop();
            L[j] = s.empty() ? 1 : s.top() + 1;
            s.push(j);
        }
        //计算当前行的右边界
        for (int j = m; j >= 1; j--) {
            while (!t.empty() && height[i][t.top()] >= height[i][j])
                t.pop();
            R[j] = t.empty() ? m : t.top() - 1;
            t.push(j);
        }
        //分别记录最大面积的左边界,右边界和行数
        int rL = 0, rR = 0, index = 0;
        for (int j = 1; j <= m; j++) {
            //计算该位置的面积
            int area = (R[j] - L[j] + 1) * height[i][j];
            //如果面积大,则更新res1和res2
            if(area > res1){
                res2 = res1;
                res1 = area;
                index = i;
                rL = L[j];
                rR = R[j];
            }
            //如果面积大于res2,同时左右边界和行数不同时相等,则与res1的矩形不一样
            else if(area > res2 && (L[j] != rL || R[j] != rR || index != i)){
                res2 = area;
            }
            //计算高度减少1和宽度减少1的次大矩形
            int areaHeight = (R[j] - L[j] + 1) * (height[i][j] - 1);
            int areaWight = (R[j] - L[j]) * height[i][j];
            res2 = max(res2, max(areaHeight,areaWight));
        }
    }
    cout << res2 << endl;
    return 0;
}

int main() {
    cin >> n >> m;
    char c;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            cin >> c;
            height[i][j] = c - '0';
            //如果height[i][j]为0,那么值为0,否则为height[i][j] + height[i - 1][j]
            height[i][j] += height[i][j] * height[i - 1][j];
        }
    }
    solve();
    return 0;
}
```

