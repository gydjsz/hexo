---
title: make的使用
tags:
- linux
---

# 编译

**预编译处理(.c) －－> 编译、优化程序（.s、.asm）－－> 汇编程序(.obj、.o、.a、.ko) －－> 链接程序（.exe、.elf、.axf等）**

<!--more-->

<img src="https://img2018.cnblogs.com/blog/1441101/201909/1441101-20190926100019672-698822665.png" width="50%" height="50%"/>

1. g++ -E 进行**预编译**处理,此时不生成文件

通过**重定向**可以将预编译的内容输出到文件中

> g++ -E hello.cpp > text

2. g++ -S hello.cpp 进行**编译**生成.s汇编文件,打开hello.s文件可以看到汇编指令

> g++ -S hello.cpp

3. g++ -c hello.s **汇编程序**变为目标代码(机器代码)生成.o的文件

> g++ -c hello.s

4. g++ hello.o -L hello.out **链接**程序,将.o文件变成可执行程序

> g++ hello.o -L hello.out

5. 运行hello.out文件

> ./hello.out

6. g++ hello.cpp可以一步到位,生成可执行的a.out文件

> g++ hello.cpp

添加-o参数,将输出放入指定文件中

> g++ hello.cpp -o hello.out

---

# makefile文件的编写

## 三要素

目标、依赖、命令

格式：
```
目标：依赖
<tab>命令
```

例子：
```cpp
app: hello.o
    g++ hello.cpp -o app 
```

执行make命令，就可以生成app可执行文件

## make工作方式
    
+ make会在当前目录寻找makefile文件
+ 找到makefile后，执行makefile中的命令
+ 首先查找目标后的依赖是否存在，不存在就向下寻找新规则来生成依赖，找到依赖后执行命令
   
<img src="https://img-blog.csdn.net/20181002091509553" width="50%" height="50%"/>

例子：
```
app: main 
	echo 1
main:
	echo 2
```

echo是shell的输出命令,首先寻找app目标的main依赖，没有，则向下寻找，main目标的依赖为空，则执行echo 2，此时找到main，然后执行app的命令echo 1

我们执行一下make, 控制台显示先执行echo 2，再执行echo 1，输出为2 1

## make命令

```
-f filename：指定“makefile”文件；
-c dirname：指定make在开始运行之后的工作目录为dirname
-k：执行命令出错时，放弃当前目标，继续维护其它目标
-n：非执行模式，输出所有执行命令，但并不执行；
-p：输出所有宏定义和目标文件描述；
-i：忽略命令执行返回的出错信息；
-s：沉默模式，执行但不显示命令信息
-S: 执行出错就退出
-t：更新目标文件；
-q：make操作将根据目标文件是否已经更新返回"0"或非"0"的状态信息；
-d：Debug模式，输出有关文件和检测时间的详细信息。
-w：在处理 makefile 之前和之后，都显示工作目录。
```

## 支持通配符
   
> "*",  "?",  "[]"

## 伪目标

```
clean:
    rm *.o
```

执行make clean，可以删除所有.o文件

+ 伪目标指的是一个标签，执行make时并不生成"clean"文件，由于伪目标不是文件，所以make无法生成它的依赖关系和决定它是否要执行
+ 如果伪目标与文件名重名，则不会生效
+ 为避免重名的情况，使用 "**.PHONY**" 来显式地指明一个目标是伪目标，不管是否有这个文件，这个目标都是伪目标

可以试验一下：如果当前目录下有clean文件

makefile中写有:
```
clean:
    rm hello.o
```

执行make clean命令会提示**make: “clean”已是最新。**

+ makefile中的第一个目标会被作为其默认目标。伪目标一般没有依赖文件，但是我们可以为它指定所依赖的文件，即便它们没有依赖关系。

一个应用:

如果我们想要makefile生成多个可执行文件,可以定义若干个目标，并将这若干个目标作为伪目标的依赖文件，当执行伪目标时，这些依赖所在的命令就会被执行,并生成相应的可执行文件

例子：

有文件p1.cpp和p2.cpp

makefile内容
```
all: p1
.PHONY: all
p1: p1.o
    g++ -o p1 p1.o
p2: p2.o
    g++ -o p2 p2.o
```
执行make后，生成p1和p2两个可执行文件

## 自动变量

```
$< : 规则中的第一个依赖
$@：规则中的目标
$^： 规则中所有的依赖
```

## 分号

如果要让上一条命令的结果应用于下一条，则使用**分号**隔开这两条命令

这里cd命令表示进入某个目录，pwd显示当前所在的目录
   
```
app:
    cd /home/admin/
    pwd
```

显示的是makefile的路径

```
app:
    cd /home/admin/;pwd
```

显示的是/home/admin路径

## 减号

如果在makefile命令行前添加一个"-", 那么当该命令执行出错时，不会终止命令的执行

有如下makefile：
```
app: main
	pwd
main:
	- cd /home/
```

执行时先寻找目标app的main依赖，找不到，则向下寻找，main是伪目标，没有依赖，则执行cd命令，然后再执行app的pwd命令

如果此时makefile变为：
```
app: main
	pwd
main:
	- cd /hom/
```

则执行cd命令后出错，然后继续执行pwd命令,没有减号，则停止make

## 变量

1. 可以是字符，数字，下划线(可以数字开头)
2. 变量声明时需要赋初值，使用变量时用${w},\$符号用\$\$表示

```
obj = hello.o
app: ${obj}
	g++ hello.o -o hello
${obj}: hello.cpp
```

## 赋值变量

```
y:=${x}cde
x=bcd
all:
	echo ${y}
```
使用":="赋值,只能使用前面已经定义好的变量,此时输出cde

```
y=${x}cde
x=bcd
all:
	echo ${y}
```

使用"="赋值,此时输出bcdcde

```
y=abc
y?=bcd
all:
	echo ${y}
```
使用"?="赋值，如果变量已经被定义过，则什么也不做，
此时输出abc

```
x = abc
y += cde
all:
    echo${y}
```
使用"+="追加值，此时输出abccde

## 常用函数的调用
${<function> <arguments>}

函数名和参数之间以空格分开，函数调用以$开头，为了统一，变量使用括号
```
$(substr a, b, $(x))
```

1. 字符串替换函数

```
$(substr <from>, <to>, <text>)
把字符串<text>中的<from>替换成<to>
```

2. 模式字符串替换函数

```
$(patsubst <pattern>, <replacement>, <text>)
查找<text>中的单词(单词以空格,Tab,回车或换行分隔)是否符合模式<pattern>, 匹配，则以<replacement>替换，"%"匹配任意长度的字串(<pattern>和<replacement>中的%都是),使用"\"来转义
```

```
all:
	echo $(patsubst %c, \%d, %cc)
```

输出%d

3. 字符串处理函数

```
$(findstring <find>, <in>)
在<in>中查找<find>, 找到则返回<find>字符串,否则返回空字符串
```

4. 过滤函数

```
$(filter <pattern...>, <text>)
以<pattern>模式匹配<text>, 可以有多个<pattern>,保留符合<pattern>模式的单词
```

5. 反过滤函数

```
$(filter-out <pattern...>, <text>)
以<pattern>模式匹配<text>, 可以有多个<pattern>,去除符合<pattern>模式的单词
```

6. 排序函数

```
$(sort <list>)
将字符串<list>中的单词升序排序， 遇到相同的单词，会删除
```

7. 取单词函数

```
$(word <n> <text>)
从<text>中去出第<n>个单词, 如果<n>的值大于<text>的单词数，返回空
```

8. 取单词串函数

```
$(wordlist <s>, <e>, <list>)
从<list>中去除从<s>到<e>的单词串，<s>和<e>为数字
```

9. 单词个数统计函数

```
$(words <text>)
统计<text>中的单词个数
```

10. 首单词函数

```
$(firstword <text>)
返回<text>中的第一个单词
```

11. 取目录函数

```
$(dir <names...>)
从names中取出目录部分
```

12. 取文件名函数

```
$(notdir <names...>)
返回<names>非目录部分
```

13. 取后缀名函数

```
$(suffix <names...>)
从文件名序列<names>中取出各个文件名的后缀名,没有后缀名则返回空
```

14. 取前缀名函数

```
$(basename <names...>)
从文件名序列<names>中取出各个文件名的前缀名,没有前缀名则返回空
```

15. 加后缀函数
    
```
$(addsuffix <suffix>, <names...>)
把后缀<suffix>加到每个<names>单词后面
```

16. 加前缀函数
    
```
$(addprefix <suffix>, <names...>)
把前缀<suffix>加到每个<names>单词前面
```

17. 循环函数
```
$(foeach <var>, <list>, <text>)
把<list>中的单词逐一取出，并放到<var>中，然后再执行<text>表达式
```
