## VUE-study
-----------------------
### Vue
-----------------------
#### 插值
1.文本
* v-text
* v-html
```html
<span>Message:{{msg}}</span>
//大括号将数据解析为文本
#纯文本的话
<div v-html="rawHtml"></div>
```
这个 div 的内容将会被替换成为属性值 rawHtml，直接作HTML数据绑定会被忽略。注意，你不能使用 v-html 来复合局部模板，因为 Vue 不是基于字符串的模板引擎。组件更适合担重用与复合的基本单元
* v-bind:属性绑定 v-bind:href="ss" 缩写:href,可以绑定class
```html
<!--v-bind: 可以缩写为: -->
<!--栗子-->
<a v-bind:href="url"></a>

//缩写
<a :href="url"></a>
```
* v-on 事件绑定 缩写 @click
```html
<!--栗子-->
<a v-on:click="dosomething"></a>

//缩写
<a @click="dosomething></a>
```
+ 条件渲染：
  * v-if:如果不满足，就不会存在dom节点中
  * v-show：如果不满足条件，依旧在dom节点中，只是display：none
  v-else 与v-if想法
+ v-model表单数据双向绑定
+ v-for 列表循环
```html
<div v-for="(item,index) in list" :class="{odd:index%2}">{{item.name}}</div>
<div v-for="(value,key) in objList">{{value}}</div>
```
+ 数组更新：
  a: 可以触发视图更新的方法：
    * push()
    * pop()
    * shift()
    * unshift()
    * splice()
    * sort()
    * reverse()

  b:不能触发视图更新的方法
    * concat()
    * slice()
    * filter()
    * 直接利用索引更改某一项
    * 直接修改索引的长度

  c: 针对已经创建的实例，Vue不能直接动态添加根级别的响应式属性，但是可以使用Vue.set(object, key, value)方法向嵌套对象添加响应式属性 
```javascript
    Vue.set(this.list, 1, {
        name: 'orange',
        price: 16
      })
    this.list[1] = {
      name: 'orange',
      price: 16
}//这种方式不可以，可以通过Vue.set()来修改
```
+ ref (string类型)
用来给元素或子组件注册引用信息。引用信息将会注册在父组件\$refs对象上。如果在普通的DOM元素上使用，引用指向的就是DOM元素；如果用在子组件上，引用就指向组件实例：
```html
<!-- vm.$refs.p will be the DOM node -->
 <p ref="p">hello</p>
 <!-- vm.$refs.child will be the child comp instance -->
 <child-comp ref="child"></child-comp>
```
当 v-for 用于元素或组件的时候，引用信息将是包含 DOM 节点或组件实例的数组。

关于 ref 注册时间的重要说明：因为 ref 本身是作为渲染结果被创建的，在初始渲染的时候你不能访问它们 - 它们还不存在！$refs 也不是响应式的，因此你不应该试图用它在模板中做数据绑定。
+ slot (string类型) 作用域插槽
用于标记往哪个具名插槽中插入子组件内容。 子组件里使用就可以把子组件标签里的结构引用到子组件里
### 实例生命周期
![avatar](/images/lifecycle.png)
  1. created 这个钩子在实例被创建之后被调用

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
1.属性和方法（前缀\$）
 ```javascript
    var data = {a:1}
    var vm = new Vue({
        el:"#app",
        data:data
    })
    vm.$data === data //->true
    vm.$el === document.getElementById("#app") //->true
 ```    
 

```
graph TD
A(new Vue)-->B(Observe Data)
B(Observe Data)-->C(Init Events)

```

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
### 子组件索引
为此可以使用 ref 为子组件指定一个索引 ID。例如：
```
<div id="parent">
  <user-profile ref="profile"></user-profile>
</div>
```
```
var parent = new Vue({ el: '#parent' })
// 访问子组件
var child = parent.$refs.profile
```
* Vue 不允许在已经创建的实例上动态添加新的根级响应式属性(root-level reactive property)。然而它可以使用 Vue.set(object, key, value) 方法将响应属性添加到嵌套的对象上：
```
Vue.set(vm.someObject, 'b', 2)
```
或者
```
this.$set(this.someObject,'b',2)
```
* 如果是在已有的对象上添加一些属性，使用Object.assign()或者_.extend()
```
this.someObject = Object.assign({},this.someObject,{a:1,b:2})
```
### 过度效果



















    
 