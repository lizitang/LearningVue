## VUE-study
### Vue
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
有事件修改器：例如v-on:click.stop就是阻止事件冒泡
```html
@keydown.enter="onKeydown"
@keydown.13="onKeydown"
<!--栗子-->
<a v-on:click="dosomething"></a>

//缩写
<a @click="dosomething></a>
```
* 自定义事件
@my-event="onMyevent"
+ 条件渲染：
  * v-if:如果不满足，就不会存在dom节点中
  * v-show：如果不满足条件，依旧在dom节点中，只是display：none
  v-else 与v-if一起用
+ v-model表单数据双向绑定
```
<input type="text" v-model="msg">
//修饰符
v-model.lazy="msg" --->彻底完成之后，blur之后才会更新
v-model.number
v-model.trim 裁剪掉前后空格
```
Vue中的控件，CheckBox,radio都是通过v-model
```
<input type="checkbox" v-model="msg">
```
select 同样也是但是绑定在select上面
+ DOM模板解析说明

像这些元素 ul,ol，table，select限制了能被它包裹的元素，而一些像 option这样的元素只能出现在某些其它元素内部。
```
//这样使用
<table>
    <tr is='my-row'>
</table>
```
#### 应当注意，如果您使用来自以下来源之一的字符串模板，这些限制将不适用：
```
<script type="text/x-template">
```
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
### 生命周期
![avatar](/images/lifecycle.png)
1. 生命周期过程中，首先创建vue实例，然后初始化事件并监测数据。在created阶段之前就完成了数据的监测(这个节点data和data的属性已存在，但是el未初始化)
2. created函数触发后，开始初始化el,并且编译template。在created和beforeMount这两个阶段之间发生的事情比较多：
(1)首先判断对象是否有el选项，有的话向下编译，没有的话，则停止编译，也就意味着停止了生命周期，直到在该vue实例上调用vm.$mout(el)
(2)template 参数选项是否对生命周期有影响：
-如果vue实例对象中有template参数选项，则将其作为模板编译成render函数
-如果没有template选项，则将外部HTML作为模板编译
+ <div style="color:red;font-size:18px">beforeCreate ：</div>组建实例化刚被创建，该过程在组件属性计算之前，在这个时候data和el,以及自定义的message都是undefined
+ <div style="color:red;font-size:18px">created ：</div>组件实例化创建完成，属性已绑定，但是dom还未生成，$el属性还不存在,在这个时候data和message已经初始化了
+ <div style="color:red;font-size:18px">beforeMount ：</div>模板编译/挂载之前 $el和data、message都被初始化了
+ <div style="color:red;font-size:18px">Mounted ：</div>模板编译/挂载之后 $el和data、message都被初始化了
+ <div style="color:red;font-size:18px">beforeUpdate ：</div>组件更新之前
+ <div style="color:red;font-size:18px">updated ：</div>组件更新之后
+ <div style="color:red;font-size:18px">activated ：</div>keep-alive,组件激活时被调用
+ <div style="color:red;font-size:18px">deactivated ：</div>keep-alive,组件移除时被调用
+ <div style="color:red;font-size:18px">beforeDestory ：</div>组件销毁前被调用
+ <div style="color:red;font-size:18px">destory ：</div>组件销毁后被调用
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>vue生命周期</title>
	<script type="text/javascript" src="https://cdn.jsdelivr.net/vue/2.1.3/vue.js"></script>
</head>
<body>
	<div id="app">
		<p>{{message}}</p>
		<button v-on:click="change">点击改变值</button>
	</div>
	<script type="text/javascript">
		new Vue({
			el: '#app',
			data: {
				message: 'hello'
			},
			methods: {
				change: function () {
					this.message = this.message.split('').reverse().join('');
				}
			},
			beforeCreate: function () {
	            console.group('beforeCreate 创建前状态===============》');
	            console.log("%c%s", "color:red" , "el     : " + this.$el); //undefined
	            console.log("%c%s", "color:red","data   : " + this.$data); //undefined 
	            console.log("%c%s", "color:red","message: " + this.message)  
        	},
        	created: function () {
            	console.group('created 创建完毕状态===============》');
            	console.log("%c%s", "color:red","el     : " + this.$el); //undefined
                console.log("%c%s", "color:red","data   : " + this.$data); //已被初始化 
                console.log("%c%s", "color:red","message: " + this.message); //已被初始化
	        },
	        beforeMount: function () {
	            console.group('beforeMount 挂载前状态===============》');
	            console.log("%c%s", "color:red","el     : " + (this.$el)); //已被初始化
	            console.log(this.$el);
	            console.log("%c%s", "color:red","data   : " + this.$data); //已被初始化  
	            console.log("%c%s", "color:red","message: " + this.message); //已被初始化  
	        },
         	mounted: function () {
            	console.group('mounted 挂载结束状态===============》');
            	console.log("%c%s", "color:red","el     : " + this.$el); //已被初始化
            	console.log(this.$el);    
                console.log("%c%s", "color:red","data   : " + this.$data); //已被初始化
                console.log("%c%s", "color:red","message: " + this.message); //已被初始化 
        	},
        	beforeUpdate: function () {
            	console.group('beforeUpdate 更新前状态===============》');
            	console.log("%c%s", "color:red","el     : " + this.$el);
            	console.log(this.$el);   
               	console.log("%c%s", "color:red","data   : " + this.$data); 
               	console.log("%c%s", "color:red","message: " + this.message); 
        	},
        	updated: function () {
            	console.group('updated 更新完成状态===============》');
           		console.log("%c%s", "color:red","el     : " + this.$el);
            	console.log(this.$el); 
               	console.log("%c%s", "color:red","data   : " + this.$data); 
               	console.log("%c%s", "color:red","message: " + this.message); 
        	},
        	beforeDestroy: function () {
            	console.group('beforeDestroy 销毁前状态===============》');
            	console.log("%c%s", "color:red","el     : " + this.$el);
            	console.log(this.$el);    
               	console.log("%c%s", "color:red","data   : " + this.$data); 
               	console.log("%c%s", "color:red","message: " + this.message); 
        	},
        	destroyed: function () {
            	console.group('destroyed 销毁完成状态===============》');
            	console.log("%c%s", "color:red","el     : " + this.$el);
            	console.log(this.$el);  
               	console.log("%c%s", "color:red","data   : " + this.$data); 
               	console.log("%c%s", "color:red","message: " + this.message)
        	}
		})
	</script>
</body>
</html>
```
* mounted和beforeMount这两种情况$el和data、message都被初始化了，但是其有一定的区别，beforeMount时应用虚拟Dom先把坑粘住了。mounted是挂载后的情况，在该阶段的时候，把值渲染进去。在beforeUpdate,可以监听到data的变化但是view层没有被重新渲染，view层的数据没有变化。等到updated的时候 view层才被重新渲染，数据更新。如下图所示。
![avatar](/images/vue.png)
#### 过滤器
可以用在插值和v-bind表达式
```html
{{message | capitalize}}
<!-- in v-bind -->
<div v-bind:id="rawID | formatId"></div>
```
过滤器函数总接受表达式的值作为第一个参数。

```javascript 
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

```html
{{message | filterA | filterB}}
<!--接受参数-->
{{message | filterA('arg1','arg2')}}
```
## 计算属性 computed

```html
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
```

```javascript
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

```html
<p>Reversed message: "{{ reversedMessage() }}"</p>
```

```javascript
// in component
methods: {
  reversedMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
```
### 子组件索引
为此可以使用 ref 为子组件指定一个索引 ID。例如：
```html
<div id="parent">
  <user-profile ref="profile"></user-profile>
</div>
```
```javascript
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
## Vue-cli
### 优点：
1. 成熟的vue项目架构设计
2. 本地测试服务器
3. 集成打包上线的方案
### 安装
> npm install vue-cli -g
vue init webpack my-project
npm install 安装依赖
npm run dev 在本地启动测试服务器
npm run build 生成上线目录部署
 ### router-link
 router-link组件支持用户在具有路由功能的应用中，通过to属性指定目标地址，默认渲染成带有正确链接的a标签，可以通过配置tag属性生成别的标签。
 ```html
 <!-- 字符串 -->
<router-link to="home">Home</router-link>
<!-- 渲染结果 -->
<a href="home">Home</a>

<!-- 使用 v-bind 的 JS 表达式 -->
<router-link v-bind:to="'home'">Home</router-link>

<!-- 不写 v-bind 也可以，就像绑定别的属性一样 -->
<router-link :to="'home'">Home</router-link>

<!-- 同上 -->
<router-link :to="{ path: 'home' }">Home</router-link>

<!-- 命名的路由 -->
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>

<!-- 带查询参数，下面的结果为 /register?plan=private -->
<router-link :to="{ path: 'register', query: { plan: 'private' }}">Register</router-link>
 ```
[具体用法点击这里](https://router.vuejs.org/zh/api/)
## 动画 transition
### props
1. name 字符串型 用于自动生成css过渡类名,假设起名字加name="fade"，自动拓展为.fade-enter,.fase-enter-active等，默认name为v
2. appear 布尔型，是否在初始渲染时使用过渡，默认为false
3. css 布尔型 是否使用css过渡类，默认为true，如果设置为false，将只能通过组件事件触发注册的javascript钩子
4. type 字符串型，过渡事件类型,transition或者animation
5. mode 字符串型 控制离开/进入的过渡时间序列,'out-in'和'in-out'
### 过度的类名
在进入/离开的过渡中，有6个class切换
1. v-enter: 进入过渡的开始状态，在元素被插入时生效
2. v-enter-active: 定义过渡的状态，元素被插入时生效，在transition/animation完成之后移除
3. v-enter-to: 定义过渡结束的状态
4. v-leave: 离开过渡的开始状态，离开过渡被触发的时生效
5. v-leave-active: 离开过渡被触发后生效，在transition/animation完成之后移除
6. v-leave-to:离开过渡的结束状态
![avatar](/images/transition.jpeg)

## v-on 事件修饰符
在事件处理程序中调用event。preventDefault()或event.stopPropagation()是非常常见的需求。尽管我们可以在方法中轻松实现这点，但更好的方式是：方法只有纯粹的数据逻辑，而不是去吃力DOM事件细节
+ .stop:阻止事件传播
+ .prevent v-on:submit.prevent阻止重新加载页面
+ .capture v-on:cllick.capture 元素自身触发的事件先处理，再交由其内部元素进一步处理
+ .self v-on:click.self 只当在 event.target 是当前元素自身时触发处理函数
+ .passive 不阻止浏览器的默认行为

















    
 