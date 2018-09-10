### require.context
最近在做一个vue的项目，用到vuex状态管理，在store文件夹下index.js文件中用到了，拿来百度了一下
```
    const storeArry = {};

    function importAll(r) {
    r.keys().forEach((key) => {
        let name = key.replace(/(^\.\/)|(\.js$)/g, '');
        storeArry[name] = r(key).default;
    });
    }

    importAll(require.context('./modules/', true, /\.js$/));

    Vue.use(Vuex);

    const store = new Vuex.Store({
    modules: {
        ...storeArry,
    },
    getters,
    });

    export default store;
```
可以通过正则匹配引入相应的文件模块
> require.context() 方法来创建自己的（模块）上下文，这个方法有 3 个参数：要搜索的文件夹目录，是否还应该搜索它的子目录，以及一个匹配文件的正则表达式
```
require.context('./modules/', true, /\.js$/);
```