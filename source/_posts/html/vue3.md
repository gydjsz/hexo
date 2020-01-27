---
title: vue学习(三)
tags:
- vue
---

# 使用watch监听data数据的变化

watch属性可以监视data数据的变化, 然后出发watch中对应的处理函数

<!--more-->

```html
<div id="app">
  <input type="text" v-model="msg" />
</div>
<script>
  var app = new Vue({
    el: '#app',
    data: {
      msg: ''
    },
    watch: {
      msg() {
        console.log(this.msg)
      }
    }
  })
</script>
```

处理函数可以添加两个参数: newVal和oldVal
分别获得更新后和更新前的值

```javascript
watch: {
  msg(newVal) {
    console.log(newVal)
  }
}
```

```javascript
watch: {
  msg(newVal, oldVal) {
    console.log(newVal + '---' + oldVal)
  }
}
```

# 监听路由地址的改变

```html
<div id="app">
  <router-link to="/login">登录</router-link>
  <router-link to="/register">注册</router-link>
  <router-view></router-view>
</div>
<script>
  var login = {
    template: '<h1>登录</h1>'
  }
  var register = {
    template: '<h1>注册</h1>'
  }
  var router = new VueRouter({
    routes: [
      {
        path: '/login',
        component: login
      },
      {
        path: '/register',
        component: register
      }
    ]
  })
  var app = new Vue({
    el: '#app',
    router,
    watch: {
      '$route.path': function(newVal, oldVal) {
        console.log(newVal + '---' + oldVal)
      }
    }
  })
</script>
```

# computed计算属性

在computed中可以定义一些属性, 这些属性叫做计算属性
计算属性的本质是一个方法, 在使用这些计算属性时, 将它们的名称直接当做属性来使用

fullName的计算方法是firstName + lastName
这里直接将fullName的计算方法写成一个函数
然后直接将input内的值绑定为该函数, 一旦firstName或lastName更新, fullName立即发生更新

```html
<div id="app">
  <input type="text" v-model="firstName" /> -
  <input type="text" v-model="lastName" />=
  <input type="text" v-model="fullName" />
</div>

<script>
  var app = new Vue({
    el: '#app',
    data: {
      firstName: '',
      lastName: ''
    },
    computed: {
      fullName: function() {
        return this.firstName + this.lastName
      }
    }
  })
</script>
```

注意: 
+ 计算属性在引用时, 不能加(), 直接当做普通的属性使用
+ 只要function内部所用的任何data数据发生变化, 就会立即重新计算这个计算属性的值
+ 计算属性的求值结果会被缓存起来, 如果计算属性方法内部的数据没有发生变化, 则不会重新对计算属性求值

# 使用render方法来渲染组件

```html
<div id="app">
  <login></login>
</div>
<script>
  var login = {
    template: '<h1>登录</h1>'
  }
  var app = new Vue({
    el: '#app',
    render: function(createElements) {
      return createElement(login)
    }
  })
</script>
```

createElements是一个方法, 能将指定的组件模板渲染为html结构
return的结果直接替换el指定的容器

# UI框架

# MINT-UI

## 安装

cnpm i mint-ui -S

## 导入全部组件

main.js

```js
import MintUI from 'mint-ui'
import 'mint-ui/lib/style.css'

Vue.use(MintUI)
```

## 按需引入

```js
import { Button } from 'mint-ui'

Vue.component(Button.name, Button)
```

Button.name为mt-button, 可以修改

# MUI

## 安装 git clone https://github.com/dcloudio/mui.git

将其中的dist放入lib包中, 并重命名为mui

## 导入

```js
import './lib/mui/css/mui.min.css
```

# axios全局配置

```js
//超时时间
axios.defaults.timeout = 3000;
//默认地址
axios.defaults.baseURL = '';
//设置请求头
axios.defaults.headers['myToken'] = ''
```

# axios拦截器

## 请求拦截器

可以在请求发出之前设置一些信息

```js
axios.interceptors.request.use(function(config){
    console.log(config)
    config.headers.mytoken = 'hello'
    return config
}, function(err){
    console.log(err)
})
```

## 响应拦截器

在获取数据之前对数据做一些加工处理

```js
axios.interceptors.response.use(function(res){
    var data = res.data
    //get请求后返回的参数为data
    return data
}, function(err){
    console.log(err)
})
```

# 跨域解决

```js
devServer{
	proxy: {
	// 访问路径
      '/download': {
		  //代理链接
        target: 'https://gydjsz.top/',
        changeOrigin: true
      }
}
```

# 事件总线

通过一个空的vue实例作为桥梁实现vue组件之间的通信

bus.js
```js
import Vue from 'vue'
var bus = new Vue()
export default bus
```

在Home.vue中引入
```js
import bus from './bus.js'
```

通过$emit发送数据, name为事件名称, data为数据
```js
bus.$emit('name', data)
```

在Person.vue中通过$on来监听并接收数据
```js
import bus from './bus.js'
```

通常挂载到生命周期created或mouted中

```js
bus.$on('name', data => {
    console.log(data)
})
```

最后需要在生命周期beforeDestroy或者destroyed中清除bus
否则反复切换组件会触发多次响应

```js
beforeDestroy() {
    bus.$off('Home')
}
```

# 手势事件

github: https://github.com/vuejs/vue-touch/tree/next

1. 安装: cnpm i vue-touch@next -S

2. 导入

```js
import VueTouch from 'vue-touch'
Vue.use(VueTouch)
```

3. 使用

```html
<v-touch v-on:swipeleft="onSwipeLeft"></v-touch>
```

|Recognizer|Events|Example|
|:-:|:-:|:-:|
|Pan|pan, panstart, panmove, panend, pancancel, panleft, panright, panup, pandown|v-on:panstart="callback"|
|Pinch|pinch, pinchstart, pinchmove,pinchend, pinchcancel, pinchin, pinchout|v-on:pinchout="callback"|
|Press|press, pressup|v-on:pressup="callback"|
|Rotate|rotate, rotatestart, rotatemove, rotateend, rotatecancel|v-on:rotateend="callback"|
|Swipe|swipe, swipeleft, swiperight, swipeup, swipedown|v-on:swipeleft="callback"|
|Tap|tap|v-on:tap="callback"|

+ Pan: 手指在屏幕上连续的移动
+ Pinch: 捏合手势动作
+ Press: 长按手势
+ Rotate: 旋转手势
+ Swipe: 轻扫手势
+ Tap: 点击屏幕



