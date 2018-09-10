### Vue.extend(options)
* 参数： {object} options
用法：
使用基础vue构造器，创建一个‘子类’。参数是一个包含组件选项的对象
data选项是特例，需要注意，在Vue.extend()中它必须是函数
```
<div id="mount-point"></div>
```
```
//创建构造器
var Profile = Vue.extend({
    template: '<p>{{firstName}} {{lastName}} aka {{alias}}</p>',
    data: function () {
    return {
      firstName: 'Walter',
      lastName: 'White',
      alias: 'Heisenberg'
    }
  }
})

// 创建 Profile 实例，并挂载到一个元素上。
new Profile().$mount('#mount-point')
```
### Vue.nextTick([callback,context])
* 参数：
    {Function} [callback]
    {Object} [context]
* 用法：
在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。
```
// 修改数据
vm.msg = 'Hello'
// DOM 还没有更新
Vue.nextTick(function () {
  // DOM 更新了
})

// 作为一个 Promise 使用 (2.1.0 起新增，详见接下来的提示)
Vue.nextTick()
  .then(function () {
    // DOM 更新了
  })
```