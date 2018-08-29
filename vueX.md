### Vuex
我们知道父子组件之间数据的传递可以使用props或者$emit,但是大型项目中往往
要子组件之间数据传递，vue的状态管理工具vex可以解决这个问题
#### 1.安装并引入Vuex
```
    npm install vuex -s
```
在main.js中引入
```
    import Vue from 'vue'
    import App from './App'
    import Vuex from 'vuex'
    import store from './vuex/store'

    Vue.use(Vuex)

    /* eslint-disable no-new */
    new Vue({
        el: '#app',
        store,
        render: h => h(App)
    })
```
### 2.构建核心仓库store.js
Vuex应用状态state都应当存放在store.js里面，Vue组件可以从store.js里面获取状态，可以把store通俗理解为一个全局变量仓库
但是和单纯的全局变量又有一些区别，主要体现在当store中的状态发生改变时，相应的vue组件也会得到高效更新

在src目录下创建一个vuex目录，将store.js放到vuex目录下
```
import Vuex from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
    state: {
        author: 'lizi'
    }
})

export default store
```
以上例子只存放一个状态author