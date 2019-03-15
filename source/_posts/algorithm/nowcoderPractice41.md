---
title: 牛客练习赛41的E题题解
---

# E题
某天lililalala正在玩一种奇妙的吃鸡游戏--因为在这个游戏里会同时有两个圆形安全区(他们可能相交)
lililalala觉得求圆的面积并太简单了，所以想把这个问题升级一下。
现在在三维空间里有 2 个球形安全区，分别用四元组<x1, y1, z1, r1>和<x2, y2, z2, r2>表示，其中 r1、r2表示球半径，<x1, y1, z1>和<x2, y2, z2>表示球心
lililalala想知道安全区的总体积是多少？即求这两个球的体积并。

输入描述:
输入有两行。
第一行四个实数x1,y1,z1,r1--第一个球的球心坐标和半径。
第二行四个实数x2,y2,z2,r2--第一个球的球心坐标和半径。
保证所有输入的坐标和半径的范围都在[−100,100] 内。

输出描述:
输出一行一个实数--表示两个球的体积并，你的答案被认为正确，当且仅当绝对误差不超过10^−6。
输入
0 0 0 1
2 0 0 1
输出
8.3775804

输入
0 0 0 1
0 0 0 0.5

输出
4.1887902

思路：设两个球体的体积为Va, Vb, 球心之间的距离为d，球体存在三种情况，相离/外切，相交, 内切
1. 相离/外切，直接求两球体积之和

2. 相交，先求两球体积之和，再减去公共部分, 公共部分为两个球缺([球缺知识](./math.md)) 
Va = π * h² * (3 * r - h) / 3

距离ｄ可以用两点间的距离公式求解
h 未知，现在求h：
<img src = "https://img-blog.csdnimg.cn/20190301215408804.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoZW55dW1l,size_16,color_FFFFFF,t_70" />
```
h = r1² - x1²
h = r2² - x2²
=> r1² - x1² = r2² - x2²
=> r1² - r2² = x1² - x2²
=> r1² - r2² = (x1 + x2) * (x1 - x2)
=> r1² - r2² = d * (x1 - (d - x1))
=> r1² - r2² = d * (2 * x1 - d)
=> x1 = (r1² - r2² + d²) / (2 * d)
=> h = r1² - (r1² - r2² + d²) / (2 * d)
```
	
3. 内切，分别求两球体积，体积大的就是两球的并

```cpp
#include <iostream>
#include <cmath>
#include <iomanip>
using namespace std;

const double PI = acos(-1);

int main(){
	double x1, y1, z1, r1;
	double x2, y2, z2, r2;
	cin >> x1 >> y1 >> z1 >> r1;
	cin >> x2 >> y2 >> z2 >> r2;
	double d = sqrt(pow(x1 - x2, 2) + pow(y1 - y2, 2) + pow(z1 - z2, 2));
	double area;
	if(d >= r1 + r2){
		area = 1.0 * 4 * PI * (pow(r1, 3) + pow(r2, 3)) / 3;
	}
	else if(abs(r1 - r2) < d && d < r1 + r2){
		double h1 = r1 - (r1 * r1 - r2 * r2 + d * d) / (2 * d);
		double h2 = r2 - (r2 * r2 - r1 * r1 + d * d) / (2 * d);
		double qiuque1 = PI * h1 * h1 * (3 * r1 - h1) / 3;
		double qiuque2 = PI * h2 * h2 * (3 * r2 - h2) / 3;
		double areaSum = 1.0 * 4 * PI * (pow(r1, 3) + pow(r2, 3)) / 3;
		area = areaSum - qiuque1 - qiuque2;
	}
	else{
		double area1 = 1.0 * 4 * PI * pow(r1, 3) / 3;
		double area2 = 1.0 * 4 * PI * pow(r2, 3) / 3;
		area = max(area1, area2);
	}
	cout << fixed << setprecision(7) << area << endl;
	return 0;
}
```

