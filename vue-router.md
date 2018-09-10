### vue-router跳转页面
#### 1.编程式导航
实际情况下，有很多按钮在执行跳转之前，还会执行一些方法，这时可以使用
配置 path 的时候，以 " / " 开头的嵌套路径会被当作根路径，所以子路由的 path 不需要添加 " / "
> 可以在main.js中定义一个全局函数$goRoute
```
Vue.prototype.$goRoute = function (index) {
    this.router.push(index)
}
//这样可以直接在标签内单击时调用
@click='$goRoute(item.route)'
```
this.$router.push(location)

```
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
```













//字符串
this.$router.push('/home/first')
//对象
this.$router.push({path:'/home/first'})
//命名的路由
this.$router.push({name:'home',params:{userId:wise}})
```