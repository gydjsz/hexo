---
title: vue路由
tags:
- vue
---

**需要引入 vue-router.js**

<!--more-->

# vue-router 的基本使用

1. 编写组件模板, 并将模板对象赋值给一个变量

```javascript
var login = {
  template: '<h1>登录组件</h1>'
}
```

2. 实例化一个 VueRouter 对象, 其中有 routes 属性, 是一个数组, 表示路由匹配规则

- path: 表示监听哪个路由地址
- component: 组件的模板对象

```javascript
var routerObj = new VueRouter({
  //路由匹配规则
  routes: [
    //path: 表示监听哪个路由地址, component: 组件的模板对象
    { path: '/login', component: login },
    { path: '/register', component: register }
  ]
})
```

3. 将路由规则对象注册到 vue 实例上, 用来监听 url 的变化, 然后展示对应的组件

```javascript
var app = new Vue({
  el: '#app',
  //将路由规则对象注册到vue实例上, 用来监听url的变化, 然后展示对应的组件
  router: routerObj
})
```

4. 使用 router-view 占位符, 将路由规则匹配到的组件展示出来

```html
<router-view></router-view>
```

5. 如果使用 a 标签进行路由的跳转, 则需要在链接上加"#"(这是 url 的 hash 值)

```html
<a href="#/login">登录</a>
```

6. 可以使用 router-link 标签, to 属性中直接加上链接, 该标签默认渲染为一个 a 标签

```html
<router-link to="/login">登录</router-link>
```

- router-link 标签中有 tag 标签, 可以指定该路由渲染的标签(点击该标签也可以触发跳转)

```html
<router-link to="/login" tag="span">登录</router-link>
```

7. 设置redirect可以将访问的根路径进行重定向

```javascript
{ path: '/', redirect: '/login' },
```

```html
<div id="app">
  <!-- <a href="#/login">登录</a> -->
  <!-- <a href="#/register">注册</a> -->
  <router-link to="/login">登录</router-link>
  <router-link to="/register">注册</router-link>
  <!-- 用做占位符, 路由规则匹配到的组件会展示到里面去 -->
  <router-view></router-view>
</div>
<script>
  var login = {
    template: '<h1>登录组件</h1>'
  }
  var register = {
    template: '<h1>注册组件</h1>'
  }
  var routerObj = new VueRouter({
    //路由匹配规则
    routes: [
      //path: 表示监听哪个路由地址, component: 组件的模板对象
      { path: '/', redirect: '/login' },
      { path: '/login', component: login },
      { path: '/register', component: register }
    ]
  })
  var app = new Vue({
    el: '#app',
    //将路由规则对象注册到vue实例上, 用来监听url的变化, 然后展示对应的组件
    router: routerObj
  })
</script>
```

# 设置选中路由高亮

链接激活时默认class样式名: router-link-active

可以设置**linkActiveClass**值来更改其默认值

```javascript
var routerObj = new VueRouter({
    routes: [],
    linkActiveClass: 'myActive'
})
```

# 路由传递参数

1. 使用query传递参数

链接中携带参数
```html
<router-link to="/login?id=10&name=zhangsan">登录</router-link>
```

通过"this.$query.参数名"来获取

```html
<div id="app">
  <router-link to="/login?id=10&name=zhangsan">登录</router-link>
  <router-link to="/register">注册</router-link>
  <router-view></router-view>
</div>
<script>
  var login = {
    template: '<h1>登录--{{$route.query.id}}--{{$route.query.name}}</h1>',
    //组件的生命周期钩子
    created() {
      console.log(this.$route.query)
    }
  }
  var register = {
    template: '<h1>注册</h1>'
  }
  var router = new VueRouter({
    routes: [
      { path: '/login:id', component: login },
      { path: '/register', component: register }
    ]
  })
  var app = new Vue({
    el: '#app',
    // router: router,
    //同名默认解析
    router
  })
</script>
```

2. params传参

在path中填入":参数名"

```javascript
{ path: '/login/:id/:name', component: login }
```

在链接中需要含有相应的数据, 否则页面显示不了
```html
<router-link to="/login/123/zhangsan">登录</router-link>
```

通过"this.$route.params"来获取数据

```html
<div id="app">
  <router-link to="/login/123/zhangsan">登录</router-link>
  <router-link to="/register">注册</router-link>
  <router-view></router-view>
</div>
<script>
  var login = {
    template: '<h1>登录</h1>',
    //组件的生命周期钩子
    created() {
      console.log(this.$route.params)
    }
  }
  var register = {
    template: '<h1>注册</h1>'
  }
  var router = new VueRouter({
    routes: [
      { path: '/login/:id/:name', component: login },
      { path: '/register', component: register }
    ]
  })
  var app = new Vue({
    el: '#app',
    // router: router,
    //同名默认解析
    router
  })
</script>
```

# 路由嵌套

+ 通过children属性来添加子路由
+ 子路由的path不用带"/"

```html
<div id="app">
  <router-link to="/account">Account</router-link>
  <router-view></router-view>
</div>
<template id="tmp">
  <div>
    <h1>hello</h1>
    <router-link to="/account/login">登录</router-link>
    <router-link to="/account/register">注册</router-link>
    <router-view></router-view>
  </div>
</template>

<script>
  var account = {
    template: '#tmp'
  }
  var login = {
    template: '<h1>登录</h1>'
  }
  var register = {
    template: '<h1>注册</h1>'
  }
  var router = new VueRouter({
    routes: [
      {
        path: '/account',
        component: account,
        children: [
          { path: 'login', component: login },
          { path: 'register', component: register }
        ]
      }
    ]
  })
  var app = new Vue({
    el: '#app',
    router
  })
</script>
```

# 使用命名视图实现经典布局

1. components中指定路由的名称

```javascript
routes: [
  {
    path: '/',
    components: {
      default: header,
      left: leftBox,
      main: mainBox
    }
  }
]
```

2. 在router-view中的name属性指定相应的路由名

```html
<router-view name="left"></router-view>
```


```html
<style>
    .header {
      background-color: aquamarine;
    }
    * {
      margin: 0;
      padding: 0;
    }
    .left {
      background-color: antiquewhite;
      height: 700px;
      flex: 2;
    }
    .main {
      background-color: cadetblue;
      height: 700px;
      flex: 8;
    }
    .container {
      display: flex;
    }
</style>
<body>
  <div id="app">
    <router-view></router-view>
    <div class="container">
      <router-view name="left"></router-view>
      <router-view name="main"></router-view>
    </div>
  </div>
  <script>
    var header = {
      template: '<h1 class="header">头部区域</h1>'
    }
    var leftBox = {
      template: '<h1 class="left">Left区域</h1>'
    }
    var mainBox = {
      template: '<h1 class="main">main区域</h1>'
    }
    var router = new VueRouter({
      routes: [
        {
          path: '/',
          components: {
            default: header,
            left: leftBox,
            main: mainBox
          }
        }
      ]
    })
    var app = new Vue({
      el: '#app',
      router
    })
  </script>
</body>
```

