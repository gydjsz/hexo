---
title: vscode运行c++
tags:
- vscode
---

1. 下载Code Runner插件

2. 配置setting.json

```json
"code-runner.executorMap": {
    "cpp": "cd $dir && g++ -std=c++11 $fileName -o /a.exe && /./a.exe"
},
"code-runner.runInTerminal": true
```
