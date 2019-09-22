---
title: 加快github和firefox扩展页面访问速度
---
访问https://www.ipaddress.com，拉下来，找到页面中下方的“IP Address Tools – Quick Links”
<!--more-->
分别输入github.global.ssl.fastly.net和github.com，查询ip地址
修改本地hosts文件

在/etc/hosts中添加以下内容：

151.101.185.194 https://github.global.ssl.fastly.net
192.30.253.112 https://github.com

117.18.232.191 mozorg.cdn.mozilla.net
117.18.232.191 addons.cdn.mozilla.net


