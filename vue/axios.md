## 概念

Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。功能和Ajax一样

## get请求

```js
// 为给定 ID 的 user 创建请求
axios.get('/user?ID=12345')
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });

// 上面的请求也可以这样做
axios.get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
//也可以这样
axios({
        method: 'get',
        url: url,
        params: {
            ID: 12345
        }
    })
```

## post请求

```js
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

axios({
        method: 'post',
        url: url,
        data: {
            firstName: 'Fred',
            lastName: 'Flintstone'
  		}
    })
```

delete和put请求省略

## 全局的 axios 默认值

```js
axios.defaults.baseURL = 'https://api.example.com';
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';

```

## 拦截器

- 比如config中的一些信息不符合服务器的要求，得及时拦截下来更改。
- 比如每次发送网络请求的时候，都希望在界面中显示一个动态加载的请求图标，就是一直在转圈圈，让用户知道当前页面正在加载数据，准备渲染视图。
- 比如请求失败的弹窗
- 比如某些网络请求（比如登录token）,必须携带一些特殊的信息。

```js
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error);
  });
```

