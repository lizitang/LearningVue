## vuex+axios发送请求
### 安装
使用npm
```
npm install axios -S
```
安装其他插件的时候，可以直接在main.js中引入并Vue.use(),但是axios并不能use,只能每个需要发送请求的组件中及时引入
为了解决这个问题，有两种开发思路，<span style="color:red;">一是在引入axios之后，修改原型链</span>;<span style="color:#080;">二是结合Vuex，封装一个action</span>
##### 方案一：改写原型链
首先在main.js中引入axios
```js
import axios from 'axios'
```
这个时候如果在其他的组件中，是无法使用axios命令的。但如果将axios改写为Vue的原型属性，就能解决这个问题
```js
Vue.prototype.$ajax = axios
```
在main.js中添加了这两行代码之后，就能直接在组件的methods中使用$ajax命令
```js
methods:{
    submitForm () {
        this.$ajax({
            method:'post',
            url: '/user',
            data:{
                name:'wise',
                info:'wrong'
            }
        })
    }
}
```
##### 方案二：在vuex中封装
之前的文章中用到过 Vuex 的 mutations，从结果上看，mutations 类似于事件，用于提交 Vuex 中的状态 state
action 和 mutations 也很类似，主要的区别在于，action 可以包含异步操作，而且可以通过 action 来提交 mutations
另外还有一个重要的区别：
mutations 有一个固有参数 state，接收的是 Vuex 中的 state 对象
action 也有一个固有参数 context，但是 context 是 state 的父级，包含  state、getters
Vuex 的仓库是 store.js，将 axios 引入，并在 action 添加新的方法
```js
// store.js
import Vue from 'Vue'
import Vuex from 'vuex'

// 引入 axios
import axios from 'axios'

Vue.use(Vuex)

const store = new Vuex.Store({
  // 定义状态
  state: {
    test01: {
      name: 'Wise Wrong'
    },
    test02: {
      tell: '12312345678'
    }
  },
  actions: {
    // 封装一个 ajax 方法
    saveForm (context) {
      axios({
        method: 'post',
        url: '/user',
        data: context.state.test02
      })
    }
  }
})

export default store
```
<span style="color:red;font-weight:bold;font-size:18px">注意：即使已经在main.js中引入了axios，并改写了原型链，也无法在store。js中直接使用$ajax命令，换言之，这两种方案是相互独立的</span>
在组件中发送请求的时候，需要使用this.\$store.dispatch来分发
```js
methods:{
    submitForm () {
        this.$store.dispatch('saveForm')
    }
}
```
submitForm 是绑定在组件上的一个方法，将触发 saveForm，从而通过 axios 向服务器发送请求
axios的完整api[点此链接](https://www.kancloud.cn/yunye/axios/234845)
为了方便，axios 还为每种方法起了别名，比如上面的 saveForm 方法等价于：
```js
axios.post('/user', context.state.test02)
```
完整的请求还应当包括 .then 和 .catch
#### 执行get请求
```js
//给定ID的user创建请求
axios.get('/user?ID=123')
    .then(function (res){
        console.log(res)
    })
    .catch(function (error) {
        console.log(error)
    })

    //上面请求也可以这么写
    axios.get('/user',{
        params:{
            ID:123
        }
    })
    .then(res => {
        console.log(res)
    })
    .catch(error => {
        console.log(error)
    })
```
这两个回调函数都有各自独立的作用域，如果直接在里面访问 this，无法访问到 Vue 实例
这时只要添加一个 .bind(this) 就能解决这个问题
```js
.then(function(res){
  console.log(this.data)
}.bind(this))
```
#### 执行post请求
```js
    axios.post('/user', {
        firstName: 'Fred',
        lastName: 'Flintstone'
    })
    .then(function (response) {
        console.log(response);
    })
    .catch(function (error) {
        console.log(error);
    });
```
#### 执行多个并发请求
```js
function getUserAccount () {
    return axios.get('/user/1234')
}

function getUserPermissions () {
    return axios.get('/user/1234/permissions')
}

axios.all([getUserAccount(),getUserPermissions()])
    .then(axios.spread(function (acct,perms)))
```

#### 并发
处理并发请求的助手函数
```js
axios.all(iterable)
axios.spread(callback)
```
