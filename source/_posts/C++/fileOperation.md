---
title: C++文件操作
tags: c++
---

1. 定义一个数据流对象指针

默认ios::in和ios::out,可以不加

```cpp
#include <fstream>

ifstream input("/home/text.txt", ios::in)
ofstream output("/home/text.txt", ios::out)
```

<!--more-->


2. 打开文件

open(filename, mode, prot)

- filename: 文件名
- mode: 打开文件的方式
- prot: 打开文件的属性


操作|说明
:-: | :-: |
ios::in|为输入(读)而打开文件
ios::out|为输出(写)而打开文件
ios::ate|初始位置：文件尾
ios::app|所有输出附加在文件末尾             
ios::trunc|如果文件已存在则先删除该文件
ios::binary|二进制方式

3. 关闭文件

close()

4. 具体实现

```cpp
//写入文件
#include <fstream>
using namespace std;

int main() {
    ofstream output("./stringsearch/test", ios::out);
    output << "hello,world" << endl;
    output.close();
    return 0;
}
```

```cpp
//读取文件
#include <fstream>
#include <iostream>
using namespace std;

int main() {
    ifstream input("./stringsearch/test", ios::in);
    string s;
    input >> s;
    cout << s << endl;
    input.close();
    return 0;
}

```

```cpp
使用getline可以读取空格
string s;
while(getline(input, s)) ;
```
