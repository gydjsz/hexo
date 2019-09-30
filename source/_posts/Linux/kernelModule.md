---
title: linux添加内核模块
tags:
- linux
---

# 显示内核模块

lsmod

<!--more-->

# 编写内核模块代码

```
#include <linux/init.h>
#include <linux/kernel.h>
#include <linux/module.h>

int simple_init(void){
	printk(KERN_INFO "Loading Module\n");
	printk(KERN_INFO "hello, world!\n");
	return 0;
}

void simple_exit(void){
	printk(KERN_INFO "Removing Module\n");
	printk(KERN_INFO "GoodBye!\n");
}

module_init(simple_init);
module_exit(simple_exit);

MODULE_LICENSE("GPL");
MODULE_DESCRIPTION("Simple Module");
MODULE_AUTHOR("SGG");
```

# 编译内核模块

创建**Makefile**文件,并写入内容

```
obj-m+=simple.o
dir=/lib/modules/$(shell uname -r)/build/
app: ${dir} 
	make -C ${dir} M=$(PWD)
clean:
	make -C ${dir} M=$(PWD) clean
```

+ 编译命令

> make

+ 清理生成的文件

> make clean

# 加载内核模块

sudo insmod simple.ko

# 查看内核模块

dmesg

# 卸载内核模块

sudo rmmod simple

# 清除缓冲区

sudo dmesg -c
