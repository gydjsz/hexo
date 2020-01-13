---
title: html学习
tags:
- html
---

# 常用标记

|标记|标记名称|功能|
|:-:|:-:|:-:|
|br|换行标记|另起一行开始|
|hr|标尺标记|形成一个水平标尺|
|blockquote|引用标记|引用名言|
|pre|预定义标记|使源代码的格式呈现在浏览器上|
|hn|标题标记|网页标题|
|b|字体加粗标记|文字加粗显示|
|i|斜体标记|文字样式斜体显示|
|sub|下标标记|文字以下标形式出现|
|sup|上标标记|文字以上标形式出现|
|u|底线标记|文字带底线形式出现|
|address|地址标记|文字以斜体形式表示地址|

<!--more-->

---

# 列表标记

|标记|描述|
|:-:|:-:|:-:|
|<ul\>|无序列表|
|<ol\>|有序列表|
|<dl\>|定义列表|
|<dt\>、<dd\>|定义列表的标记|
|<li\>|列表项目的标记|

---

# 表单
# <form\>标记的属性

|名称|描述|
|:-:|:-:|:-:|
|action|处理程序(网址或相对路径)|
|method|定义表单传送信息的方式,可取get或post|
|target|指定目标窗口或目标帧(当前窗口_self, 父级窗口_parent, 顶层窗口_top, 空白窗口_blank)|

---

# 表单控件标记

|名称|描述|
|:-:|:-:|:-:|
|type|指定表单元素的类型,text,password,checkbox,radio,submit,reset,file,hidden,button|
|name|指定表单元素的名称|
|value|指定表单元素的初始值|
|size|指定表单元素的长度(text和password)|
|maxlength|指定text和password中可输入的最大字符数|
|checked|指定radio和checkbox是否被选中|
|placeholder|输入框内提示|
|required|规定不能为空|

**form表单中设置了name属性的标签才能将值传到服务器,value属性可以指定值,checkbox和radio必须指定value**

```html
<form action="" method="GET">
    用户名<input type="text" required><br>
    密码<input type="password"><br>
    男<input type="radio" name="sex" checked>
    女<input name="sex" type="radio"><br>
    喜欢: 运动<input type="checkbox">
    看书<input type="checkbox">
    睡觉<input type="checkbox"><br>
    自我评价:<br>
    <textarea cols="30" rows="10"></textarea><br>
    <input type="submit" value="提交">
    <input type="reset" value="重置">
</form>
```

 <form action="/login" method="GET">
        用户名<input type="text" requierd><br>
        密码<input type="pyonghumingassword"><br>
        男<input type="radio" name="sex" checked>
        女<input name="sex" type="radio"><br>
        喜欢: 运动<input type="checkbox">
        看书<input type="checkbox">
        睡觉<input type="checkbox"><br>
        自我评价:<br>
        <textarea cols="30" rows="10"></textarea><br>
        <input type="submit" value="提交">
        <input type="reset" value="重置">
</form>

---

# 下拉列表

<select\></select\>创建一个下拉列表框

|名称|描述|
|:-:|:-:|:-:|
|multiple|表示列表框可以多选|
|size|设置列表宽度,size为1且没有设置multiple则为下拉列表框|

<option\></option\>设置列表框中的一个选项

|名称|描述|
|:-:|:-:|:-:|
|selected|指定默认选项,表示该选项被选中|
|value|给选项赋值|

```html
<select name="sport" size="1">
    <option value="0">请选择</option>
    <option value="1">篮球</option>
    <option value="2">足球</option>
    <option value="3">乒乓球</option>
</select>
```
<select name="sport" size="1">
    <option value="0">请选择</option>
    <option value="1">篮球</option>
    <option value="2">足球</option>
    <option value="3">乒乓球</option>
</select>

# 其他输入类型
  
|名称|描述|
|:-:|:-:|:-:|
|email|验证email输入|
|url|验证url输入|
|number|验证数值输入,max规定最大值,min规定最小值,step规定数字间隔, value规定默认值|
|range|滚动条,min设置最小值,max设置最大值,step设置数字间隔,value设置默认值|
|date|日、月、年|
|month|日、月|
|week|周、年|
|time|时间(小时和分钟)|
|datetime-local|时间、日、月、年|

```html
<form action="">
    邮箱<input type="email"><br>
    链接<input type="url"><br>
    数值<input type="number" step="3" value="1"><br>
    滚动条<input type="range" min="0" max="100" value="3"><br>
    日期<input type="datetime-local"><br>
    <input type="submit">
</form>
```

<form action="">
    邮箱<input type="email"><br>
    链接<input type="url"><br>
    数值<input type="number" step="3" value="1"><br>
    滚动条<input type="range" min="0" max="100" value="3"><br>
    日期<input type="datetime-local"><br>
    <input type="submit">
</form>

---

# datalist

datalist规定输入的选项列表
列表通过option元素创建，通过input中的list属性引用datalist的id
```html
<input type="url" list="name_list">
    <datalist id="name_list">
        <option label="搜狐" value="http://www.sohu.com"></option>
        <option label="百度" value="http://www.baidu.com"></option>
        <option label="天涯" value="http://www.tianya.cn"></option> 
</datalist>
```
<input type="url" list="name_list">
    <datalist id="name_list">
    <option label="搜狐" value="http://www.sohu.com"></option>
    <option label="百度" value="http://www.baidu.com"></option>
    <option label="天涯" value="http://www.tianya.cn"></option> 
</datalist>

---

# 表格

|标记名|说明|
|:-:|:-:|:-:|
|<table\>|表格标记|
|<tr\>|表格行标记|
|<td\>|表格单元格|
|<caption\>|表格标题,默认黑体居中|
|<th\>|表格列标题|
|border|边框宽度|
|bgcolor|表格背景色|
|align|设置对其方式|
|cellpadding|设置单元边框和内部内容之间的间隔大小|
|cellspacing|设置单元格之间的间隔大小|
|width|表格宽度|
|height|表格高度|
|colspan|列合并标记|
|rowspan|行合并标记|

```html
<table border="2" align="center" width=80%>
    <caption>
        信息
    </caption>
    <tr>
        <th>姓名</th>
        <th>年龄</th>
    </tr>
    <tr>
        <td>张三</td>
        <td>13</td>
    </tr>
    <tr>
        <td>李四</td>
        <td rowspan="2">14</td>
    </tr>
    <tr>
        <td>王五</td>
    </tr>
</table>
```

---

# 多媒体

+ <audio\></audio\>
+ <video\></video\>

|属性|说明|
|:-:|:-:|:-:|
|autoplay|自动播放|
|controls|显示播放控件|
|loop|循环播放|
|preload|预加载音频|
|src|音频的url|

# 超链接

|属性|说明|
|:-:|:-:|:-:|
|href|链接|
|download|指定下载内容的名称(需要服务端支持跨域)|
|title|鼠标停留在链接上显示提示|

