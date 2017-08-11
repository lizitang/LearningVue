### 引入bootstrap
1.将使用的文件放在src目录下的assets文件夹中  
2.在入口文件src/main.js中引入bootstrap  

```
import './assets/bootstrap.min.js'
import './assets/bootstrap.min.css'
```
这样就可以在vue项目中直接使用bootstrap样式了
### 引入jquery
#### 1,下载jquery依赖

```
cnpm install jquery --save
```
#### 2.修改配置
1.在build文件夹下的webpack.base.conf.js文件。加入webpack对象

```
var webpack=require('webpack');
```
2.在webpack.base.conf.js在下方module.exports对象里面加入

```
Plugins:[
 new webpack.ProvidePlugin({
    $:'jquery',
    jQuery:'jquery',
    jquery:'jquery',
    'window.jQuery':'jquery'
 })
]
```
### 使用jq插件
只需要把文件放在src/assets里面或者直接放在static里面。然后将插件的文件引用进组件

```
<script>
import '../static/slick/slick.min'
export default {
    name: 'app',
    methods: {
        sliderFn () {
            //和平时用的插件差不多
        }
    }，
    mouted () {
        this.text()
        this.sliderFn();//组件加载完后自动运行的钩子
    }
}
</script>
<style>
//引入css 
</style>
```




