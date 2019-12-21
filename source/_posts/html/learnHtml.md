---
title: vue学习
tags:
- vue
---
vue的基本知识
<!--more-->

# 数据获取

```html
<body>
    <div id="app">
        {{message}}
        <h2>{{school.name}} {{school.mobile}}</h2>
        <ul>
            <li>{{campus[0]}}</li>
        </ul>
    </div>

    <script src="js/vue.js"></script>
    <script>
        var app = new Vue({
            el:"#app",
            data:{
                message:"hello, world!",
                school:{
                    name: "school",
                    mobile: "2233"
                },
                campus: ["1", "2", "3"]
            }
        })
    </script>
</body>
```

# 指令

## v-text

使用v-text后，标签内的文本不会显示

```html
<body>
    <div id="app">
        <h2 v-text="message">abc</h2>
        <h2 v-text="info"></h2>
        <h2>message: {{message}}</h2>
    </div>
    <script src="js/vue.js"></script>
    <script>
        var app = new Vue({
            el: "#app",
            data: {
                message: "hello",
                info: "world"
            }
        })
    </script>
</body>
```

## v-html

v-html: 将文本以html的形式解析

```html
<body>
    <div id="app">
        <h2 v-html="content"></h2>
    </div>

    <script src="js/vue.js"></script>

    <script>
        var app = new Vue({
            el: "#app",
            data:{
                content: "<a href='https://www.bilibili.com'>程序</a>"
            }
        })
    </script>
</body>
```

## v-on

使用v-on:事件=方法来绑定事件响应方法
@方法: 可以替代v-on
在vue()中定义methods: 方法

```html
<body>
    <div id="app">
        <input type="button" value="v-on指令" v-on:click="doIt">
        <input type="button" value="v-on简写" @click="doIt">
    	<input type="button" value="双击事件" @dblclick="doIt">
    </div>
    <script src="js/vue.js"></script>
    <script>
        var app = new Vue({
            el: "#app",
            methods: {
                doIt: function() {
                    alert("hello");
                }
            }
        })
    </script>
</body>
```

方法增加参数
@keyup.enter 按键回车后调用方法
```html
<body>
    <div id="app">
        <input type="text" @keyup.enter="inputData(2233)">
    </div>
    
    <script src="js/vue.js"></script>
    <script>
        var app = new Vue({
            el: "#app",
            methods: {
                inputData: function(p1) {
                    alert(p1);
                }
            }
        })
    </script>
</body>
```

## this

this.dataName 可以修改数据

```html
<body>
    <div id="app">
        <h2 @click="changeData">{{fruit}}</h2> 
    </div>
    
    <script src="js/vue.js"></script>
    <script>
        var app = new Vue({
            el: "#app",
            data:{
                fruit: "apple"
            },
            methods: {
                changeData: function() {
                    this.fruit += " banana"
                }
            }
        })
    </script>
</body>
```

# v-show

v-show: 选择是否显示元素
true为显示，false为隐藏

```html
<body>
    <div id="app">
        <input type="button" value="切换显示状态" @click="changeShow">
        <h1 v-show="isShow">hello</h1>
        <h1 v-show="age >= 18">hello</h1>
    </div>
    
    <script src="js/vue.js"></script>
    <script>
        var app = new Vue({
            el: "#app",
            data:{
                isShow: true,
				age: 18
            },
            methods: {
                changeShow: function() {
                    this.isShow = !this.isShow;
                }
            }
        })
    </script>
</body>
```

## v-if

通过操作dom来切换显示状态，
为false则移除dom

```html
<body>
    <div id="app">
        <input type="button" value="显示" @click="changeShow">
        <h1 v-if="isShow">hello</h1>
    </div>

    <script src="js/vue.js"></script>
    <script>
        var app = new Vue({
            el: "#app",
            data: {
                isShow: true
            },
            methods: {
                changeShow: function() {
                    this.isShow = !this.isShow;
                }
            }
        })
    </script>
</body
```

## v-bind

v-bind：为元素绑定属性
简写为':'
支持三元运算表达式
也可以直接{active:true}
```html
<style>
	.active{
		background-color: red;
    }
</style>
<body>
    <div id="app">
        <a v-bind:href="src">链接</a>
        <a :href="src" :class="isActive ? 'active':''">链接</a>
        <a :href="src" :class="{active:isActive}">链接</a>
    </div>

    <script src="js/vue.js"></script>
    <script>
        var app = new Vue({
            el: "#app",
            data:{
                src: "https://www.baidu.com",
                isActive: true
            }
        })
    </script>
</body>
```

## v-for

遍历数据

```html
<body>
    <div id="app">
        <ul>
            <li v-for="item, index in arr">{{index}}: {{item}}</li>
        </ul>
        <ul>
            <li v-for="item in fruit">{{item.name}}</li>
        </ul>
        
    </div>
    
    <script src="js/vue.js"></script>
    <script>
        var app = new Vue({
            el: "#app",
            data: {
                arr: [1, 2, 3, 4, 5],
                fruit: [
                    {name: "apple"},
                    {name: "banana"},
                    {name: "pear"}
                ]
            }
        })
    </script>
</body>
```

## v-model

表单元素和绑定的值双向关联，改变其中一项，另一项同步改变

```html
<body>
    <div id="app">
        <input type="text" v-model="message"> 
        <h2 v-text="message"></h2>
    </div>
    
    <script src="js/vue.js"></script>
    <script>
        var app = new Vue({
            el: "#app",
            data: {
                message: "hello"
            }
        })
    </script>
</body>
```

# api
1. 网易云搜素：https://autumnfish.cn/search?keywords=songName
2. 网易云播放：https://autumnfish.cn/song/url?id=437250680

# 实战经验

## main.js代码引入
import './plugins/element.js'

## element.js组件引入
import { Button } from 'element-ui'
Vue.use(Button)

## 表单组件

prefix-icon添加图标

```html
<el-form>
	<el-form-item>
		<el-input  prefix-icon="iconfont icon-user"></el-input>
	</el-form-item>
</el-form>
```

## 数据绑定

```html
<el-form :model="loginForm">
	<el-form-item>
		<el-input v-model="loginForm.username"></el-input>
	</el-form-item>
</el-form>

<script>
	export default {
	  data () {
		  // 使用return包裹后数据中变量只在当前组件中生效，不会影响其他组件
		return {
		  loginForm: {
			username: 'zs'
		  }
		}
	  }
	}
</script>
```

## 数据验证

rules指定规则对象
prop指定验证的数据名
然后在data中定义
```html
		// 表单验证规则对象
		loginFormRules: {
			username: [
			  { required: true, message: '请输入登录名称', trigger: 'blur' },
			  { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
			]
	  }
```

```html
<el-form :model="loginForm" :rules="loginFormRules">
	<el-form-item prop="username">
		<el-input v-model="loginForm.username"></el-input>
	</el-form-item>
</el-form>

<script>
	export default {
	 data () {
		return {
		  loginForm: {
			username: 'zs'
		  }
		},
		// 表单验证规则对象
		loginFormRules: {
			username: [
			  { required: true, message: '请输入登录名称', trigger: 'blur' },
			  { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
			]
	  }
	 }
	}
</script>
```

## 预验证

ref指定预验证的对象loginFormRef
验证成功valid为true
```html
	 methods: {
		 login () {
			this.$refs.loginFormRef.validate(valid => {
				console.log(valid)
			})
		 }
	 }
```

```html
<el-form :rules="loginFormRules" :model="loginForm" ref="loginFormRef">
	<el-form-item prop="username">
		<el-input v-model="loginForm.username"></el-input>
	</el-form-item>
	<el-form-item>
        <el-button type="primary" @click="login">登录</el-button>
    </el-form-item>
</el-form>

<script>
	export default {
	 data () {
		return {
		  loginForm: {
			username: 'zs'
		  }
		},
		// 表单验证规则对象
		loginFormRules: {
			username: [
			  { required: true, message: '请输入登录名称', trigger: 'blur' },
			  { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
			]
	  }
	 },
	 methods: {
		 login () {
			this.$refs.loginFormRef.validate(valid => {
				console.log(valid)
			})
		 }
	 }
	}
</script>
```

## 表单重置

ref指定对象loginFormRef
click事件resetLoginForm
语句this.$refs.loginFormRef.resetFields()


```html
<el-form ref="loginFormRef" :model="loginForm">
	<el-form-item>
		<el-input v-model="loginForm.username"></el-input>
	</el-form-item>
	<el-form-item class="btns">
          <el-button type="primary" @click="resetLoginForm">重置</el-button>
    </el-form-item>
</el-form>

<script>
	export default {
	 data () {
		return {
		  loginForm: {
			username: 'zs'
		  }
		},
		// 表单验证规则对象
		loginFormRules: {
			username: [
			  { required: true, message: '请输入登录名称', trigger: 'blur' },
			  { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
			]
	  }
	 },
	 methods: {
		resetLoginForm () {
			this.$refs.loginFormRef.resetFields()
		}
	 }
	}

```

## 使用axios
import axios form 'axios'

axios.defaults.baseURL='根路径'

Vue.config.$http=axios

使用:
// login是地址，this.loginForm是数据
this.$http.post('login', this.loginForm)

{data: res} 将post返回数据中的data属性解构出来并保存到res中

状态码为200，表示登录成功
```html
login () {
      this.$refs.loginFormRef.validate(async valid => {
        console.log(valid)
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) { return console.log('登录失败') }
        console.log('登录成功')
		}
}
```

## message消息提示

+ element.js引入

import { Message } from 'element-ui'

Vue.prototype.$message = Message

+ 使用

this.$message.error('登录失败！')

this.$message.success('登录成功!')


## token

保存token：window.sessionStorage.setItem('token', res.data.token)



## 页面跳转

新建页面Home.vue

在router.js中添加

import Home from '../components/Home.vue'

在routes中添加

{ path: '/home', component: Home }


```html
import Vue from 'vue'
import Router from 'vue-router'
import Login from '../components/Login.vue'
import Home from '../components/Home.vue'

import '../assets/css/global.css'
Vue.use(Router)

export default new Router({
  routes: [
    { path: '/', redirect: '/login' },
    { path: '/login', component: Login },
    { path: '/home', component: Home }
  ]
})
```

页面跳转：this.$router.push('/home')


## 路由导航守卫控制访问权限

如果用户没有登录，但是直接通过特定的url访问特定页面，需要重新导航到登录页面

```html
// 挂载路由导航守卫
router.beforeEach((to, from, next) => {
  // to将要访问的路径
  // from代表从哪个路径跳转而来
  // next是一个函数，代表放行
  // next(), next('/login') 强制跳转

  if (to.path === '/login') { return next() }
  // 获取token
  const tokenstr = window.sessionStorage.getItem('token')
  if (!tokenstr) return next('/login')
  next()
})
```

完整页面router.js

```html
import Vue from 'vue'
import Router from 'vue-router'
import Login from '../components/Login.vue'
import Home from '../components/Home.vue'

import '../assets/css/global.css'
Vue.use(Router)

const router = new Router({
  routes: [
    { path: '/', redirect: '/login' },
    { path: '/login', component: Login },
    { path: '/home', component: Home }
  ]
})

// 挂载路由导航守卫
router.beforeEach((to, from, next) => {
  // to将要访问的路径
  // from代表从哪个路径跳转而来
  // next是一个函数，代表放行
  // next(), next('/login') 强制跳转

  if (to.path === '/login') { return next() }
  // 获取token
  const tokenstr = window.sessionStorage.getItem('token')
  if (!tokenstr) return next('/login')
  next()
})
export default router
```

## container

+ element.js中导入

import { Container, Header, Aside, Main } from 'element-ui'

Vue.use(Container)

Vue.use(Header)

Vue.use(Aside)

Vue.use(Main)

+ 使用

```html
<el-container>
    <!-- 头部区域 -->
    <el-header>Header</el-header>
    <!-- 页面主体区域 -->
    <el-container>
      <el-aside width="200px">Aside</el-aside>
      <el-container>
        <!-- 右侧内容主题 -->
        <el-main>Main</el-main>
      </el-container>
    </el-container>
  </el-container>
```

## 导航

+ 引入

import { Menu, Submenu, MenuItemGroup, MenuItem } from 'element-ui'

Vue.use(Menu)

Vue.use(Submenu)

Vue.use(MenuItemGroup)

Vue.use(MenuItem)

```html
</el-header>
    <!-- 页面主体区域 -->
    <el-container>
      <!-- 侧边栏 -->
      <el-aside width="200px">
        <el-menu background-color="#333744" text-color="#fff" active-text-color="#ffd04b">
          <!-- 一级菜单 -->
          <el-submenu index="1">
            <template slot="title">
              <!-- 图标 -->
              <i class="el-icon-location"></i>
              <!-- 文本 -->
              <span>导航一</span>
            </template>
            <el-submenu index="1-4">
              <template slot="title">选项4</template>
              <el-menu-item index="1-4-1">选项1</el-menu-item>
            </el-submenu>
          </el-submenu>
        </el-menu>
      </el-aside>
      <el-container>
        <!-- 右侧内容主题 -->
        <el-main>Main</el-main>
      </el-container>
    </el-container>
  </el-container>
```

```html
   </el-header>
    <!-- 页面主体区域 -->
    <el-container>
      <!-- 侧边栏 -->
      <el-aside width="200px">
        <el-menu background-color="#333744" text-color="#fff" active-text-color="#ffd04b">
          <!-- 一级菜单 -->
          <el-submenu index="1">
            <template slot="title">
              <!-- 图标 -->
              <i class="el-icon-location"></i>
              <!-- 文本 -->
              <span>导航一</span>
            </template>
            <el-menu-item index="1-1">
              <template slot="title">
                <!-- 图标 -->
                <i class="el-icon-location"></i>
                <!-- 文本 -->
                <span>导航一</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <el-container>
        <!-- 右侧内容主题 -->
        <el-main>Main</el-main>
      </el-container>
    </el-container>
  </el-container>
```

## 获得menu数据

```html
created() {
      this.getMenuList()
    },
methods: {
      async getMenuList() {
        const { data: res } = await this.$http.get('menus')
        if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
        this.menulist = res.data
        console.log(res)
      }
    }
```

##  v-for生成menu

```html
<el-submenu :index="item.id + ''" v-for="item in menulist" :key="item.id">
```

## menu中添加unique-opened，同时只展开一个菜单

```html
<el-menu background-color="#333744" text-color="#fff" active-text-color="#409EFF" unique-opened>
```

## 设置侧边栏折叠

```html
<!-- 侧边栏根据isCollapse来设置宽度-->
<el-aside :width="isCollapse  ? '64px' : '200px'">
<div class="toggle-button" @click="toggleCollapse">|||</div>
<!-- unique-opened设置同时只展开一个菜单，collapse设置是否折叠，collapse-transition设置是否开始折叠动画-->
<el-menu unique-opened :collapse="isCollapse" :collapse-transition="false">
```



```css
.toggle-button {

  background-color: #4A5064;

  font-size: 10px;

  line-height: 24px;

  color: #fff;

  text-align: center;

  letter-spacing: 0.2em;  // 文字间距

  cursor: pointer;   // 鼠标移上去变成小手

 }
```

```javascript
 	// data中设置
 	isCollapse: false
	// methods中设置点击按钮切换菜单的折叠与展开
      toggleCollapse() {
        this.isCollapse = !this.isCollapse
      }
```

## 定义子页面

```javascript
import Welcome from '../components/Welcome.vue' //编写页面并引用到router.js中
// 设置welcome为home的子路由，并当进入home页面时，重定向到welcome
{
    path: '/home',
      component: Home,
      redirect: '/welcome',
      children: [
        { path: '/welcome', component: Welcome }
      ] }
```

```html
<!-- 在html中放置 -->
<router-view></router-view>
```

## 点击菜单跳转

+ router：启用该模式会在激活时以index作为path进行路由跳转

```javascript
routes: [
    { path: '/', redirect: '/login' },
    { path: '/login', component: Login },
    { path: '/home',
      component: Home,
      redirect: '/welcome',
      children: [
        { path: '/welcome', component: Welcome },
        { path: '/users', component: Users }
      ] }
  ]
```


```html
<el-menu-item :index="'/' + subItem.path" v-for="subItem in item.children" :key="subItem.id" @click="saveNavState('/' + subItem.path)">
```


## Breadcrumb导航

+ 引入

```javascript
import { Breadcrumb, BreadcrumbItem } from 'element-ui'
Vue.use(Breadcrumb)
Vue.use(BreadcrumbItem)
```

```html
<el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>用户管理</el-breadcrumb-item>
      <el-breadcrumb-item>用户列表</el-breadcrumb-item>
</el-breadcrumb>
```


## Card和布局

+ 引入

```javascript
import { Card, Row, Col } from 'element-ui'
Vue.use(Card)
Vue.use(Row)
Vue.use(Col)
```

```html
<el-card>
    <!-- gutter指定间隔 -->
      <el-row :gutter="20">
          <!-- 占 1/7-->
        <el-col :span="7">
          <el-input placeholder="请输入内容">
            <el-button slot="append" icon="el-icon-search"></el-button>
          </el-input>
        </el-col>
        <el-col :span="4">
          <el-button type="primary">添加用户</el-button>
        </el-col>
      </el-row>
    </el-card>
```

## table

+ 引入

```javascript
import { Table, TableColumn } from 'element-ui'
Vue.use(Table)
Vue.use(TableColumn)
```

```html
	<!-- 用户列表区域, userList为json格式的数据， prop指定对应的数据, , border添加边框，strip添加斑马纹-->
      <el-table :data="userList" border>
		<!-- index自动标号 -->
        <el-table-column type="index" label="#"></el-table-column>
        <el-table-column label="姓名" prop="username"></el-table-column>
        <el-table-column label="邮箱" prop="email"></el-table-column>
        <el-table-column label="电话" prop="mobile"></el-table-column>
        <el-table-column label="角色" prop="role_name"></el-table-column>
        <el-table-column label="状态" prop="mg_state"></el-table-column>
        <el-table-column label="操作"></el-table-column>
      </el-table>
```

## 样式button

```html
<!-- size指定大小-->
<el-button type="primary" icon="el-icon-edit" size="mini"></el-button>
<el-button type="danger" icon="el-icon-delete" size="mini"></el-button>
<el-button type="warning" icon="el-icon-setting" size="mini"></el-button>
```

## Tooltip

+ 引入

```javascript
import { Tooltip } from 'element-ui'
Vue.use(Tooltip)
```

```html
<!-- placement： 放置位置, enterable: 鼠标是否可以进入-->
<el-tooltip effect="dark" content="分配权限" :enterable="false" placement="bottom-end">
	<el-button type="warning" icon="el-icon-setting" size="mini"></el-button>
</el-tooltip>
```

## pagination分页

+ 引入

```javascript
import { pagination } from 'element-ui'
Vue.use(pagination)
```

```html
<!-- 分页,  @size-change: 页数改变事件 @current-change: 页号改变事件 :current-page: 当前页数 :page-size： 当前页号 layout: 显示哪些内容-->
      <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="queryInfo.pagenum" :page-sizes="[1, 2, 5, 10]" :page-size="queryInfo.pagesize" layout="total, sizes, prev, pager, next, jumper" :total="total">
      </el-pagination>

 data() {
      return {
        queryInfo: {
          query: '',
          //当前的页数
          pagenum: 1,
          //当前页显示多少数据
          pagesize: 2
        },
        userList: [],
        total: 0
      }
    },
 methods: {
      async getUserList() {
        const { data: res } = await this.$http.get('users', { params: this.queryInfo })
        if (res.meta.status !== 200) return this.$message.error("获取用户列表失败")
        this.userList = res.data.users
        this.total = res.data.total
        console.log(res)
      },
      handleSizeChange(newSize) {
        console.log(newSize)
        this.queryInfo.pagesize = newSize
        this.getUserList()
      },
      handleCurrentChange(newPage) {
        console.log(newPage)
        this.queryInfo.pagenum = newPage
        this.getUserList()
      }
    }
```

## input清空

```html
 <!-- clearable: 清空图标, @clear：清空后的事件-->
<el-input placeholder="请输入内容" v-model="queryInfo.query" clearable @clear="getUserList">
```

## Dialog

+ 引入

```html
import { Dialog } from 'element-ui'
Vue.use(Dialog)
```

```html
<!-- :visible.sync: 控制对话框是否显示-->
<el-dialog
  title="提示"
  :visible.sync="dialogVisible"
  width="30%"
  :before-close="handleClose">
  <span>这是一段信息</span>
  <span slot="footer" class="dialog-footer">
    <el-form-item label="用户名">
    	<el-input v-model="addForm.username"></el-input>
    </el-form-item>
    <el-button @click="dialogVisible = false">取 消</el-button>
    <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
  </span>
</el-dialog>
```

