### vue-resource
在实际项目中，都需要从后端获取数据，以此来渲染前端页面
#### 1.安装
> npm install vue-resource -D
vue-resource是一个Vue插件，在安装完成后需要在main.js文件内载入这个插件代码如下
所示：
```
import Vue from 'vue'
import VueResource from 'vue-resource'

Vue.use(VueResource)
```
对于那些不能处理REST/HTTP请求方法的老旧浏览器，vue-resouce可以代开emulateHTTP开
关，已取得兼容性的支持:
> Vue.http.options.emulateHTTP = true

```
export default {
    created () {
        this.$http.get('/home').then(res => {
            this.annoucement = res.body.annoucement
            this.slides = res.body.slides
            this.recommended = res.body.recommeded
        })
    }
}
```
可以让代码变得更简洁一点
```
this.$http.get('/home')
        .then(res => {
            for prop in res.body {
                this[prop] = res.body[prop]
            }.(error) => {
                console.log(')
            }
        }
```
#### $http对象上提供了以下安装方法
get，head，delete，jsonp,post,put,patch
