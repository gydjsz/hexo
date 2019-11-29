---
title: c语言多线程编程
tags:
- linux 
---

# 基础知识

1. 头文件

```cpp
#include <pthread.h>
```

2. 函数

+ 使用pthread_t p声明线程id

+ pthread_create()创建一个线程  
第一个是线程ID的指针，第二个是线程属性，可以设为空，第三个是函数名，第四个是函数的参数,参数可以为空
成功返回0, 失败返回-1
```cpp
int pthread_create(pthread_t* thread，const pthread_attr_t* arr, void* fun(void* arg), void* arg);
```

C语言中void* 为 "不确定类型指针"，void* 可以用来声明指针
void* 接受任何类型的赋值
```cpp
void* a = NULL;
int b = 1;
a = b;
```

**函数必须声明为void\* fun(void\* arg),参数和返回值均为void\*形式**

+ pthread_join()用来等待一个线程的结束  
代码中如果没有pthread_join主线程会很快结束从而使整个进程结束，从而使创建的线程没有机会开始执行就结束了。
加入pthread_join后，主线程会一直等待直到等待的线程结束自己才结束，使创建的线程有机会执行。
0代表成功。失败，返回的则是错误号。

第一个为线程ID，第二个可以为空
```cpp
int pthread_join(pthread_t thread, void **retval)
```

一个简单的例子：
testThread.c
```cpp
#include <stdio.h>
#include <pthread.h>

void* func(void* arg){
        printf("Hello, World!\n");
        return NULL;
}

int main(){
        pthread_t p;
        pthread_create(&p, NULL, func, NULL);
        pthread_join(p, NULL);
        return 0;
}
```

+ 编译运行  

在使用gcc编译多线程程序时，必须与pthread库进行链接,即: -lpthread

```cpp
  gcc testThread.c -lpthread -o testThread  
  ./testThread
```

