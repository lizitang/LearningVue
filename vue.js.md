### vue.js
 1.属性和方法（前缀$）
 ```
    var data = {a:1}
    var vm = new Vue({
        el:"#app",
        data:data
    })
    vm.$data === data //->true
    vm.$el === document.getElementById("#app") //->true
 ```    
 ### 实例生命周期
    1.created 这个钩子在实例被创建之后被调用
 ``` 
        var vm = new Vue({
          data: {
            a: 1
          },
          created: function () {
            // `this` 指向 vm 实例
            console.log('a is: ' + this.a)
          }
        })
        // -> "a is: 1"
```
### vue生命周期图（未完）

```
graph TD
A(new Vue)-->B(Observe Data)
B(Observe Data)-->C(Init Events)

```
### 插值
    1.文本
```
 <span>Message:{{msg}}</span>
//大括号将数据解析为文本
#纯文本的话
<div v-html="rawHtml"></div>
```
这个 div 的内容将会被替换成为属性值 rawHtml，直接作HTML数据绑定会被忽略。注意，你不能使用 v-html 来复合局部模板，因为 Vue 不是基于字符串的模板引擎。组件更适合担重用与复合的基本单元
#### 过滤器
可以用在插值和v-bind表达式
```
{{message | capitalize}}
<!-- in v-bind -->
<div v-bind:id="rawID | formatId"></div>
```
过滤器函数总接受表达式的值作为第一个参数。

```
new Vue({
    filters:{
        capitalize : function(value){
            if(!value) return ''
            value = value.toString()
            return value.charAt(0).toUpperCase() + value.slice(1)
        }
    }
})
```
过滤器可以串联，也可以接受参数

```
{{message | filterA | filterB}}
<!--接受参数-->
{{message | filterA('arg1','arg2')}}
```
### v-bind缩写

```
<!--v-bind: 可以缩写为: -->
<!--栗子-->
<a v-bind:href="url"></a>

//缩写
<a :href="url"></a>
```
### v-on缩写

```
<!--栗子-->
<a v-on:click="dosomething"></a>

//缩写
<a @click="dosomething></a>
```
## 计算属性 computed

```
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
```

```
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // a computed getter
    reversedMessage: function () {
      // 'this' points to the vm instance
      return this.message.split('').reverse().join('')
    }
  }
})
```
也可以用methods调用方法来实现

```
<p>Reversed message: "{{ reversedMessage() }}"</p>
```

```
// in component
methods: {
  reversedMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
```



















    
 