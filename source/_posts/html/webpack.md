---
title: webpack的使用
tags:
- webpack
---

# 介绍 

+ webpack能处理js之间的依赖关系
+ 将较为高级的、浏览器无法识别的语法转化为低级的、浏览器能识别的语法

# 基本使用

1. 新建文件夹: web
2. web文件夹: 文件夹: dist、src; 文件: index.html、main.js
3. src文件夹: css、js、images

index.html
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=\, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <script src="/bundle.js"></script>
  </head>
  <body>
    <ul>
      <li>这是第1个li</li>
      <li>这是第2个li</li>
      <li>这是第3个li</li>
      <li>这是第4个li</li>
      <li>这是第5个li</li>
      <li>这是第6个li</li>
      <li>这是第7个li</li>
      <li>这是第8个li</li>
      <li>这是第9个li</li>
      <li>这是第10个li</li>
    </ul>
  </body>
</html>
```

main.js
```js
import $ from 'jquery'

$(function(){
    $('li:odd').css('backgroundColor', 'yellow')
    $('li:even').css('backgroundColor', function () {
        return 'orange'
    })
})
```

+ 安装: cnpm i webpack webpack-cli -g
+ 打包: webpack src\main.js -o dist\bundle.js --mode=development 

# 配置文件

在web目录下新建webpack.config.js

```js
const path = require('path')

module.exports = {
    //入口, 表示要使用webpack打包哪个文件
    entry: path.join(__dirname, './src/main.js'),
    //输出文件相关配置
    output: {
        //指定打包好的文件, 输出到哪个目录中去
        path: path.join(__dirname, './dist'),
        //指定输出的文件的名称
        filename: 'bundle.js'
    },
    //设置模式
    mode: 'development'
}
```

执行webpack, 即可打包

# 自动打包编译

webpack-dev-server
1. 使用该包需要将webpack、webpack-cli安装到本地: cnpm i webpack -D, cnpm i webpack-cli -D
2. 运行cnpm i webpack-dev-server -D 把这个工具安装到项目的本地开发依赖
3. 在package.json文件的scripts中添加一条dev命令

```js
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack-dev-server"
}
```

4. 运行cnpm run dev

注意:
+ webpack-dev-server打包生成的bundle.js保存在内存中, 不存在物理磁盘中, 可以认为该文件在根目录下: /bundle.js

# webpack-dev-server配置

1. 指令形式

```js
"dev": "webpack-dev-server --open --port 8082 --contentBase src --hot"
```

2. 配置形式

```js
//配置dev-server
devServer: {
    //自动打开浏览器
    open: true,
    //设置启动时运行的端口
    port: 8082,
    //指定托管的根目录
    contentBase: 'src',
    //启用热更新
    hot: true,
}
```

# html-webpack-plugin的使用

作用:
1. 自动在内存中根据指定页面生成一个html页面
2. 自动把打包好的bundle.js追加到页面中

+ 安装cnpm i html-webpack-plugin -D

```js
const htmlWebpackPlugin = require('html-webpack-plugin')
//配置插件的节点
plugins: [
    // 创建一个在内存中生成html页面的插件
    new htmlWebpackPlugin({
        // 指定模板页面, 将来会根据指定的页面路径, 生成内存中的页面
        template: path.join(__dirname, "./src/index.html"),
        // 指定生成的页面的名称
        filename: 'index.html'
    })
]
```

此时html文件中自动添加bundle.js路径, 所以可以不用导入

```html
<!-- <script src="/bundle.js"></script> -->
```

# loader 配置处理css样式

## 处理css文件

安装插件: cnpm i style-loader css-loader -D

在webpack.config.js中新增一个配置节点module, 这是一个对象, 这个对象有个rules属性, 该属性是数组, 存放了所有第三方文件的匹配和处理规则

在css文件夹中添加index.css

```js
//用于配置所有第三方模块加载器
module: {
    //所有第三方模块的匹配规则
    rules: [
        //配置.css文件的第三方loader规则
        {test: /\.css$/, use: ['style-loader', 'css-loader']}
    ]
}
```

webpack处理的是js文件

处理过程:
+ 发现处理的文件不是js文件, 就会到配置文件中查找有没有第三方loader规则
+ 能找到对应的规则, 就会调用对应的loader处理这种文件类型
+ 当调用loader的时候, rules中的处理规则是从后往前调用的
+ 当最后一个loader调用完毕, 会把处理结果直接交给webpack进行打包合并, 最终输出到bundle.js中

## 处理less文件

安装插件处理less文件: cnpm i less-loader less -D

在css文件夹中添加index.less

```css
ul{
    margin: 0;
    padding: 0;
}
```

main.js中导入

```js
import './css/index.less'
```

在rules中追加
```js
{ test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader'] }
```

+ 处理scss文件 

安装插件处理scss文件: cnpm i sass-loader node-sass -D

在css文件夹下添加文件index.scss

index.scss

```scss
html, body{
    margin: 0;
    padding: 0;
    li{
        font-size: 12px;
        line-height: 30px;
    }
}
```

main.js中导入
```js
import './css/index.scss'
```

webpack.config.js
rules规则

```js
{ test: /\.scss$/, use: ['style-loader', 'css-loader', 'sass-loader'] }
```

## 处理图片url

安装插件处理图片路径: cnpm i url-loader file-loader -D

添加rules规则

```js
{test: /\.(jpg|png|gif|bmp|jpeg)$/, use: 'url-loader' }
```

图片会被转化为base64格式的字符串

可以设置limit值, 当引用的图片大于给定的limit值, 则不会转化为base64格式的字符串, 保证了小图片使用base64, 大图片使用图片路径

```js
use: 'url-loader?limit=3000'
```

图片名会被转化为生成的hash值
如果要显示图片名可以设置name属性

```js
use: 'url-loader?limit=3000&name=[hash:8]-[name].[ext]'
```

+ hash: 表示图片的hash值, hash:8表示32位的hash数只显示前8位
+ name: 表示图片的名称
+ ext: 表示图片的后缀

## 处理字体font

rules规则

```js
{test: /\.(ttf|eot|svg|woff|woff2)$/, use: 'url-loader'}
```

## 将高级语法转化为低级语法babel

安装插件: 
+ cnpm i babel-core babel-loader babel-plugin-transform-runtime -D
+ cnpm i babel-preset-env babel-preset-stage-0 -D

rules规则

```js
{ test: /\.js$/, use: 'babel-loader', exclude: /node_modules/ }
```

需要将node_modules目录通过exclude排除

在web目录下新建.bashrc

在.bashrc写配置项:

```js
{
    "presets": ["env", "stage-0"],
    "plugins": ["transform-runtime"]
}
```

# vue使用webpack

+ 安装vue: cnpm i vue -S

index.html
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <!-- <script src="../dist/bundle.js"></script> -->
  </head>
  <body>
    <div id="app">
      {{msg}}
    </div>
  </body>
</html>
```

main.js
```js
import Vue from 'vue'

var app = new Vue({
  el: '#app',
  data: {
    msg: 'hello'
  }
})
```

main.js中导入的vue包, 是vue.runtime.common.js
不是最全的包, 所以需要修改webpack.config.js, 设置vue被导入包的路径

webpack.config.js

需要设置resolve属性使得导入的vue包为vue.js

```js
resolve: {
    alias: {
        "vue$": "vue/dist/vue.js"
    }
}
```

## 使用render函数渲染指定的组件到容器中

1. 安装插件 cnpm i vue-loader vue-template-compiler -D

添加rules规则

```js
{test: /\.vue$/, use: 'vue-loader'}
```

2. 配置plugin

webpack.config.js
```js
const VueLoaderPlugin = require('vue-loader/lib/plugin')

plugins: [
    new VueLoaderPlugin()
]
```

3. main.js和login.vue

main.js
```js
import Vue from 'vue'
import login from './login.vue'

var app = new Vue({
  el: '#app',
  data: {
    msg: 'hello, world'
    },
    render: function (creatElements) {
      return creatElements(login)
  }
})
```

login.vue
```html
<template>
  <div>
    <h1>登录组件</h1>
  </div>
</template>
```

render属性中的creatElements属性是一个方法, 能将组件模板转化为html
其结果直接替换el容器

即
```html
<div id="app">
</div>
```

变为
```html
<div>
    <h1>登录</h1>
</div>
```

使用箭头函数
```js
render: c => c(login)
```

## export default和export的使用

1. 使用export default来向外暴露成员

使用export default暴露的成员可以使用任意的变量名接收
export default只允许暴露一次

login.vue
```html
<template>
  <div>
    <h1>登录组件{{ msg }}</h1>
  </div>
</template>

<script>
export default {
  data() {
    return {
      msg: 'hello, world'
    }
  },
  methods: {
    show() {
      console.log('login')
    }
  }
}
</script>
```

main.js

```js
import m from './login.vue'
console.log(m)
```

2. 使用export

使用export向外暴露的成员, 只能使用{}的形式来接收

可以使用多次

test.js

```js
export var title = "person"
```

main.js

```js
import { title } from "./test.js"

console.log(title)
```

如果要更改导出的成员名, 可以使用as

```js
import { title as t } from "./test.js"
```

## 使用vue-router

1. 安装: cnpm vue-router -S
2. 导入vue-router

```js
import VueRouter from 'vue-router'
Vue.use(VueRouter)
```

3. 实例化路由

```js
var router = new VueRouter({
  routes: [
    { path: '/account', component: account },
    {
      path: '/goodsList',
      component: goodsList
    }
  ]
})
```

4. 添加路由

```js
var app = new Vue({
  el: '#app',
  data: {
    msg: 'hello, world'
  },
  render: c => c(main),
  router
})
```

main.js

```js
import Vue from 'vue'
import main from './App.vue'
import VueRouter from 'vue-router'
import account from './main/Account.vue'
import goodsList from './main/GoodsList.vue'

Vue.use(VueRouter)
var router = new VueRouter({
  routes: [
    { path: '/account', component: account },
    {
      path: '/goodsList',
      component: goodsList
    }
  ]
})

var app = new Vue({
  el: '#app',
  data: {
    msg: 'hello, world'
  },
  render: c => c(main),
  router
})
```

Account.vue
```html
<template>
  <div>
    <h1>Account</h1>
  </div>
</template>
```

GoodsList.vue
```html
<template>
  <div>
    <h1>GoodsList</h1>
  </div>
</template>
```

index.html
```html
<body>
  <div id="app"></div>
</body>
```

5. 子路由

Login.vue
```html
<template>
    <div>
        <h1>登录组件</h1>
    </div>
</template>
```

Register.vue
```html
<template>
    <div>
        <h1>注册组件</h1>
    </div>
</template>
```

Account.vue
```html
<template>
  <div>
    <h1>Account</h1>
    <router-link to="/account/login">登录</router-link> 
    <router-link to="/account/register">注册</router-link> 
    <router-view></router-view>
  </div>
</template>
```

main.js中添加children

```js
var router = new VueRouter({
  routes: [
    {
      path: '/account',
      component: account,
      children: [
        {
          path: 'login',
          component: login
        },
        {
          path: 'register',
          component: register
        }
      ]
    },
    {
      path: '/goodsList',
      component: goodsList
    }
  ]
})
```

## 组件中style标签lang属性和scoped属性

普通的style标签只支持普通的样式, 如果要启用scss或less, 需要为style元素设置lang属性

```html
<style lang="scss">
</style>
```

在style内部写的css是全局的, 可以设置scoped让其变为局部

```html
<style scoped>
</style>
```

## 抽离路由

router.js

```js
import account from './main/Account.vue'
import goodsList from './main/GoodsList.vue'
import login from './childCom/Login.vue'
import register from './childCom/Register.vue'
import VueRouter from 'vue-router'

var router = new VueRouter({
  routes: [
    {
      path: '/account',
      component: account,
      children: [
        {
          path: 'login',
          component: login
        },
        {
          path: 'register',
          component: register
        }
      ]
    },
    {
      path: '/goodsList',
      component: goodsList
    }
  ]
})

export default router
```

main.js

```js
import Vue from 'vue'
import main from './App.vue'
import router from './router.js'
import VueRouter from 'vue-router'

Vue.use(VueRouter)

var app = new Vue({
  el: '#app',
  data: {
    msg: 'hello, world'
  },
  render: c => c(main),
  router
})
```

附:

webpack.config.js

```js
const path = require('path')
const htmlWebpackPlugin = require('html-webpack-plugin')
const VueLoaderPlugin = require('vue-loader/lib/plugin')

module.exports = {
  //入口, 表示要使用webpack打包哪个文件
  entry: path.join(__dirname, './src/main.js'),
  //输出文件相关配置
  output: {
    //指定打包好的文件, 输出到哪个目录中去
    path: path.join(__dirname, './dist'),
    //指定输出的文件的名称
    filename: 'bundle.js'
  },
  //设置模式
  mode: 'development',
  //配置dev-server
  devServer: {
    //自动打开浏览器
    open: true,
    //设置启动时运行的端口
    port: 8082,
    //指定托管的根目录
    contentBase: 'src',
    //启用热更新
    hot: true
  },
  //配置插件的节点
  plugins: [
    // 创建一个在内存中生成html页面的插件
    new htmlWebpackPlugin({
      // 指定模板页面, 将来会根据指定的页面路径, 生成内存中的页面
      template: path.join(__dirname, './src/index.html'),
      // 指定生成的页面的名称
      filename: 'index.html'
    }),
      new VueLoaderPlugin()
  ],
  resolve: {
    alias: {
        // "vue$": "vue/dist/vue.js"
    }
  },
  module: {
    rules: [
      //配置.css文件的第三方loader规则
      { test: /\.css$/, use: ['style-loader', 'css-loader'] },
      //配置.less文件的第三方loader规则
      { test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader'] },
      //配置.scss文件的第三方loader规则
      { test: /\.scss$/, use: ['style-loader', 'css-loader', 'sass-loader'] },
      //配置图片路径loader规则
      {
        test: /\.(jpg|png|gif|bmp|jpeg)$/,
        use: 'url-loader?limit=232&name=[hash:8]-[name].[ext]'
      },
      //配置字体文件loader规则
      { test: /\.(ttf|eot|svg|woff|woff2)$/, use: 'url-loader' },
      //配置ES6高级语法loader规则
      { test: /\.js$/, use: 'babel-loader', exclude: /node_modules/ },
      //配置vue的loader规则
      { test: /\.vue$/, use: 'vue-loader' }
    ]
  }
}
```
