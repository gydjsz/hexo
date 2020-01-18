---
title: vue学习(二)
tags:
- vue
---

# 事件修饰符

+ .stop 阻止冒泡
+ .prevent 阻止默认事件
+ .capture 添加事件侦听器时使用事件捕获模式
+ .self 只当事件在该元素本身(比如不是子元素)触发时回调
+ .once 事件只触发一次

<!--more-->

1. .stop

从内到外触发事件为冒泡

以下代码会存在冒泡行为

div有onClick事件, 其子元素也有onClick事件, 当点击子元素时, div也会触发事件

```html
<div id="app">
  <div @click="divClick" style="background-color: aquamarine;">
      <input type="button" value="点击" @click="btnClick">
  </div>
</div>
<script>
  var app = new Vue({
    el: '#app',
    methods: {
        divClick(){
            console.log('divClick');
        },
        btnClick(){
            console.log('btnClick');
        }
    }
  })
</script>
```

使用.stop阻止冒泡行为
```html
<div id="app">
  <div @click="divClick" style="background-color: aquamarine;">
      <input type="button" value="点击" @click.stop="btnClick">
  </div>
</div>
<script>
  var app = new Vue({
    el: '#app',
    methods: {
        divClick(){
            console.log('divClick');
        },
        btnClick(){
            console.log('btnClick');
        }
    }
  })
</script>
```

2. .prevent

a链接有默认的事件: 点击后跳转

使用.prevent可以阻止该事件的发生

```html
<body>
<div id="app">
  <a href="https://www.baidu.com" @click.prevent="linkFun">跳转</a>
</div>
<script>
  var app = new Vue({
    el: '#app',
    methods: {
      linkFun() {
        console.log('prevent')
      }
    }
  })
</script>
```

3. .capture

从外到内触发事件为捕获

此处点击按钮, 先触发divClick, 后触发btnClick

```html
<div id="app">
  <div @click.capture="divClick" style="background-color: aquamarine;">
    <input type="button" value="点击" @click="btnClick" />
  </div>
</div>
<script>
  var app = new Vue({
    el: '#app',
    methods: {
      divClick() {
        console.log('divClick')
      },
      btnClick() {
        console.log('btnClick')
      }
    }
  })
</script>
```

4. .self

只有点击元素的时候才会触发事件

```html
<div id="app">
  <div @click.self="divClick" style="background-color: aquamarine;">
    <input type="button" value="点击" @click="btnClick" />
  </div>
</div>
<script>
  var app = new Vue({
    el: '#app',
    methods: {
      divClick() {
        console.log('divClick')
      },
      btnClick() {
        console.log('btnClick')
      }
    }
  })
</script>
```

5. .once

只触发一次事件处理函数

第一次点击触发linkFun函数, 不跳转
第二次点击不触发linkFun函数, 进行跳转

```html
<a href="https://www.baidu.com" @click.prevent.once="linkFun">link</a>
```

# 在vue中使用样式

## 使用class样式

默认:
```html
<style>
  .red {
    color: red;
  }
  .thin {
    font-weight: 300;
  }
</style>
<h1 class="red thin">hello, world</h1>
```

1. 数组

```html
<style>
    .red {
        color: red;
    }
    .thin {
        font-weight: 300;
    }
</style>
<div id="app">
    <h1 :class="['red', 'thin']">hello, world</h1>
</div>
<script>
    var app = new Vue({
        el: '#app'
    })
</script>
```

2. 使用三元表达式

```html
<style>
    .red {
        color: red;
    }
    .thin {
        font-weight: 300;
    }
</style>
<div id="app">
    <h1 :class="['red', isActive ? 'thin' : '']">hello, world</h1>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            isActive: true
        }
    })
</script>
```

3. 使用对象来代替三元表达式

```html
<style>
    .red {
        color: red;
    }
    .thin {
        font-weight: 300;
    }
</style>
<div id="app">
    <h1 :class="['red', {'thin': isActive}]">hello, world</h1>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            isActive: true
        }
    })
</script>
```

4. 直接使用对象

```html
<style>
    .red {
        color: red;
    }
    .thin {
        font-weight: 300;
    }
</style>
<div id="app">
    <h1 :class="classObj">hello, world</h1>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            classObj: {red: true, thin: true}
        }
    })
</script>
```

# 添加内联样式

```html
<h1 :style="[style1, style2]"></h1>
data: {
    style1: {},
    style2: {}
}
```

# v-for迭代数字

count的值从1开始
```html
<h1 v-for="count in 10">{{count}}</h1> 
```

# v-if和v-show的区别

+ v-if 有较高的切换性能消耗, 当值为false时, 元素直接被移除
+ v-show 有较高的初始渲染消耗, 当值为false时, 样式中的display被设置为none, 涉及到频繁的切换时使用

# v-for进行列表的操作

```html
<div id="app">
  <div class="panel panel-primary">
    <div class="panel-heading">
      <h3 class="panel-title">添加品牌</h3>
    </div>
    <div class="panel-body form-inline">
      <label>
        Id:
        <input type="text" class="form-control" v-model="id" />
      </label>
      <label>
        Name:
        <input type="text" class="form-control" v-model="name" @keyup.enter="add"/>
      </label>
      <input
        type="button"
        value="添加"
        class="btn btn-primary"
        @click="add"
      />
      <label>
        搜索名称关键字:
        <input type="text" class="form-control" v-model="keywords" />
      </label>
    </div>
  </div>

  <table class="table table-bordered table-hover table-striped">
    <thead>
      <tr>
        <th>Id</th>
        <th>Name</th>
        <th>Ctime</th>
        <th>Operation</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(item, index) in search(keywords)" :key="index">
        <td>{{item.id}}</td>
        <td>{{item.name}}</td>
        <td>{{item.ctime | dateFormat}}</td>
        <td>
          <a href="" @click.prevent="del(index)">删除</a>
        </td>
      </tr>
    </tbody>
  </table>
</div>

<script>
    Vue.filter('dateFormat', function(dateStr) {
        var dt = new Date(dateStr)
        var y = dt.getFullYear()
        var m = (dt.getMonth() + 1).toString().padStart(2, '0')
        var d = dt.getDate().toString().padStart(2, '0')
        var hh = dt.getHours().toString().padStart(2, '0')
        var mm = dt.getMinutes().toString().padStart(2, '0')
        var ss = dt.getSeconds().toString().padStart(2, '0') 
        //模板字符串
        return `${y}-${m}-${d} ${hh}:${mm}:${ss}`
    })
    var app = new Vue({
        el: '#app',
        data: {
        id: '',
        name: '',
        keywords: '',
        list: [
            { id: 1, name: '奔驰', ctime: new Date() },
            { id: 2, name: '宝马', ctime: new Date() }
        ]
        },
        methods: {
            add() {
                if (!this.id || !this.name) return
                this.list.push({
                id: this.id,
                name: this.name,
                ctime: new Date()
                })
                this.id = ''
                this.name = ''
            },
            del(id) {
                this.list.splice(id, 1)
            },
            search(keywords) {
                //方法一: 通过遍历来搜索
                //   var newList = []
                //   this.list.forEach(item => {
                //       if(item.name.includes(keywords))
                //         newList.push(item)
                //   })
                //   return newList

                //方法二: 通过filter来筛选 
                return this.list.filter(item => {
                return item.name.includes(keywords);
                })
            }
        }
    })
</script>
```

1. splice

在数组中添加/删除项目, 然后返回被删除的项目

```javascript
array.splice(index, howmany, item1, ..., itemX)
```

+ index: 必须, 规定位置, 使用负数表示从结尾处开始规定位置
+ howmany: 必须, 表示要删除的数量, 为0, 则不会删除
+ item1,...,itemX: 可选, 向数组中添加新的项目

2. forEach

```javascript
array.forEach(function(currentValue, index, arr), thisValue)
```

+ currentValue: 必须项, 表示当前元素
+ index: 可选, 表示当前元素的索引值
+ arr: 可选, 表示当前元素所属的数组对象
+ thisValue: 可选, 对象作为该执行回调时使用, 传递给函数, 用作 "this" 的值。
如果省略了thisValue, "this" 的值为 "undefined"

3. filter

创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素

```javascript
array.filter(function(currentValue, index, arr), thisValue)
```

参数和forEach一致

4. includes

用于判断字符串是否包含指定的子字符串
找到匹配的返回true, 否则返回false

```javascript
str.includes('hello')
```

# vue过滤器

```html
<div id="app">
  <p>{{msg | msgFilter('w') | msgFilter2}}</p>
</div>

<script>
    //全局过滤器, 所有的Vue实例都会共享
  Vue.filter('msgFilter', function(msg, arg) {
      //正则表达式, 将文本中的h替换成变量arg所表示的内容
    return msg.replace(/h/g, arg)
  })
  Vue.filter('msgFilter2', function(msg, arg){
      return msg + '2233'
  })
  var app = new Vue({
    el: '#app',
    data: {
      msg: 'hello, world'
    }
  })
</script>
```

+ 在new Vue之前声明filter

function中的第一个值是'|'左边的值, 第二个是调用过滤器时传入的参数

```javascript
Vue.filter('msgFilter', function(msg, arg) {
    //正则表达式, 将文本中的h替换成变量arg所表示的内容
  return msg.replace(/h/g, arg)
})
```

+ 通过插值表达式或者v-bind来调用, 多个过滤器通过'|'连接

```html
<p>{{msg | msgFilter('w') | msgFilter2}}</p>
```

+ 私有过滤器

如果全局过滤器和私有过滤器名称一样, 优先调用私有过滤器

```javascript
var app = new Vue(){
    filters: {
        testFilter(msg, arg){
            return msg + '2233'
        }
    }
}
```

# padStart和padEnd

+ padStart: 在头部补全字符串
+ padEnd: 在尾部补全字符串

字符串不足两位, 在字符前面补0
```javascript
str.padStart(2, '0')
```

# keyup

通过按键修饰符来监听键盘事件

```javascript
//按下回车键时触发add()事件
@keyup.enter="add()"
```

+ .enter
+ .tab
+ .delete (捕获“删除”和“退格”键)
+ .esc
+ .space
+ .up
+ .down
+ .left
+ .right

# 自定义全局按键修饰符

enter键的键码为13

```html
//按下回车键触发fun方法
<input type="text" @keyup.et="fun">
<script>
    //自定义修饰符为et, 回车键的键码为13
    Vue.config.keyCodes.et = 13
</script>
```

# 自定义指令

以下代码实现新增一个v-foces指令, 使得页面加载完自动获得输入框焦点
```html
<div id="app">
    <input type="text" v-focus>
</div>
<script>
    Vue.directive('focus', {
        inserted(el){
            el.focus()
        }
    })
    var app = new Vue({
        el: '#app'
    })
</script> 
```

+ 使用Vue.directive('name', {})定义全局指令
+ 参数1: 指令的名称, 定义的时候不需要加'v-', 使用的时候添加
+ 参数2: 对象, 在这个对象中有相关的函数, 函数在特定的阶段执行相关的操作

```javascript
Vue.directive('focus', {
    //当指令绑定到元素上时, 会立即执行, 只执行一次
    bind(el){},
    //元素插入dom时, 会执行, 触发一次
    inserted(el){},
    //VNode更新时调用, 触发多次
    updated(el){},
    //所在组件的VNode及其VNode全部更新后调用
    componentUpdated(el){},
    //指令与元素解绑时调用, 只调用一次
    unbind(el){}
})
```

el: 指令所绑定的元素, 可以用来直接操作dom

binding：一个对象，包含以下属性：
+ name：指令名，不包括 v- 前缀。
+ value：指令的绑定值，例如：v-my-directive="1 + 1" 中，绑定值为 2。
+ oldValue：指令绑定的前一个值，仅在 update 和 componentUpdated 钩子中可用。无论值是否改变都可用。
+ expression：字符串形式的指令表达式。例如 v-my-directive="1 + 1" 中，表达式为 "1 + 1"。
+ arg：传给指令的参数，可选。例如 v-my-directive:foo 中，参数为 "foo"。
+ modifiers：一个包含修饰符的对象。例如：v-my-directive.foo.bar 中，修饰符对象为 { foo: true, bar: true }。
vnode：Vue 编译生成的虚拟节点。移步 VNode API 来了解更多详情。
oldVnode：上一个虚拟节点，仅在 update 和 componentUpdated 钩子中可用。

可以在bind()方法中修改元素的style
```html
<input type="text" v-color>
<script>
    Vue.directive('color', {
        bind(el){
            el.style.color = 'red'
        }
    })
</script>
```

通过binding参数获取传入的值
```javascript
<input type="text" v-color="'red'">
<script>
    Vue.directive('color', {
        bind(el, binding){
            el.style.color = binding.value
        }
    })
</script>
```

+ 私有指令

```javascript
var app = new Vue({
    el: '#app',
    directives: {
        'color': {
            bind(el, binding){
                el.style.color = binding.value
            }
        }
    }
})
```

+ 简写

等同于把方法写入了bind和update中
```javascript
Vue.directive('color', function (el, binding) {
  el.style.color = binding.value
})

var app = new Vue({
    el: '#app',
    directives: {
        color(el, binding){
            el.style.color = binding.value
        }
    }
})
```

# 生命周期

<img src="https://cn.vuejs.org/images/lifecycle.png" width="50%" height="50%"/>


+ beforeCreate: 在该生命周期函数执行的时候, data和methods中的数据没有初始化
+ created: data和methods初始化好了
+ created到beforeMount之间的过程是模板编译过程, 最终在内存中生成一个编译好的模板字符串, 并渲染为内存中dom, 并没有挂载到页面中
+ befroeMount: 模板在内存中编译完成, 但尚未渲染到页面
+ mounted: 模板已经挂载到页面中, 页面渲染完成, 如果要通过某些插件操作页面上的dom节点, 最早要在mounted中进行, 只要执行完mounted, 就表示整个vue实例初始化完毕
+ beforeUpdate: 界面未被更新, 数据已经更新
+ updated: 页面和数据已经保持同步
+ beforeDestory: 进入销毁状态, 但还没执行销毁过程
+ destoryed: 组件被完全销毁

# 动画

1. 使用transitation元素将需要被动画控制的元素包裹起来

```html
<transition>
    <h3 v-if="flag">hello</h3>
</transition>
```

2. 自定义两组样式来控制transition内部的元素实现动画

```css
.v-enter,
.v-leave-to{
    opacity: 0;
    transform: translateX(150px);
}
.v-enter-active,
.v-leave-active{
    transition: all 0.8s ease;
}
```

+ v-enter: 定义进入过渡的开始状态
+ v-enter-active: 定义进入过渡生效时的状态
+ v-enter-to: 定义进入过渡的结束状态
+ v-leave: 定义离开过渡的开始状态
+ v-leave-active: 定义离开过渡生效时的状态
+ v-leave-to: 定义离开过渡的结束状态

```html
<style>
    .v-enter,
    .v-leave-to{
        opacity: 0;
    }
    .v-enter-active,
    .v-leave-active{
        transition: all 0.8s ease;
    }
</style>
<div id="app">
  <input type="button" value="显示" @click="flag=!flag" />
  <transition>
    <h3 v-if="flag">hello</h3>
  </transition>
</div>
<script>
  var app = new Vue({
    el: '#app',
    data: {
      flag: false
    }
  })
</script>
```

3. 通过给transition指定name属性可以自定义类名

```html
<style>
    .my-enter,
    .my-leave-to{
        opacity: 0;
        transform: translateY(150px);
    }
    .my-enter-active,
    .my-leave-active{
        transition: all 0.8s ease;
    }
</style>
<div id="app">
  <input type="button" value="显示" @click="flag=!flag" />
  <transition name="my">
    <h3 v-if="flag">hello</h3>
  </transition>
</div>
<script>
  var app = new Vue({
    el: '#app',
    data: {
      flag: false
    }
  })
</script>
```

# 使用第三方库animate.css实现动画

```html
<div id="app">
  <input type="button" value="toggle" @click="flag=!flag" />
  <transition
    enter-active-class="animated bounceIn"
    leave-active-class="animated bounceOut"
  :duration="400">
    <h3 v-if="flag">hello</h3>
  </transition>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            flag: false
        }
    })
</script>
```

在enter-active-class中指定animate.css中的类名bounceIn, 并加上animated

可以简写
```html
<transition
    enter-active-class="bounceIn"
    leave-active-class="bounceOut"
  :duration="400">
    <h3 v-if="flag" class="animated">hello</h3>
</transition>
```

使用**:duration="毫秒值"**来统一设置入场和离场时的动画时长

分别设置时长**:duration="{enter: 200, leave: 400}"**

# JavaScript钩子

```javascript
methods: {
    //进入
    beforeEnter: function (el) {},
     // 当与 CSS 结合使用时
    // 回调函数 done 是可选的
    enter: function (el, done) {
        done()
    },
    afterEnter: function (el) {},
    enterCancelled: function (el) {},

    //离开
    beforeLeave: function (el) {},
    // 当与 CSS 结合使用时
    // 回调函数 done 是可选的
    leave: function (el, done) {
        // ...
        done()
    },
    afterLeave: function (el) {},
    // leaveCancelled 只用于 v-show 中
    leaveCancelled: function (el) {}
}
```

```html
<style>
      .ball {
        width: 15px;
        height: 15px;
        border-radius: 50%;
        background-color: red;
      }
    </style>
  </head>
  <body>
    <div id="app">
      <input type="button" value="跳动" @click="flag=!flag" />
      <transition
        @before-enter="beforeEnter"
        @enter="enter"
        @after-enter="afterEnter"
      >
        <div class="ball" v-show="flag"></div>
      </transition>
    </div>
    <script>
      var app = new Vue({
        el: '#app',
        data: {
          flag: false
        },
    methods: {
        beforeEnter(el){
            //动画入场之前, 此时动画尚未开始
            //设置小球起始位置
            el.style.transform = 'translate(0, 0)'
        },
        enter(el, done){
            el.offsetWidth
            //动画开始之后, 可以设置小球动画完成之后的结束状态
            el.style.transform = "translate(150px, 450px)"
            el.style.transition = "all 1s ease"
            //done是afterEnter的引用
            done()
        },
        afterEnter(el){
            //动画完成之后调用
            this.flag = !this.flag
        }
    },
  })
</script>
```

# 列表动画

```html
<style>
    li {
      border: 1px dashed #999;
      margin: 5px;
      line-height: 35px;
      padding-left: 5px;
      font-size: 12px;
    }
    li:hover {
      background-color: rgb(189, 126, 44);
      transition: all 0.8s ease;
    }
    .v-enter,
    .v-leave-to {
      opacity: 0;
      transform: translateY(80px);
    }

    .v-enter-active,
    .v-leave-active {
      transition: all 0.6s ease;
    }
  </style>
</head>
<body>
  <div id="app">
    <div>
      <label>
        Id:
        <input type="text" v-model="id" />
        Name:
        <input type="text" v-model="name" />
        <input type="button" value="添加" @click="add" />
      </label>
    </div>
    <!-- <ul> -->
    <transition-group appear tag="ul">
      <li v-for="item, index in list" :key="index" @click="del(index)">
        {{item.id}}---{{item.name}}
      </li>
    </transition-group>
    <!-- </ul> -->
  </div>
  <script>
    var app = new Vue({
      el: '#app',
      data: {
        id: '',
        name: '',
        list: [
          { id: 1, name: 'a' },
          { id: 2, name: 'b' },
          { id: 3, name: 'c' },
          { id: 4, name: 'd' }
        ]
      },
      methods: {
        add() {
          this.list.push({ id: this.id, name: this.name })
        }
      }
    })
  </script>
</body>
```

+ v-for实现动画需要使用transition-group元素
+ 必须添加":key"属性
+ 在transition-group中添加appear属性可以实现入场效果
+ transition-group默认渲染一个span元素, 此时可以为其设置tag="ul", 同时列表中的ul元素可以省去
