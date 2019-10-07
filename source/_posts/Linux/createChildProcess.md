---
title: 创建子进程
tags:
- linux进程
---

1. 创建一个子进程并求1~100的和

<!--more-->

文件cal.c

```c
#include <sys/types.h>
#include <stdio.h>
#include <unistd.h>

void solve(){
	int sum = 0;
	for(int i = 1; i <= 100; i++)
		sum += i;
	printf("%d\n", sum);
}

int main(){
	pid_t pid;
	pid = fork();
	if(pid < 0){
		fprintf(stderr, "Fork Failed");
		return 1;
	}
	else if(pid == 0){
		printf("child: ");
		solve();
	}
	else{
		wait(NULL);
		printf("parent\n");
	}
	return 0;
}
```

2. 执行:

gcc cal.c
./a.out

3. 利用gdb调试进程

**gcc cal.c -g**生成调试信息
输入**gdb**进入调试
**file a.out**加载调试文件
![](c1.png)

**l**查看代码, **b 行数**添加断点
![](c2.png)

**set follow-fork-mode child**跟踪子进程(该命令在fork之前执行, 设置parent则跟踪父进程)
**show follow-fork-mode**查看跟踪的进程
![](c3.png)

**r**运行程序
**p pid**打印pid信息
**n**执行下一步
![](c4.png)

执行结果
![](c5.png)

在fork函数执行之后会返回一个值
-1即fork失败，值为0时即子进程，返回值大于0即子进程id

