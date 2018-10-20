---
title: 关于JFrame使用setBackground()设置背景色没反应的问题
---
参考链接:&ensp;[https://zhidao.baidu.com/question/263731760.html](https://zhidao.baidu.com/question/263731760.html)
参考链接:&ensp;[https://blog.csdn.net/qq_34970891/article/details/69665986](https://blog.csdn.net/qq_34970891/article/details/69665986)

原因:JFrame当中会默认使用流式布局管理器(FlowLayout)将整个窗体进行覆盖操作，也就是说设置的颜色确实是存在的，只是被布局管理器给覆盖住了，所以无法看见。

**1. 如果直接使用Frame,则修改背景色使用setBackColor(Color.black);**

**2. 使用JFrame修改背景色**

方法一: 调用getContentPane()方法得到一个contentPane容器，然后将其设置为不可见
```java
setBackground(Color.black);
getContentPane().setVisible(true);
```

方法二: 在JFrame当中添加一个面板容器，使得面板容器对窗体形成一个覆盖后，直接设置面板的背景颜色就可以达到相当于对窗体背景颜色进行设置的效果
```java
Container con = getContentPane();
con.setBackground(Color.black); 
```

