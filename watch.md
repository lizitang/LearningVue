### vue中的watch
#### 首先确认watch是一个对象(对象有键，有值)
* 可以在数据变化时来执行异步操作
> 键：要监控的那个，比如路由$route或者某个数据的变化
值可以是函数，函数名，也可以是包含选项的对象
选项包括有三个：
hanlder:其值是一个回调函数，监听到变化时应执行的函数
deep：true/false,是否深入监听
immediate：其值是true或false；确认是否以当前的初始值执行handler的函数
```
    watch: {
        '$route': function (to,from) {
            if(to.path == '/activePublic'){
                this.active = true ;
            }else if(to.path == '/activeManage'){
                this.active = false ;
        }
    }
```
监听一个对象
```
watch: {
        info: {  // 这监听对象值的改变 和上面的不一样。
            handler(curVal,oldVal){
                console.log(curVal);
            },
            deep: true, 
        },
    }
    //这里info是data中的一个对象
```