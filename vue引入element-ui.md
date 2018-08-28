### vue引入element-ui
#####  1.打开终端找到项目目录---安装element-ui
> npm install element-ui -S
##### 2.src/main.js入口文件，添加
```
    import ElementUI from 'element-ui'
    import 'element-ui/lib/theme-chalk/index.css';

    Vue.use(ElementUI)
```
##### 3.在webpack.config.js文件中
```
    module: {
    rules: [
　　　　......

      {
        test: /\.(eot|svg|ttf|woff|woff2)(\?\S*)?$/,
        loader: 'file-loader'
      },

　　　　......
    ]
  },
```