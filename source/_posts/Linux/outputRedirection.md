---
title: Linux将文件内容输出重定向
---
1. 将一个文件内容重定向至另一个文件中
```
cat hello.cpp > world.cpp
```

2. 将一个文件中的内容重定向至剪贴板中
```
sudo apt install xsel

cat hello.cpp | xsel -b -i  //将hello.cpp内容写入剪贴板中
```
