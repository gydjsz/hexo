---
title: vuex学习
tags:
- vue
---

通过vuex来进行非父子组件之间的通信

# 使用state存储变量

1. 安装: cnpm i vuex -S
2. 导包

```js
import Vuex from 'vuex'
Vue.use(Vuex)
```

3. 创建store对象

```js
export default new Vuex.Store({
    state: {
        isShow: true
    },
    mutations: {

    },
    actions: {

    }
})
```

store.js
```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    isShow: true
  }
})
```

4. 挂载到Vue实例

main.js
```js
import store from './store.js'

var app = new Vue({
  el: '#app',
  render: c => c(App),
  router,
  store
})
```

5. 使用

```js
console.log(this.$store.state.isShow)
```

## 访问State数据的第二种方式

导入mapState函数
```js
import { mapState } from 'vuex'
```

通过mapState函数, 可以将当前组件需要的全局数据映射为组件的computed计算属性

```js
computed: {
    ...mapState: (['count'])
}
```

访问数据

```js
{{count}}
```

# 通过mutations修改state中的数据

在mutations中编写函数, 第一个参数是state, 表示state对象, 第二个是data, 表示传入的数据

store.js
```js
export default new Vuex.Store({
  state: {
    isShow: true
  },
  mutations: {
    show(state, data) {
      state.isShow = data
    }
  }
})
```

Home.vue
通过commit调用show方法, 并传递参数来更改数据
```js
this.$store.commit('show', false)
```

## 触发mutations的第二种方式

1. 导入mapMutations函数

```js
import { mapMutations } from 'vuex'
```

2. 将指定的mutations函数, 映射为当前组件的methods函数

```js
methods: {
    ...mapMutations(['add'])
}
```

3. 使用
```js
this.add()
```

# 使用Action处理异步任务

在Action中通过触发Mutation的方式间接变更数据

```js
mutations: {
    add(state, data) {
      state.count += data
    }
},
actions: {
    addAsync(context, data) {
      setTimeout(() => {
        context.commit('add', data)
      }, 1000)
    }
}
```

使用
```js
this.$store.dispatch('addAsync', 1)
```

## 触发Actions的第二种方式

1. 导入mapActions函数

```js
import { mapActions } from 'vuex'
```

2. 通过导入的mapActions函数, 将actions函数映射为methods方法

```js
methods: {
    ...mapActions(['addAsync'])
}
```

3. 使用

```js
this.addAsync(5)
```

# 使用Getter对数据进行加工

Getter用于对Store中的数据进行加工处理形成新的数据

```js
getters: {
    showNum(state) {
      return '当前最新的数量是' + state.count
    }
}
```

使用
```js
this.$store.getters.showNum
```

## 使用getters的第二种方式

```js
import { mapGetters } from 'vuex'
```

```js
computed: {
    ...mapGetters(['showNum'])
}
```

使用
```js
{{showNum}}
```

