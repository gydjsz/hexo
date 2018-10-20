---
title: 终端下emacs鼠标支持
---
[参考链接]http://cn-popeye.iteye.com/blog/1164033
[参考链接]http://blog.chinaunix.net/uid-20609878-id-1915823.html
[参考链接]http://emacser.com/mouse.htm
ECB模式开启四个窗口，可以浏览目录，源码文件，方法和历史记录，但是默认只支持键盘操作，RET才能打开。

要支持鼠标需如下操作：

1.在 Emacs 中执行“ M-x ecb-customize-most-important ”，找到“ Ecb Primary Secondary Mouse Buttons ”选项

2.将其设为“ Primary: mouse-1, secondary: mouse -2 ” ，

3.点击state，设置成“ Save for Future Sessions ”保存。


在.emacs配置文件中加入(xterm-mouse-mode t), 支持终端鼠标


