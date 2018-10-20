---
title: 取物游戏
---
100个物品，两个玩家a、b轮流从这堆物品中取物，规定每次可以取的数目可以是1、3、4、6.最后一次取光者得胜。问为了获胜应先取还是后取，应采取什么策略？

```c++
#include <iostream>
using namespace std;
const int MAX = 100;
int f[MAX];
int game(int n){
	int i;
	f[0] = 0;
	f[1] = f[3] = f[4] = f[6] = 1;
	f[2] = 0;
	f[5] = 1;
	for(i = 7; i <= n; i++)
		f[i] = !(f[i - 1] && f[i - 3] && f[i - 4] && f[i - 6]);
	return f[n];
}
int main(){
	int n;
	cin >> n;
	cout << game(n) << endl;
	return 0;
}
```
