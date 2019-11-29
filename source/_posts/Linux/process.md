---
title: linux进程
tags:
- linux 
---

## 基础知识

### 进程简介

４个要素:
> - 要有一段程序供该进程运行
> - 进程专用的系统堆栈空间
> - 进程控制块
> - 有独立的存储空间

当进程缺少其中的某一个要素时，称为线程

<!--more-->

Linux系统包括３中不同类型的进程:
> - 交互进程: 由一个shell启动的进程。交互进程既可以在前台运行，也可以在后台运行
> - 批处理进程: 这中进程和终端没有联系，是一个进程序列
> - 监控进程(守护进程): Linux系统启动时启动的进程，并在后台运行 

### 进程状态

+ 新的(new): 进程正在创建
+ 运行(running): 指令正在执行
+ 等待(waiting): 进程等待发生某个事件
+ 就绪(ready): 进程等待分配处理器
+ 终止(terminated): 进程已经完成执行

### 进程标识

> 在Linux中，每个进程在创建时都会被分配一个数据结构，称为进程控制块(Process Control Block, PCB)
> 用它来记录进程的外部特征，描述进程的运动变化过程。
> 系统利用PCB来控制和管理进程，所以PCB是系统感知进程的唯一标志
> 进程与PCB是一一对应的关系

进程ID 0是调度进程，常常被称为交换进程
init进程的PID为1,它可以创建各种用户进程, 是所有用户进程的父进程

### 上下文切换

切换CPU到另一个进程需要保存当前进程状态和恢复另一个进程的状态,这个任务称为上下文切换

---

## 函数

> + getpid(): 返回进程的pid

> + system(): 执行shell命令

```cpp
#include <stdlib.h>
system("ls");  //执行ls命令查看当前目录下的内容
```

+ exec函数

fork函数创建新进程后，经常会在新进程中调用exec函数去执行另外一个程序。

当进程调用exec函数时，该进程被完全替换为新程序。因为调用exec函数并不创建新进程，所以前后进程的ID并没有改变

1. int execl(const char *path, const char *arg, ...)

第一参数path字符指针所指向要执行的文件路径， 接下来的参数代表执行该文件时传递的参数列表：argv[0],argv[1]... 最后一个参数须用空指针NULL作结束


```cpp
#include <unistd.h>

int main() {
    execl("/bin/ls", "ls", "-al", NULL);
    return 0;
}
```

2. int execlp(const char * filename,const char * arg,...)

从PATH环境变量所指的目录中查找符合参数file的文件名,后面是参数列表

```cpp
#include <unistd.h>

int main() {
    execlp("ls", "ls", NULL);
    return 0;
}
```

3. int execle(const char* pathname, const char* arg,..., char* const envp[])

4. int execv(const char* pathname, char* const argv[])

5. int execvp(const char* filename, char* const argv[])

6. int execve(const char* pathname, char* const argv[], char* const envp[])

+ fork()

```cpp
pid_t p;
p = fork();
```

> 若成功,父进程中返回子进程ID, 子进程中返回0;若出错则返回-1

testFork.c文件
```cpp
#include <unistd.h>
#include <sys/wait.h>
#include <sys/types.h>
#include <stdlib.h>
#include <stdio.h>

int main(){
	pid_t p;
    //执行fork之后会创建一个子进程,此时有父进程和子进程两个进程
	p = fork();
    //子进程和父进程执行下列代码
	if(p < 0){
		fprintf(stderr, "fork failed");
		return -1;
	}
	else if(p == 0){
        //子进程执行
		printf("child\n");
	}
	else{
        //父进程执行
		wait(NULL);
		printf("parent\n");
	}
	return 0;
}
```

+ wait()

  pid_t wait(int* status)
pid_t waitpid(pid_t pid, int*  status, int options)

成功执行返回进程id,出错返回-1

> 进程一旦调用了wait,就立即阻塞自己,由wait自动分析当前进程的某个子进程已经退出,如果找到这样一个僵尸进程,wait就会收集这个子进程的信息,并把它彻底销毁后返回;如果没有找到,wait就会一直阻塞这里,直到有一个出现为止 


> + 如果参数status是一个空指针,则表示父进程不关心子进程的终止状态

> + 如果参数status不是一个空指针,则status存放终止进程的状态

如果没有子进程,wait调用就会失败,此时返回-1

## 进程终止

> + 当进程完成执行最后语句并且通过系统调用exit()请求操作系统删除自身时,进程终止.
> + 此时,进程可以返回状态值到父进程(通过系统调用wait()), 所有进程资源,如物理虚拟内存、打开文件和I/O缓冲区等，会由操作系统释放。

在正常终止时，可以直接调用exit(), 也可以间接调用(通过main的返回语句)

> 父进程可以通过调用wait(), 等待子进程的终止.
> 系统调用wait()可以通过参数,让父进程获得子进程的退出状态;这个系统调用也返回终止子进程的标识符

```cpp
pid_t p;
int status;
pid = wait(&status);
```

> 当一个进程终止时,操作系统会释放其资源,但它位于进程表中的条目还是在的.直到它的父进程调用wait(),这是因为进程表中包含了进程的退出状态

> 当进程已经终止,但其父进程未调用wait(),这样的进程称为**僵尸进程**
所有进程终止时都会过渡到这种状态,一旦父进程调用了wait(),僵尸进程的进程标识符和进程表中的条目都会释放

> 如果父进程没有调用wait(),就终止,子进程就称为**孤儿进程**

+ Linux中是这样处理的:
> init成为孤儿进程的父进程,由于init进程定期调用wait(),以便收集任何孤儿进程的退出状态,并释放孤儿进程的标识符和进程表条目

这里我们可以这样验证一下:

写入文件testChildFork.c
```cpp
#include <unistd.h>
#include <stdio.h>
#include <sys/types.h>
#include <sys/wait.h>

int main(){
	pid_t p;
	p = fork();
	if(p < 0){
		fprintf(stderr, "Fork Failed!");
		return 1;
	}
	else if(p == 0){
		while(1);
	}
	else{
		while(1);
	}
	return 0;
}
```

执行
> gcc testChildFork.c  
> ./a.out &

这里&符号是将程序运行到后台

程序中子进程和父进程都执行while(1),保证了进程一直执行

执行程序之后,我们使用ps -ef | grep ./a.out命令,在终端打印了该进程的信息

![](p1.png)

进程有两个,一个父进程,一个子进程
root后面的两列数字分别为pid和ppid,即进程id,和父进程id
很明显,pid为20149的为子进程,因为其ppid为20146,是第一个进程的pid

我们使用kill -9 20146杀死父进程之后,再来查看子进程的pid

![](p2.png)

可以看到,此时子进程的ppid为1,说明子进程将init进程作为其父进程

最后用kill -9 20149将该子进程杀死

## 进程间通信

### 共享内存

建立一块供协作进程共享的内存区域,进程通过向此共享内存区域读出或写入数据来交换信息
                                     

### 消息传递

通过在协作进程间交换信息来实现通信

