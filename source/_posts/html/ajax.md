---
title: AJAX学习
tags:
- AJAX
---

# 网络请求

1. 创建AJAX对象

<!--more-->

```javascript
var xhr = new XMLHttpRequest();
```

2. 告诉AJAX请求地址以及请求方式

```javascript
xhr.open('get', 'http://localhost:8081/test');
```

3. 发送请求

```javascript
xhr.send();
```

4. 获取服务器端响应数据

```javascript
xhr.onload = function(){
    console.log(xhr.responseText);
}
```

5. 完整的代码

```javascript
//获取ajax对象
var xhr = new XMLHttpRequest();
//请求方式及请求地址
xhr.open('get', 'http://localhost:8081/test');
//发送请求
xhr.send();
//获取响应数据
xhr.onload = function() {
    console.log(JSON.parse(xhr.responseText));
}
```

# 带参数的get请求

```javascript
var xhr = new XMLHttpRequest();
var params = 'userName=zhangsan&age=18';
xhr.open('get', 'http://localhost:8081/test?' + params);
xhr.send();
xhr.onload = function(){
    console.log(xhr.repsonseText);
}
```

# 带参数的post请求

**使用post请求需要通过
setRequestHeader()添加一个Http头部
然后在send()方法中指定发送的数据**

```javascript
//数据格式为userName=zhangsan&age=18的请求参数类型
setRequestHeader('Content-Type', 'application/x-www-form-urlencoded')
```

```javascript
var xhr = new XMLHttpRequest();
var params = 'userName=zhangsan&age=18';
xhr.open('post', 'http://localhost:8081/test');
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
xhr.send(params);
xhr.onload = function(){
    console.log(xhr.repsonseText);
}
```

Content-Type: 用于定义网络文件的类型和网页的编码，决定浏览器将以什么形式、什么编码读取这个文件.

常见的媒体格式类型:

+ text/html: HTML格式
+ text/plain: 纯文本格式      
+ text/xml: XML格式
+ image/gif: gif图片格式    
+ image/jpeg: jpg图片格式 
+ image/png: png图片格式
+ application/json: JSON数据格式
+ application/pdf: pdf格式  
+ application/msword: Word文档格式
+ application/octet-stream: 二进制流数据（如常见的文件下载）
+ application/x-www-form-urlencoded
+ multipart/form-data: 需要在表单中进行文件上传时，就需要使用该格式

# 将JSON数据转化为JSON字符串

在传递请求参数的时候, 参数必须以字符串的形式及进行传递

```javascript
JSON.stringify()
```

将json数据转化为字符串后再传到服务器
```javascript
var xhr = new XMLHttpRequest();
xhr.open('post', 'http://localhost:8081/test');
var params = {name: 'zhangsan', age: '18'}
xhr.setRequestHeader('Content-Type', 'application/json');
xhr.send(JSON.stringify(params));
xhr.onload = function(){
    console.log(xhr.responseText);
}
```

# 将服务器端的JSON字符串转化为JSON对象

```javascript
var responseText = JSON.parse(xhr.responseText);
```

# AJAX网络状态

xhr.status 返回状态码

网络中断时可以设定触发事件
```javascript
xhr.onerror = function(){
    console.log('网络中断')
}
```

# 使用jQuery框架的AJAX

```javascript
$.ajax({
    //请求方式
    type: 'get',
    //请求地址
    url: 'http://localhost:8081/test',
    contentType: 'application/json', 
    //请求数据
    data: JSON.stringify({
        name: 'zhangsan',
        age: '18'
    }),
    //在发送之前调用
    beforeSend: function(){
        //请求不会被发送
        return false;
    },
    //请求成功后调用
    success: function(response){},
    //请求失败后调用
    error: function(xhr){}
})
```

```javascript
$.get('http://localhost:8081/test', {name: 'zhangsan'}, function(response){})
$.post('http://localhost:8081/test', {name: 'zhangsan'}, function(response){})
```
