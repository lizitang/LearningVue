### vue-router跳转页面
#### 引入方式
在我们安装vue-cli的过程中，有提示安装vue-router，所以如果在这个步骤中安装后，就不需要自己手动安装了。
1. 如果在package.json中没有vue-router
> npm install vue-router --save
2. 在入口文件main.js中引入
```javascript
//main.js
import router from './router'; // 路由表

//router->index.js
import Vue from 'vue';
import Router from 'vue-router';

import Apple from '@/components/Apple'
import Banana from '@/components/Banana'' 
Vue.use(Router)//使用
export default  new VRouter({
	routes: [
		{
			path: '/Apple',
			component: Apple
		},
		{
			path: '/Banana',
			component: Banana
		}
	]
}) //实例化
```
实际情况下，有很多按钮在执行跳转之前，还会执行一些方法，这时可以使用配置 path 的时候，以 " / " 开头的嵌套路径会被当作根路径，所以子路由的 path 不需要添加 " / "
+ 可以在main.js中定义一个全局函数$goRoute
```javascript
Vue.prototype.$goRoute = function (index) {
    this.router.push(index)
}
//这样可以直接在标签内单击时调用
@click='$goRoute(item.route)'
```
this.$router.push(location)

```html
<div>
    <button class="register">注册</button>
    <button class="sign" @click="goFirst">注册</button>
</div>

methods:{
    goFirst:function () {
        this.$router.push({path:'/home/first'})
    }
}
```
push后面可以是对象，也可以是字符串
```javascript
//字符串
this.$router.push('/home/first')
//对象
this.$router.push({path:'/home/first'})
//命名的路由
this.$router.push({name:'home',params:{userId:wise}})
```