### vue-router跳转页面
#### 1.编程式导航
实际情况下，有很多按钮在执行跳转之前，还会执行一些方法，这时可以使用
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