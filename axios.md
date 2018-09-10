### 安装
使用npm
```
npm install axios
```
### example
#### 执行get请求
```
//给定ID的user创建请求
axios.get('/user?ID=123')
    .then(function (res){
        console.log(res)
    })
    .catch(function (error) {
        console.log(error)
    })

    //上面请求也可以这么写
    axios.get('/user',{
        params:{
            ID:123
        }
    })
    .then(res => {
        console.log(res)
    })
    .catch(error => {
        console.log(error)
    })
```

#### 执行post请求
```
    axios.post('/user', {
        firstName: 'Fred',
        lastName: 'Flintstone'
    })
    .then(function (response) {
        console.log(response);
    })
    .catch(function (error) {
        console.log(error);
    });
```
#### 执行多个并发请求
```
function getUserAccount () {
    return axios.get('/user/1234')
}

function getUserPermissions () {
    return axios.get('/user/1234/permissions')
}

axios.all([getUserAccount(),getUserPermissions()])
    .then(axios.spread(function (acct,perms)))
```

#### 并发
处理并发请求的助手函数
```
axios.all(iterable)
axios.spread(callback)
```
