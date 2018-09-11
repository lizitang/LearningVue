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
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
    state: {
        author: 'lizi'
    }
})

export default new Vuex.Store
```
以上例子只存放一个状态author
* <div style="color:red;font-size:18px">将状态映射到组件</div>
```html
<template>
  <footer class="footer">
    <ul>
      <li v-for="lis in ul">{{lis.li}}</li>
    </ul>
    <p>
      Copyright&nbsp;&copy;&nbsp;{{author}} - 2016 All rights reserved
    </p>
  </footer>
</template>

<script>
  export default {
    name: 'footerDiv',
    data () {
      return {
        ul: [
          { li: '琉璃之金' },
          { li: '朦胧之森' },
          { li: '缥缈之滔' },
          { li: '逍遥之火' },
          { li: '璀璨之沙' }
        ]
      }
    },
    computed: {
      author () {
        return this.$store.state.author
      }
    }
  }
</script>
```
主要在 computed 中，将 this.$store.state.author 的值返回给 html 中的 author
```html
<!--父组件中引入子组件-->
    <template>
    <div>
        <a href="javascript:;" @click="$store.state.show = true">点击</a>
        <t-dialog></t-dialog>
    </div>
    </template>

    <script>
    import dialog from './components/dialog.vue'
    export default {
    components:{
        "t-dialog":dialog
    }
    }
    </script>


<!--子组件-->
    <template>
    <el-dialog :visible.sync="$store.state.show"></el-dialog>
    </template>

    <script>
    export default {}
    </script>
```
### mutations
我们进行一个状态，需要多个依赖，这时候需要用到mutations
```
 export default {
     state:{
         show:false
     },
     mutations: {
         //这里的操作必须是同步的
         switch_dialog(state) {//这里state对应上面state
            state.show = state.show?false:true
         }
     }
 }
```
使用mutations后，原先的父组件可以改为
```
<template>
  <div id="app">
    <a href="javascript:;" @click="$store.commit('switch_dialog')">点击</a>
    <t-dialog></t-dialog>//子组件
  </div>
</template>
```
使用 $store.commit来触发mutations中的switch_dialog方法
这里需要注意的是：
    1.mutations 中的方法必须是同步的

### actions 
多个state操作，使用mutations来触发会比较好维护，那么需要执行多个mutations就需要
用action了

```
export default {
    state :{
        show:false
    },
    mutations :{
        switch_dialog(state) {
            state.show = state.show?false:true;
        }
    },
    actions:{
        switch_dialog(context){//这里的context和我们使用的$store拥有相同的对象和方法
            context.commit('switch_dialog');
            //你还可以在这里触发其他的mutations方法
        },
    }
}
```
那么 , 在之前的父组件中 , 我们需要做修改 , 来触发 action 里的 switch_dialog 方法:
```
    <template>
    <div id="app">
        <a href="javascript:;" @click="$store.dispatch('switch_dialog')">点击</a>
        <t-dialog></t-dialog>
    </div>
    </template>

```
使用 $store.dispatch('switch_dialog') 来触发 action 中的 switch_dialog 方法。

官方推荐 , 将异步操作放在 action 中

### getters
getters 和 vue 中的 computed 类似 , 都是用来计算 state 然后生成新的数据 ( 状态 ) 的。
还是前面的例子 , 假如我们需要一个与状态 show 刚好相反的状态 , 使用 vue 中的 computed 可以这样算出来 :
```
computed(){
    not_show(){
        return !this.$store.state.dialog.show;
    }
}
```
那么 , 如果很多很多个组件中都需要用到这个与 show 刚好相反的状态 , 那么我们需要写很多很多个 not_show , 使用 getters 就可以解决这种问题 :
```
    export default {
    state:{//state
        show:false
    },
    getters:{
        not_show(state){//这里的state对应着上面这个state
            return !state.show;
        }
    },
    mutations:{
        switch_dialog(state){//这里的state对应着上面这个state
            state.show = state.show?false:true;
            //你还可以在这里执行其他的操作改变state
        }
    },
    actions:{
        switch_dialog(context){//这里的context和我们使用的$store拥有相同的对象和方法
            context.commit('switch_dialog');
            //你还可以在这里触发其他的mutations方法
        },
    }
}
```
我们在组件中使用 $store.state.dialog.show 来获得状态 show , 类似的 , 我们可以使用 $store.getters.not_show 来获得状态 not_show 。

注意 : $store.getters.not_show 的值是不能直接修改的 , 需要对应的 state 发生变化才能修改。

###  mapState、mapGetters、mapActions
```
    <template>
    <el-dialog :visible.sync="show"></el-dialog>
    </template>

    <script>
    import {mapState} from 'vuex';
    export default {
    computed:{

        //这里的三点叫做 : 扩展运算符
        ...mapState({
        show:state=>state.dialog.show
        }),
    }
    }
    </script>
```
mapGetters、mapActions 和 mapState 类似 , mapGetters 一般也写在 computed 中 , mapActions 一般写在 methods 中。