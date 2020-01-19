---
title: vue组件
tags:
- vue
---

# 组件的使用

1. 使用Vue.extend来创建一个全局组件

<!--more-->

```javascript
var com1 = Vue.extend({
  template: '<h3>使用Vue.extend创建的组件</h3>'
})
```

2. Vue.component('组件的名称', 创建出来的组件模板对象)

```javascript
Vue.component('myCom1', com1)
```

3. 直接将组件的名称以标签的形式引入到页面中即可, 驼峰命名法需要修改成小写并加上"-"

```html
<div id="app">
    <my-com1></my-com1>
</div>
```

```html
<div id="app">
    <my-com1></my-com1>
</div>
<script>
  var com1 = Vue.extend({
    template: '<h3>使用Vue.extend创建的组件</h3>'
  })
  Vue.component('myCom1', com1)
  var app = new Vue({
    el: '#app',
    data: {}
  })
</script>
```

简写
```javascript
Vue.component('myCom1', {
    template: '<h3>使用Vue.extend创建的组件</h3>'
})
```

注意:
组件template的内容只能有一个根元素

以下是错误的
```javascript
template: '<h1></h1><h2></h2>'
```

可以用div来包裹
```javascript
template: '<div><h1></h1><h2></h2></div>'
```

## 在外面定义模板结构

```html
<div id="app">
  <my-com></my-com>
</div>
<template id="tmp">
  <div>
    <h1>hello</h1>
  </div>
</template>
<script>
  Vue.component('myCom', {
    template: '#tmp'
  })
  var app = new Vue({
    el: '#app'
  })
</script>
```

# 定义私有组件

```html
<div id="app">
    <com></com>
</div> 
<template id="com1">
    <div>
        <h1>hello</h1>
    </div>
</template>
<script>
    var app = new Vue({
        el: '#app',
        components: {
            com: {
                template: '#com1'
            }
        }
    })
</script>
```

# 组件中使用data

组件中的data为一个方法, 方法中返回一个对象

```html
<div id="app">
    <counter></counter>
</div>
<template id="com">
  <div>
    <input type="button" value="+1" @click="increment" />
    <h1>{{count}}</h1>
  </div>
</template>
<script>
  Vue.component('counter', {
    template: '#com',
    data() {
      return {
        count: 0
      }
    },
    methods: {
      increment() {
        this.count++
      }
    }
  })
  var app = new Vue({
    el: '#app'
  })
</script>
```

# 多组件之间的切换

:is属性用来指定要展示的组件的名称
```html
<component :is="myCom"></component>
```

组件之间的切换动画使用transition包裹, 设置:is属性即可
其中有mode属性, 设置其为"out-in", 表示先让组件离开完成, 再让下一个组件出现
```html
<style>
      .v-enter,
      .v-leave-to{
          opacity: 0;
          transform: translateX(150px);
      }
      .v-enter-active,
      .v-leave-active{
          transition: all 0.5s ease;
      }
  </style>

</head>
<body>
  <div id="app">
    <a href="" @click.prevent="comName='login'">登录</a>
    <a href="" @click.prevent="comName='register'">注册</a>
    <transition mode="out-in">
      <component :is="comName"></component>
    </transition>
  </div>
  <script>
    Vue.component('login', {
      template: '<h1>登录</h1>'
    })
    Vue.component('register', {
      template: '<h1>注册</h1>'
    })
    var app = new Vue({
      el: '#app',
      data: {
        comName: 'login'
      }
    })
  </script>
</body>
```

# 父组件向子组件中传值

1. 父组件中有值msg

```javascript
data: {
    msg: '父组件'
}
```

2. 编写子组件,子组件中使用props来指定父组件传入的变量, props中的数据只读, 无法重新赋值

```javascript
components: {
  com1: {
    data() {
      return {}
    },
    template: '<h1>{{parentmsg}}</h1>',
    props: ['parentmsg'],
    directives: {},
    filters: {},
    components: {},
    methods: {}
   }
}
```

3. 将组件应用到页面中, 并使用v-bind来绑定父组件中的值

```html
<div id="app">
    <com1 :parentmsg="msg"></com1>
</div>
```

4. 子组件中的data方法保存自己的私有数据
```javascript
data() {
  return {
      content: 'hello, world'
  }
}
```

```html
<div id="app">
    <com1 :parentmsg="msg"></com1>
</div>
<script>
  var app = new Vue({
    el: '#app',
    data: {
        msg: '父组件'
    },
    components: {
      com1: {
        data() {
          return {
              content: 'hello, world'
          }
        },
        template: '<h1>{{parentmsg}}</h1>',
        props: ['parentmsg'],
        directives: {},
        filters: {},
        components: {},
        methods: {}
      }
    }
  })
</script>
```

# 子组件向父组件中传值

+ 通过子组件调用父组件中的方法, 并传递值到该方法中, 来修改父组件中的值

```javascript
methods: {
    show(){
        console.log('调用父组件中的方法')
    }
}
```

+ 传递父组件中的方法使用v-on, 缩写@

```html
<com @func="show"></com>
```

show为父组件中的方法, func为该方法在子组件中的方法名

这里show不能写成show(), 因为show表示方法对象, 而show()表示方法返回的值

在子组件中通过this.$emit('myFun', arg1, ..., argn)来调用父组件传到子组件中的方法

```html
<div id="app">
    <com @func="show"></com>
</div>
<template id="tmp">
  <div>
    <input type="button" value="调用方法" @click="myClick">
  </div>
</template>
<script>
  var com = {
    template: '#tmp', 
    methods: {
        myClick(){
            this.$emit('func')
        }
    }
  }
  var app = new Vue({
    el: '#app',
    methods: {
        show(){
            console.log('调用父组件中的方法')
        }
    },
    components: {
      com: com
    }
  })
</script>
```

如果要传递值

```html
<div id="app">
    <com @func="show"></com>
</div>
<template id="tmp">
  <div>
    <input type="button" value="调用方法" @click="myClick">
  </div>
</template>
<script>
  var com = {
    template: '#tmp', 
    data(){
        return {
            sonmsg: '传递子组件中的值'
        }
    },
    methods: {
        myClick(){
            this.$emit('func', this.sonmsg)
        }
    }
  }
  var app = new Vue({
    el: '#app',
    methods: {
        show(data){
            console.log('调用父组件中的方法,' + data)
        }
    },
    components: {
      com: com
    }
  })
</script>
```

# 使用ref获取元素的dom和组件的引用

设置所在元素的ref属性, 然后通过"this.$refs.属性名"来获取dom

```html
<div id="app">
  <input type="button" value="获取h1值" @click="getElement" />
  <h1 ref="myH1">hello, world</h1>
</div>
<script>
  var app = new Vue({
    el: '#app',
    methods: {
      getElement() {
        console.log(this.$refs.myH1.innerText)
      }
    }
  })
</script>
```

获取组件的引用后可以获取组件内的数据和方法

```html
<div id="app">
  <input type="button" value="获取组件的引用" @click="myBtn" />
  <com ref="myCom"></com>
</div>
<template id="tmp">
  <div>
    <h1>hello, world</h1>
  </div>
</template>

<script>
  var app = new Vue({
    el: '#app',
    methods: {
      myBtn() {
        //输出组件中的数据
        console.log(this.$refs.myCom.msg)
        //调用组件中的方法
        this.$refs.myCom.show()
      }
    },
    components: {
      com: {
        template: '#tmp',
        data() {
          return {
            msg: 'hello'
          }
        },
        methods: {
          show() {
            console.log(this.msg)
          }
        }
      }
    }
  })
</script>
```

